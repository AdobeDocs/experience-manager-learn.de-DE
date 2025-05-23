---
title: Entwickeln für CORS (Cross-Origin Resource Sharing) mit AEM
description: Ein kurzes Beispiel für die Nutzung von CORS zum Zugriff auf AEM-Inhalte von einer externen Web-Anwendung über Client-seitiges JavaScript.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '318'
ht-degree: 100%

---

# Entwickeln für CORS (Cross-Origin Resource Sharing)

Ein kurzes Beispiel für die Nutzung von [!DNL CORS] zum Zugriff auf AEM-Inhalte von einer externen Web-Anwendung über Client-seitiges JavaScript. In diesem Beispiel wird die CORS OSGi-Konfiguration verwendet, um den CORS-Zugriff auf AEM zu aktivieren. Die Verwendung der OSGi-Konfiguration ist möglich, wenn:

* Eine einzige Quelle auf Inhalte von AEM Publish zugreift
* Für AEM Author ein CORS-Zugriff erforderlich ist

Wenn der Zugriff auf AEM Publish von mehreren Quellen aus erforderlich ist, lesen Sie [diese Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=de#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/34015?quality=12&learn=on&captions=ger)

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

### Zulassen von CORS-Anfrage-Headern

Damit die erforderlichen [HTTP-Anfrage-Header zur Verarbeitung in AEM durchgeleitet werden können](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#specifying-the-http-headers-to-pass-through-clientheaders), müssen sie in der `/clientheaders`-Konfiguration vom Dispatcher zugelassen werden.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Zwischenspeichern von CORS-Anfrage-Headern

Um das Zwischenspeichern und Bereitstellen von CORS-Headern für zwischengespeicherte Inhalte zu ermöglichen, fügen Sie die folgende [/cache-/headers-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#caching-http-response-headers) zur Datei `dispatcher.any` von AEM Publish hinzu.

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
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (kompatibel mit Windows/macOS/Linux)

* [Grundlegendes zu CORS (Cross-Origin Resource Sharing) in AEM](./understand-cross-origin-resource-sharing.md)
* [Cross-Origin Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffssteuerung (Mozilla MDN)](https://developer.mozilla.org/de/docs/Web/HTTP/Access_control_CORS)
