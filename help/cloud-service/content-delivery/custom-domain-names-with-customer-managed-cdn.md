---
title: Benutzerspezifischer Domänenname mit kundenverwaltetem CDN
description: Erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen auf die AEM as a Cloud Service-Website implementieren, die ein kundenverwaltetes CDN verwendet.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15945
thumbnail: KT-15945.jpeg
source-git-commit: 07225f1ae4455e2fa69c8e488851361c725fe9e8
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 29%

---

# Benutzerspezifischer Domänenname mit kundenverwaltetem CDN

Erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen zu einer AEM as a Cloud Service-Website hinzufügen, die ein **kundenverwaltetes CDN** verwendet.

In diesem Tutorial wird das Branding der Beispiel-Website [AEM WKND](https://github.com/adobe/aem-guides-wknd) verbessert, indem mithilfe eines kundenverwalteten CDN ein HTTPS-adressierbarer benutzerdefinierter Domänenname `wkndviaawscdn.enablementadobe.com` mit Transport Layer Security (TLS) hinzugefügt wird. In diesem Tutorial wird AWS CloudFront als kundenverwaltetes CDN verwendet. Jeder CDN-Anbieter sollte jedoch mit AEM as a Cloud Service kompatibel sein.

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

Die allgemeinen Schritte lauten wie folgt:

![Benutzerdefinierter Domänenname mit Kunden-CDN](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}

## Voraussetzungen

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) und [dig](https://www.isc.org/blogs/dns-checker/) sind auf Ihrem lokalen Computer installiert.
- Es besteht Zugang zu Drittanbieterdiensten:
   - Zertifizierungsstelle (ZS) – zum Anfordern des signierten Zertifikats für Ihre Sitedomain, z. B. [DigitCert](https://www.digicert.com/)
   - Kunden-CDN : Zum Einrichten des Kunden-CDN und Hinzufügen von SSL-Zertifikaten und Domänendetails wie AWS CloudFront, Azure CDN oder Akamai.
   - Domain Name System(DNS)-Hosting-Dienst – zum Hinzufügen von DNS-Einträgen für Ihre benutzerdefinierte Domain, z. B. Azure DNS oder AWS Route 53
- Zugriff auf [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) , um die HTTP-Header-Validierungs-CDN-Regel in der AEM as a Cloud Service-Umgebung bereitzustellen.
- Beispiel-Site [AEM WKND](https://github.com/adobe/aem-guides-wknd) wird in der AEM as a Cloud Service-Umgebung des Typs [Produktionsprogramm](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) bereitgestellt.

Wenn Sie keinen Zugang zu Drittanbieterdiensten haben, _führen Sie diese Schritte zusammen mit Ihrem Sicherheits- oder Hostingteam aus_.

## Generieren eines SSL-Zertifikats

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Es gibt zwei Optionen:

1. Mit dem Befehlszeilen-Tool `openssl` können Sie einen privaten Schlüssel und einen Certificate Signing Request (CSR) für Ihre Sitedomain generieren. Um ein signiertes Zertifikat anzufordern, senden Sie die CSR an eine Zertifizierungsstelle (Certificate Authority, CA).
1. Ihr Hostingteam stellt den erforderlichen privaten Schlüssel und das signierte Zertifikat für Ihre Site bereit.

Sehen wir uns die Schritte für die erste Option an.

Um einen privaten Schlüssel und einen CSR zu generieren, führen Sie die folgenden Befehle aus und geben Sie die erforderlichen Informationen ein, wenn Sie dazu aufgefordert werden:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Um ein signiertes Zertifikat anzufordern, stellen Sie der ZS den generierten CSR bereit, indem Sie die zugehörige Dokumentation befolgen. Nachdem die ZS den CSR signiert hat, erhalten Sie die signierte Zertifikatsdatei.

### Überprüfen des signierten Zertifikats

Sie sollten das signierte Zertifikat überprüfen, bevor Sie es zu Cloud Manager hinzufügen. Um sich die Details zum Zertifikat anzusehen, können Sie den folgenden Befehl verwenden:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

Das signierte Zertifikat kann die Zertifikatskette enthalten, die die Stamm- und Zwischenzertifikate zusammen mit dem Anwenderzertifikat (auch als End-Entity-Zertifikat bezeichnet) umfasst.

Adobe Cloud Manager akzeptiert das Anwenderzertifikat und die Zertifikatskette _in separaten Formularfeldern_, sodass Sie das Anwenderzertifikat und die Zertifikatskette aus dem signierten Zertifikat extrahieren müssen.

In diesem Tutorial wird das [DigitCert](https://www.digicert.com/)-signierte Zertifikat, das für die Domain `*.enablementadobe.com` ausgestellt wurde, als Beispiel verwendet. Das Anwenderzertifikat und die Zertifikatskette werden extrahiert, indem das signierte Zertifikat in einem Texteditor geöffnet und der Inhalt zwischen den Markern `-----BEGIN CERTIFICATE-----` und `-----END CERTIFICATE-----` kopiert wird.

## Einrichten eines kundenverwalteten CDN

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

Richten Sie das Kunden-CDN wie AWS CloudFront, Azure CDN oder Akamai ein und fügen Sie das SSL-Zertifikat und die Domänendetails hinzu. In diesem Tutorial wird AWS CloudFront als Beispiel verwendet. Je nach CDN-Anbieter können die Schritte jedoch unterschiedlich sein. Die wichtigsten Legenden sind:

- Fügen Sie das SSL-Zertifikat zum CDN hinzu.
- Fügen Sie den benutzerdefinierten Domänennamen zum CDN hinzu.
- Konfigurieren Sie das CDN, um Inhalte wie Bilder, CSS- und JavaScript-Dateien zwischenzuspeichern.
- Fügen Sie den HTTP-Header `X-Forwarded-Host` zu den CDN-Einstellungen hinzu, damit Ihr CDN diesen Header in allen Anforderungen einbezieht, die an die AEMCD-Herkunft gesendet werden.
- Stellen Sie sicher, dass der Kopfzeilenwert `Host` auf die standardmäßige AEM as a Cloud Service-Domäne eingestellt ist, die die Programm- und Umgebungs-ID enthält und mit `adobeaemcloud.com` endet. Der HTTP-Host-Header-Wert, der vom Kunden-CDN an das Adobe-CDN übergeben wird, muss die standardmäßige AEM as a Cloud Service-Domäne sein. Jeder andere Wert führt zu einem Fehlerstatus.

## DNS-Einträge konfigurieren

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

Gehen Sie wie folgt vor, um den DNS-Eintrag für Ihre benutzerdefinierte Domain zu konfigurieren:

1. Fügen Sie einen CNAME-Eintrag für die benutzerdefinierte Domäne hinzu, der auf den CDN-Domänennamen verweist.

In diesem Tutorial wird Azure DNS für die benutzerdefinierte Domäne `wkndviaawscdn.enablementadobe.com` mit einem CNAME-Eintrag versehen und auf den Verteilungsdomänennamen AWS CloudFront verweisen.

### Verifizieren der Site

Überprüfen Sie den Namen der benutzerdefinierten Domäne, indem Sie mithilfe des benutzerdefinierten Domänennamens auf die Site zugreifen.
Abhängig von der vhhost-Konfiguration in der AEM as a Cloud Service-Umgebung funktioniert sie möglicherweise nicht.

Ein wichtiger Sicherheitsschritt besteht darin, die CDN-Regel für die HTTP-Header-Überprüfung in der AEM as a Cloud Service-Umgebung bereitzustellen. Die Regel stellt sicher, dass die Anfrage vom Kunden-CDN stammt und nicht von einer anderen Quelle.

## Aktueller Arbeitsstatus ohne CDN-Regel zur HTTP-Header-Überprüfung

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

Ohne die HTTP-Header-Validierungs-CDN-Regel wird der Header-Wert `Host` auf die standardmäßige AEM as a Cloud Service-Domäne gesetzt, die die Programm- und Umgebungs-ID enthält und mit `adobeaemcloud.com` endet. Adobe CDN wandelt den Header-Wert `Host` nur dann in den Wert des vom Kunden-CDN empfangenen `X-Forwarded-Host` um, wenn die HTTP-Header-Validierungs-CDN-Regel bereitgestellt ist. Andernfalls wird der Header-Wert `Host` unverändert an die AEM as a Cloud Service-Umgebung übergeben und der Header `X-Forwarded-Host` wird nicht verwendet.

### Beispiel-Servlet-Code zum Drucken des Host-Header-Werts

Der folgende Servlet-Code druckt die HTTP-Header-Werte `Host`, `X-Forwarded-*`, `Referer` und `Via` in der JSON-Antwort.

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

Um das Servlet zu testen, aktualisieren Sie die Datei `../dispatcher/src/conf.dispatcher.d/filters/filters.any` mit der folgenden Konfiguration. Stellen Sie außerdem sicher, dass das CDN so konfiguriert ist, dass der Pfad `/bin/*` **NICHT zwischengespeichert** wird.

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## CDN-Regel zur HTTP-Header-Überprüfung konfigurieren und bereitstellen

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

Gehen Sie wie folgt vor, um die HTTP-Header-Validierungs-CDN-Regel zu konfigurieren und bereitzustellen:

- Fügen Sie die HTTP-Header-Validierungs-CDN-Regel in die Datei `cdn.yaml` ein. Ein Beispiel finden Sie unten.

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
  envTypes: ["prod"]
  data:
  authentication:
      authenticators:
      - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
      - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
          type: authenticate
          authenticator: edge-auth
  ```

- Erstellen Sie mit der Cloud Manager-Benutzeroberfläche geheime Umgebungsvariablen (CDN_EDGEKEY_080124, CDN_EDGEKEY_110124).
- Stellen Sie die HTTP-Header-Validierungs-CDN-Regel mithilfe der Cloud Manager-Pipeline in der AEM as a Cloud Service-Umgebung bereit.

## Geheimschlüssel im HTTP-Header &quot;X-AEM-Edge-Schlüssel&quot;

>[!VIDEO](https://video.tv.adobe.com/v/3432567?quality=12&learn=on)

Aktualisieren Sie das Kunden-CDN, um das Geheimnis in der HTTP-Kopfzeile `X-AEM-Edge-Key` zu übergeben. Das Geheimnis wird vom Adobe-CDN verwendet, um zu überprüfen, ob die Anfrage vom Kunden-CDN stammt, und den Kopfzeilenwert `Host` in den Wert des vom Kunden-CDN empfangenen `X-Forwarded-Host` umzuwandeln.

## Vollständiges Video

Sie können sich auch das durchgängige Video ansehen, in dem die oben beschriebenen Schritte zum Hinzufügen eines benutzerdefinierten Domänennamens mit einem kundenverwalteten CDN zu einer von AEM as a Cloud Service gehosteten Site beschrieben werden.

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
