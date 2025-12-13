---
title: Aktivieren des CDN-Cachings
description: Erfahren Sie, wie Sie das Caching von HTTP-Antworten im CDN von AEM as a Cloud Service aktivieren.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 100%

---

# Aktivieren des CDN-Cachings

Erfahren Sie, wie Sie das Caching von HTTP-Antworten im CDN von AEM as a Cloud Service aktivieren. Das Caching von Antworten wird durch die HTTP-Antwort-Cache-Header `Cache-Control`, `Surrogate-Control` oder `Expires` gesteuert.

Diese Cache-Header werden normalerweise in AEM Dispatcher-vhost-Konfigurationen mithilfe von `mod_headers` festgelegt, können aber auch in benutzerdefiniertem Java™-Code festgelegt werden, der in AEM Publish selbst ausgeführt wird.

## Caching-Standardverhalten

Wenn KEINE benutzerdefinierten Konfigurationen vorhanden sind, werden die Standardwerte verwendet. Im folgenden Screenshot sehen Sie das Caching-Standardverhalten für AEM Publish und Author, wenn ein AEM-Projekt `mynewsite` bereitgestellt wird, das auf einem [AEM-Projektarchetypen](https://github.com/adobe/aem-project-archetype) basiert.

![Caching-Standardverhalten](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Weitere Informationen finden Sie unter [AEM Publish – Standardmäßige Cache-Lebensdauer](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html?lang=de#cdn-cache-life) und [AEM Author – Standardmäßige Cache-Lebensdauer](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?lang=de#default-cache-life).

Kurz zusammengefasst: AEM as a Cloud Service speichert die meisten Inhaltstypen (HTML, JSON, JS, CSS und Assets) in AEM Publish und einige Inhaltstypen (JS, CSS) in AEM Author zwischen.

## Aktivieren des Cachings

Um das Caching-Standardverhalten zu ändern, können Sie die Cache-Header auf zwei Arten aktualisieren.

1. **Dispatcher-vhost-Konfiguration:** Nur für AEM Publish verfügbar.
1. **Benutzerdefinierter Java™-Code:** Für AEM Publish und Author verfügbar.

Sehen wir uns diese beiden Optionen an.

### Dispatcher-vhost-Konfiguration

Diese Option wird zum Aktivieren des Cachings empfohlen, allerdings ist sie nur für AEM Publish verfügbar. Um die Cache-Header zu aktualisieren, verwenden Sie das Modul `mod_headers` und die Anweisung `<LocationMatch>` in der vhost-Datei des Apache HTTP-Servers. Die allgemeine Syntax lautet folgendermaßen:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

Im Folgenden ist der Zweck der einzelnen **Header** und entsprechenden **Attribute** für den Header zusammengefasst.

|                     | Webbrowser | CDN | Beschreibung |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | Dieser Header steuert die Webbrowser- und CDN-Cache-Lebensdauer. |
| Surrogate-Control | ✘ | ✔ | Dieser Header steuert die CDN-Cache-Lebensdauer. |
| Ablaufdatum | ✔ | ✔ | Dieser Header steuert die Webbrowser- und CDN-Cache-Lebensdauer. |


- **max-age**: Dieses Attribut steuert die Gültigkeitsdauer (Time to Live, TTL) des Antwortinhalts in Sekunden.
- **stale-while-revalidate**: Dieses Attribut steuert, wie Antwortinhalt im _Status „Veraltet“_ auf CDN-Ebene gehandhabt wird, wenn die empfangene Anfrage innerhalb des festgelegten Zeitraums in Sekunden liegt. Der _Status „Veraltet“_ tritt in dem Zeitraum auf, nachdem die TTL abgelaufen ist, aber bevor die Antwort erneut validiert wird.
- **stale-if-error**: Dieses Attribut steuert, wie Antwortinhalt im _Status „Veraltet“_ auf CDN-Ebene gehandhabt wird, wenn der Ursprungs-Server nicht verfügbar ist und die empfangene Anfrage innerhalb des festgelegten Zeitraums in Sekunden liegt.

Weitere Informationen finden Sie unter [Veraltung und erneute Validierung](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/).

#### Beispiel

Gehen Sie wie folgt vor, um die Webbrowser- und CDN-Cache-Lebensdauer des **HTML-Content-Typs** auf _10 Minuten_ zu verlängern, ohne dass er als veraltet gilt:

1. Suchen Sie in Ihrem AEM-Projekt im Verzeichnis `dispatcher/src/conf.d/available_vhosts` die gewünschte vhost-Datei.
1. Aktualisieren Sie die vhost-Datei (z. B. `wknd.vhost`) wie folgt:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   Die vhost-Dateien im Verzeichnis `dispatcher/src/conf.d/enabled_vhosts` sind **Symlinks** zu den Dateien im Verzeichnis `dispatcher/src/conf.d/available_vhosts`. Stellen Sie daher sicher, dass Sie Symlinks erstellen, falls diese nicht vorhanden sind.
1. Stellen Sie die vhost-Änderungen in der gewünschten AEM as a Cloud Service-Umgebung bereit. Verwenden Sie dazu die [Web-Stufen-Konfigurations-Pipeline von Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=de#web-tier-config-pipelines) oder [RDE-Befehle](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=de#deploy-apache-or-dispatcher-configuration).

Wenn Sie jedoch unterschiedliche Werte für die Lebensdauer des Webbrowsers und des CDN-Caches haben möchten, können Sie den Header `Surrogate-Control` im oben genannten Beispiel verwenden. Soll der Cache an einem bestimmten Datum und zu einer bestimmten Uhrzeit ablaufen, können Sie auch den Header `Expires` verwenden. Außerdem können Sie mit den Attributen `stale-while-revalidate` und `stale-if-error` steuern, wie Antwortinhalt im Status „Veraltet“ gehandhabt wird. Das AEM-WKND-Projekt nutzt eine CDN-Cache-Konfiguration mit einer [Referenz für den Status „Veraltet“](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155).

Entsprechend können Sie die Cache-Header auch für andere Inhaltstypen (JSON, JS, CSS und Assets) aktualisieren.

### Benutzerdefinierter Java™-Code

Diese Option ist für AEM Publish und Author verfügbar. Es wird jedoch nicht empfohlen, das Caching in AEM Author zu aktivieren und das Caching-Standardverhalten beizubehalten.

Verwenden Sie zum Aktualisieren der Cache-Header das Objekt `HttpServletResponse` in benutzerdefiniertem Java™-Code (Sling-Servlet, Sling-Servlet-Filter). Die allgemeine Syntax lautet folgendermaßen:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
