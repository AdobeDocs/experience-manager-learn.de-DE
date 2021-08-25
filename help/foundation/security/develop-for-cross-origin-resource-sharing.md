---
title: Entwickeln für Cross-Origin Resource Sharing (CORS) mit AEM
description: Ein kurzes Beispiel für die Nutzung von CORS für den Zugriff auf AEM Inhalt von einer externen Webanwendung über clientseitiges JavaScript.
version: 6.3, 6,4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# Für Cross-Origin Resource Sharing (CORS) entwickeln

Ein kurzes Beispiel für die Nutzung von [!DNL CORS] für den Zugriff auf AEM Inhalt von einer externen Web-Anwendung über clientseitiges JavaScript.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

In diesem Video:

* **www.example.** commaps auf localhost via  `/etc/hosts`
* **aem-publish.** localmaps to localhost via  `/etc/hosts`
* SimpleHTTPServer (ein Wrapper für den SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) von [[!DNL Python]) versorgt die HTML-Seite über Port 8000.
   * _Nicht mehr im Mac App Store verfügbar. Verwenden Sie Ähnliches wie [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] wird in Version  [!DNL Apache HTTP Web Server] 2.4 ausgeführt und die Anfrage zur umgekehrten Proxy  `aem-publish.local` an  `localhost:4503`.

Weitere Informationen finden Sie unter [Informationen zur Cross-Origin Resource Sharing (CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML und JavaScript

Diese Webseite weist eine Logik auf, dass

1. Nach Klicken auf die Schaltfläche
1. Stellt eine [!DNL AJAX GET] -Anfrage an `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Ruft die `jcr:title` aus der JSON-Antwort ab
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

## OSGi-Werkskonfiguration

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

Um das Zwischenspeichern und Bereitstellen von CORS-Headern für zwischengespeicherten Inhalt zu ermöglichen, fügen Sie allen unterstützenden AEM Publish-Dateien `dispatcher.any` die folgende [/clientheaders-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) hinzu.

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

Es ist wahrscheinlich, dass der Cache vollständig gelöscht werden muss, um sicherzustellen, dass Kopfzeilen bei der nächsten Anfrage nach einer `/clientheaders`-Konfigurationsaktualisierung ordnungsgemäß zwischengespeichert werden.

## Unterstützende Materialien {#supporting-materials}

* [AEM OSGi-Konfigurationsfactory für Cross-Origin Resource Sharing-Richtlinien](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Jeeves für macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)  (Windows/macOS/Linux-kompatibel)

* [Verstehen der Cross-Origin Resource Sharing (CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [Cross-Origin Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffssteuerung (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

