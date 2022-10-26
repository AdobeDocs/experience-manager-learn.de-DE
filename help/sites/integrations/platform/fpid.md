---
title: Generieren von Adobe Experience Platform-FPIDs mit AEM
description: Erfahren Sie, wie Sie Adobe Experience Platform FPID-Cookies mithilfe von AEM generieren oder aktualisieren.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
source-git-commit: aeeed85ec05de9538b78edee67db4d632cffaaab
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# Generieren von Experience Platform-FPIDs mit AEM

Für die Integration von Adobe Experience Manager (AEM) in Adobe Experience Platform (AEP) ist es erforderlich, dass AEM ein eindeutiges Erstanbieter-Geräte-ID-Cookie (FPID) generieren und verwalten, um die Benutzeraktivität eindeutig zu verfolgen.

Lesen Sie die entsprechende Dokumentation unter [Erfahren Sie mehr über die Zusammenarbeit von Erstanbieter-Geräte-IDs und Experience Cloud-IDs](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

Nachstehend finden Sie einen Überblick darüber, wie FPIDs bei der Verwendung von AEM als Webhost funktionieren.

![FPID und ECIDs mit AEM](./assets/aem-platform-fpid-architecture.png)

## FPID mit AEM generieren und beibehalten

Der AEM-Veröffentlichungsdienst optimiert die Leistung, indem Anforderungen so oft wie möglich zwischengespeichert werden, sowohl im CDN- als auch im AEM Dispatcher-Cache.

Es ist zwingend erforderlich, dass HTTP-Anforderungen, die das Unique-per-User-FPID-Cookie generieren und den FPID-Wert zurückgeben, nie zwischengespeichert und direkt von AEM Publish bereitgestellt werden. Dadurch kann Logik implementiert werden, um Eindeutigkeit zu gewährleisten.

Vermeiden Sie das Generieren des FPID-Cookies in Anforderungen für Web-Seiten oder andere zwischenspeicherbare Ressourcen, da die Kombination aus der eindeutigen Anforderung der FPID diese Ressourcen unerreichbar machen würde.

Im folgenden Diagramm wird beschrieben, wie der AEM Publish-Dienst FPIDs verwaltet.

![FPID und Flussdiagramm AEM](./assets/aem-fpid-flow.png)

1. Der Webbrowser stellt eine Anforderung für eine von AEM gehostete Webseite. Die Anforderung kann mithilfe einer zwischengespeicherten Kopie der Webseite aus dem CDN- oder AEM Dispatcher-Cache verarbeitet werden.
1. Wenn die Webseite nicht über CDN oder AEM Dispatcher-Caches bedient werden kann, erreicht die Anforderung den AEM-Veröffentlichungsdienst, der die angeforderte Webseite generiert.
1. Die Webseite wird dann an den Webbrowser zurückgegeben und die Caches gefüllt, die die Anforderung nicht bedienen konnten. Bei AEM erwarten Sie, dass die Cache-Trefferraten von CDN und AEM Dispatcher über 90 % liegen.
1. Die Webseite enthält JavaScript, das eine nicht speicherbare asynchrone XHR-Anfrage (AJAX) an ein benutzerdefiniertes FPID-Servlet im AEM Publish-Dienst sendet. Da es sich um eine nicht speicherbare Anforderung handelt (aufgrund des zufälligen Abfrageparameters und der Cache-Control-Header), wird sie nie vom CDN oder AEM Dispatcher zwischengespeichert und erreicht immer den AEM-Veröffentlichungsdienst, um die Antwort zu generieren.
1. Das benutzerdefinierte FPID-Servlet im AEM Publish-Dienst verarbeitet die Anforderung und generiert eine neue FPID, wenn kein vorhandenes FPID-Cookie gefunden wird, oder verlängert die Lebensdauer eines vorhandenen FPID-Cookies. Das Servlet gibt auch die FPID im Antworttext für die Verwendung durch clientseitiges JavaScript zurück. Glücklicherweise ist die benutzerdefinierte FPID-Servlet-Logik einfach und verhindert, dass diese Anfrage die Leistung des AEM-Veröffentlichungsdienstes beeinträchtigt.
1. Die Antwort für die XHR-Anfrage wird mit dem FPID-Cookie und der FPID als JSON im Antworttext zur Verwendung durch das Platform Web SDK an den Browser zurückgegeben.

## Code-Beispiel

Der folgende Code und die folgende Konfiguration können im AEM Publish-Dienst bereitgestellt werden, um einen Endpunkt zu erstellen, der ein vorhandenes FPID-Cookie generiert oder verlängert und die FPID als JSON zurückgibt.

### AEM FPID-Cookie-Servlet

Es muss ein AEM HTTP-Endpunkt erstellt werden, um mithilfe eines [Sling-Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ Das Servlet ist an `/bin/aem/fpid` da für den Zugriff keine Authentifizierung erforderlich ist. Wenn eine Authentifizierung erforderlich ist, binden Sie an einen Sling-Ressourcentyp.
+ Das Servlet akzeptiert HTTP-GET-Anfragen. Die Antwort ist mit `Cache-Control: no-store` , um die Zwischenspeicherung zu verhindern. Dieser Endpunkt sollte jedoch auch mithilfe eindeutiger Cache-Busting-Abfrageparameter angefordert werden.

Wenn eine HTTP-Anforderung das Servlet erreicht, prüft das Servlet, ob in der Anfrage ein FPID-Cookie vorhanden ist:

+ Wenn ein FPID-Cookie vorhanden ist, verlängern Sie die Lebensdauer des Cookies und sammeln Sie seinen Wert, um in die Antwort zu schreiben.
+ Wenn kein FPID-Cookie vorhanden ist, generieren Sie ein neues FPID-Cookie und speichern Sie den Wert, der in die Antwort geschrieben werden soll.

Das Servlet schreibt dann die FPID als JSON-Objekt in die Antwort im Formular: `{ fpid: "<FPID VALUE>" }`.

Es ist wichtig, die FPID dem Client im Hauptteil bereitzustellen, da das FPID-Cookie markiert ist `HttpOnly`, was bedeutet, dass nur der Server seinen Wert lesen kann und clientseitiges JavaScript dies nicht kann.

Der FPID-Wert aus dem Antworttext wird verwendet, um Aufrufe mithilfe des Platform Web SDK zu parametrisieren.

Nachstehend finden Sie einen Beispielcode für einen Servlet-Endpunkt AEM (verfügbar über `HTTP GET /bin/aep/fpid`), das ein FPID-Cookie generiert oder aktualisiert und die FPID als JSON zurückgibt.

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### HTML-Skript

Ein benutzerdefiniertes clientseitiges JavaScript muss zur Seite hinzugefügt werden, um das Servlet asynchron aufzurufen, das FPID-Cookie zu generieren oder zu aktualisieren und die FPID in der Antwort zurückzugeben.

Dieses JavaScript-Skript wird der Seite mit einer der folgenden Methoden hinzugefügt:

+ [Tags in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM Client Library](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

Der XHR-Aufruf an das benutzerdefinierte AEM-FPID-Servlet ist schnell, wenn auch asynchron, sodass ein Benutzer eine von AEM bereitgestellte Webseite besuchen und wegnavigieren kann, bevor die Anfrage abgeschlossen werden kann.
In diesem Fall wird der gleiche Prozess beim nächsten Laden einer Webseite von AEM erneut versucht.

Die HTTP-GET zum AEM FPID-Servlet (`/bin/aep/fpid`) mit einem zufälligen Abfrageparameter parametrisiert wird, um sicherzustellen, dass jede Infrastruktur zwischen dem Browser und dem AEM-Veröffentlichungsdienst die Antwort der Anfrage nicht zwischenspeichert.
Entsprechend wird die `Cache-Control: no-store` -Anfragekopfzeile wird hinzugefügt, um die Zwischenspeicherung zu vermeiden.

Bei einem Aufruf des AEM FPID-Servlets wird die FPID aus der JSON-Antwort abgerufen und von der [Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) , um sie an Experience Platform-APIs zu senden.

Weitere Informationen finden Sie in der Dokumentation zur Experience Platform . [Verwenden von FPIDs in identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### Dispatcher-Zulassungsfilter

Schließlich müssen HTTP-GET-Anfragen an das benutzerdefinierte FPID-Servlet über AEM Dispatcher zugelassen werden. `filter.any` Konfiguration.

Wenn diese Dispatcher-Konfiguration nicht korrekt implementiert ist, fordert der HTTP-GET Folgendes an: `/bin/aep/fpid` ergibt 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platformen

Lesen Sie die folgende Experience Platform-Dokumentation für Erstanbieter-Geräte-IDs (FPIDs) und verwalten Sie Identitätsdaten mit dem Platform Web SDK.

+ [Erstanbieter-Geräte-IDs generieren](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [Erstanbieter-Geräte-IDs im Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Identitätsdaten im Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)


