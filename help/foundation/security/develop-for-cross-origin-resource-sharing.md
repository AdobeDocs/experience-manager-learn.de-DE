---
title: Entwickeln für Cross-Herkunft Resource Sharing (CORS) mit AEM
description: Ein kurzes Beispiel für die Nutzung von CORS für den Zugriff auf AEM Inhalte einer externen Webanwendung über clientseitiges JavaScript.
version: 6.3, 6,4, 6.5
sub-product: Stiftung, Inhaltsdienste, Sites
feature: null
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
translation-type: tm+mt
source-git-commit: bc14783840a47fb79ddf1876aca1ef44729d097e
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 0%

---


# Entwickeln für Cross-Herkunft Resource Sharing (CORS)

Ein kurzes Beispiel für die Nutzung von [!DNL CORS] für den Zugriff auf AEM Inhalte einer externen Webanwendung über clientseitiges JavaScript.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

In diesem Video:

* **www.example.** commaps to localhost über  `/etc/hosts`
* **aem-publish.** localmaps für localhost über  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)  (ein Wrapper für  [[!DNL Python]den SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) liefert die HTML-Seite über Port 8000.
* [!DNL AEM Dispatcher] läuft auf  [!DNL Apache HTTP Web Server] 2.4 und umgekehrte Proxying-Anfrage  `aem-publish.local` zu  `localhost:4503`.

Weitere Informationen finden Sie unter [Informationen zum Cross-Herkunft Resource Sharing (CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com von HTML und JavaScript

Diese Webseite hat eine Logik, die

1. Klicken auf die Schaltfläche
1. Fordert [!DNL AJAX GET] an `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json` an
1. Ruft das `jcr:title` aus der JSON-Antwort ab
1. injiziert das `jcr:title` in das DOM

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

Die OSGi-Konfigurationsfactory für [!DNL Cross-Origin Resource Sharing] ist verfügbar über:

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

## Dispatcher-Konfiguration {#dispatcher-configuration}

Um die Zwischenspeicherung und Bereitstellung von CORS-Headern für zwischengespeicherten Inhalt zuzulassen, fügen Sie allen unterstützenden AEM Publish `dispatcher.any`-Dateien die folgende [/clientheaders-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) hinzu.

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Starten Sie die Webserver-** Anwendung neu, nachdem Sie Änderungen an der  `dispatcher.any` Datei vorgenommen haben.

Es ist wahrscheinlich erforderlich, den Cache vollständig zu leeren, um sicherzustellen, dass Header bei der nächsten Anforderung nach einem `/clientheaders`-Konfigurationsupdate ordnungsgemäß zwischengespeichert werden.

## Unterstützende Materialien {#supporting-materials}

* [AEM OSGi-Konfigurationsfactory für Richtlinien zum Cross-Herkunft Resource Sharing](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer für macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (kompatibel mit Windows/macOS/Linux)

* [Grundlegendes zum Cross-Herkunft Resource Sharing (CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [Cross-Herkunft Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffskontrolle (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

