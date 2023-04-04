---
title: Entwickeln für Cross-Origin Resource Sharing (CORS) mit AEM
description: Ein kurzes Beispiel für die Nutzung von CORS für den Zugriff auf AEM Inhalt von einer externen Webanwendung über clientseitiges JavaScript.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Für Cross-Origin Resource Sharing (CORS) entwickeln

Ein kurzes Beispiel für die Nutzung von [!DNL CORS] , um über clientseitiges JavaScript auf AEM Inhalt einer externen Webanwendung zuzugreifen.

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

In diesem Video:

* **www.example.com** Zuordnung zu localhost über `/etc/hosts`
* **aem-publish.local** Zuordnung zu localhost über `/etc/hosts`
* SimpleHTTPServer (ein Wrapper für [[!DNL Python]&#39;s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) bedient die HTML-Seite über Port 8000.
   * _In Mac App Store nicht mehr verfügbar. Verwenden Sie ähnliche Beispiele wie [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] läuft auf [!DNL Apache HTTP Web Server] 2.4 und umgekehrte Weiterleitungsanfrage an `aem-publish.local` nach `localhost:4503`.

Weitere Informationen finden Sie unter [Verstehen der Cross-Origin Resource Sharing (CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML und JavaScript

Diese Webseite weist eine Logik auf, dass

1. Nach Klicken auf die Schaltfläche
1. Macht einen [!DNL AJAX GET] Anfrage an `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Ruft die `jcr:title` aus der JSON-Antwort
1. Fügt die `jcr:title` in das DOM

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

## Dispatcherkonfiguration {#dispatcher-configuration}

Um das Zwischenspeichern und Bereitstellen von CORS-Headern für zwischengespeicherten Inhalt zu ermöglichen, fügen Sie Folgendes hinzu [/clientheaders configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) für alle unterstützenden AEM-Veröffentlichungen `dispatcher.any` Dateien.

```
/myfarm { 
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

**Webserver-Anwendung neu starten** , nachdem Sie Änderungen an der `dispatcher.any` -Datei.

Es ist wahrscheinlich erforderlich, den Cache vollständig zu löschen, um sicherzustellen, dass Kopfzeilen bei der nächsten Anforderung nach einer `/clientheaders` Konfigurationsaktualisierung.

## Unterstützende Materialien {#supporting-materials}

* [Jeeves für macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (Kompatibel mit Windows/macOS/Linux)

* [Verstehen der Cross-Origin Resource Sharing (CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [Cross-Origin Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffssteuerung (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
