---
title: Entwickeln für CORS (Cross-Origin Resource Sharing) mit AEM
description: Ein kurzes Beispiel für die Nutzung von CORS zum Zugriff auf AEM-Inhalte von einer externen Web-Anwendung über Client-seitiges JavaScript.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 1114ec01555baa1c6ffc2ccc5e77165ec9827e4d
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 69%

---

# Entwickeln für CORS (Cross-Origin Resource Sharing)

Ein kurzes Beispiel für die Nutzung von [!DNL CORS] zum Zugriff auf AEM-Inhalte von einer externen Web-Anwendung über Client-seitiges JavaScript. In diesem Beispiel wird die CORS OSGi-Konfiguration verwendet, um den CORS-Zugriff auf AEM zu aktivieren. Der OSGi-Konfigurationsansatz ist möglich, wenn:

* Eine Quelle greift auf AEM Inhalt veröffentlichen zu
* CORS-Zugriff ist für AEM Author erforderlich

Wenn der Zugriff auf AEM Veröffentlichung mit mehreren Quellen erforderlich ist, lesen Sie den Abschnitt [diese Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

In diesem Video:

* **www.example.com** ist localhost über `/etc/hosts` zugeordnet.
* **aem-publish.local** ist localhost über `/etc/hosts` zugeordnet.
* SimpleHTTPServer (eine Umschließung für SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) in [[!DNL Python]) bedient die HTML-Seite über Port 8000.
   * _Im Mac App Store nicht mehr verfügbar. Verwenden Sie vergleichbare Alternativen wie [Jeeves](https://apps.apple.com/de/app/jeeves-local-http-server/id980824182?mt=12)._.
* [!DNL AEM Dispatcher] wird unter [!DNL Apache HTTP Web Server] 2.4 ausgeführt und führt einen Reverse-Proxy-Vorgang von `aem-publish.local` nach `localhost:4503` aus.

Weitere Informationen finden Sie unter [Grundlegendes zu CORS (Cross-Origin Resource Sharing) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com – HTML und JavaScript

Diese Web-Seite hat folgende Logik:

1. Es wird auf die Schaltfläche geklickt.
1. Dadurch wird eine [!DNL AJAX GET]-Anfrage an `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json` gestellt.
1. Daraufhin wird `jcr:title` aus der JSON-Antwort abgerufen.
1. Schließlich wird `jcr:title` in das DOM eingefügt.

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## OSGi-Factory-Konfiguration

Die OSGi-Konfigurations-Factory für [!DNL Cross-Origin Resource Sharing] ist verfügbar über:

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Konfiguration des Dispatchers {#dispatcher-configuration}

### Zulassen von CORS-Anforderungsheadern

So lassen Sie die erforderlichen [HTTP-Anforderungs-Header zur Weiterleitung an AEM zur Verarbeitung](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#specifying-the-http-headers-to-pass-through-clientheaders), müssen sie in der Dispatcher-Funktion `/clientheaders` Konfiguration.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Zwischenspeichern von CORS-Antwortheadern

Um das Zwischenspeichern und Bereitstellen von CORS-Headern für zwischengespeicherten Inhalt zu ermöglichen, fügen Sie Folgendes hinzu [/cache /headers configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#caching-http-response-headers) zur AEM Veröffentlichung `dispatcher.any` -Datei.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

**Starten Sie die Webserver-Anwendung neu**, nachdem Sie Änderungen an der Datei `dispatcher.any` vorgenommen haben.

Es ist wahrscheinlich erforderlich, den Cache vollständig zu löschen, um sicherzustellen, dass Header bei der nächsten Anfrage nach einer `/cache /headers`-Konfigurationsaktualisierung ordnungsgemäß zwischengespeichert werden.

## Hilfsmaterialien {#supporting-materials}

* [Jeeves für macOS](https://apps.apple.com/de/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (kompatibel mit Windows/macOS/Linux)

* [Grundlegendes zu CORS (Cross-Origin Resource Sharing) in AEM](./understand-cross-origin-resource-sharing.md)
* [Cross-Origin Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffssteuerung (Mozilla MDN)](https://developer.mozilla.org/de/docs/Web/HTTP/Access_control_CORS)
