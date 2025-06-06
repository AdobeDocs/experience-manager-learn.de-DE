---
title: Deaktivieren des CDN-Cachings
description: Erfahren Sie, wie Sie das Caching von HTTP-Antworten im CDN von AEM as a Cloud Service deaktivieren.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: cf006f24abbc5aa4b91277b91d68538c41d33e15
workflow-type: ht
source-wordcount: '0'
ht-degree: 100%

---

# Deaktivieren des CDN-Cachings

Erfahren Sie, wie Sie das Caching von HTTP-Antworten im CDN von AEM as a Cloud Service deaktivieren. Das Caching von Antworten wird durch die HTTP-Antwort-Cache-Header `Cache-Control`, `Surrogate-Control` oder `Expires` gesteuert.

Diese Cache-Header werden normalerweise in AEM Dispatcher-vhost-Konfigurationen mithilfe von `mod_headers` festgelegt, können aber auch in benutzerdefiniertem Java™-Code festgelegt werden, der in AEM Publish selbst ausgeführt wird.

## Caching-Standardverhalten

Das Caching von HTTP-Antworten im CDN von AEM as a Cloud Service wird durch die folgenden HTTP-Antwort-Header der ursprünglichen Elemente `Cache-Control`, `Surrogate-Control` oder `Expires` gesteuert.  Ursprungsantworten, die `private`, `no-cache` oder `no-store` in `Cache-Control` enthalten, werden nicht zwischengespeichert.

Überprüfen Sie das [Caching-Standardverhalten](./enable-caching.md#default-caching-behavior) für AEM Publish und Author, wenn ein AEM-Projekt bereitgestellt wird, das auf einem AEM-Projektarchetyp basiert.


## Deaktivieren des Cachings

Das Deaktivieren des Cachings kann sich negativ auf die Leistung Ihrer AEM as a Cloud Service-Instanz auswirken. Seien Sie daher vorsichtig, wenn Sie das Caching-Standardverhalten deaktivieren.

Es gibt jedoch einige Szenarien, in denen es angebracht sein kann, das Caching zu deaktivieren, z. B.:

- Entwickeln einer neuen Funktion, wobei Sie die Änderungen sofort sehen möchten.
- Inhalt ist sicher (nur für authentifizierte Benutzende vorgesehen) oder dynamisch (z. B. Warenkorb, Bestelldetails) und sollte nicht zwischengespeichert werden.

Um das Caching zu deaktivieren, können Sie die Cache-Header auf zwei Arten aktualisieren.

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

    # Instructs the Browser and the CDN to not cache the response.
    Header always set Cache-Control "private"

    # Instructs only the CDN to not cache the response.
    Header always set Surrogate-Control "private"
</LocationMatch>
```

#### Beispiel

Um das CDN-Caching der **CSS-Inhaltstypen** zum Beheben bestimmter Fehler zu deaktivieren, führen Sie diese Schritte aus.

Beachten Sie, dass zum Umgehen des vorhandenen CSS-Caches die CSS-Datei bearbeitet werden muss, um einen neuen Cache-Schlüssel für die CSS-Datei zu generieren.

1. Suchen Sie in Ihrem AEM-Projekt im Verzeichnis `dispatcher/src/conf.d/available_vhosts` nach der gewünschten vhost-Datei.
1. Aktualisieren Sie die vhost-Datei (z. B. `wknd.vhost`) wie folgt:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the Browser and the CDN to not cache the response.
       Header always set Cache-Control "private"
   </LocationMatch>
   ```

   Die vhost-Dateien im Verzeichnis `dispatcher/src/conf.d/enabled_vhosts` sind **Symlinks** zu den Dateien im Verzeichnis `dispatcher/src/conf.d/available_vhosts`. Stellen Sie daher sicher, dass Sie Symlinks erstellen, falls diese nicht vorhanden sind.
1. Stellen Sie die vhost-Änderungen in der gewünschten AEM as a Cloud Service-Umgebung bereit. Verwenden Sie dazu die [Web-Stufen-Konfigurations-Pipeline von Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=de#web-tier-config-pipelines) oder [RDE-Befehle](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=de#deploy-apache-or-dispatcher-configuration).

### Benutzerdefinierter Java™-Code

Diese Option ist für AEM Publish und Author verfügbar. Verwenden Sie zum Aktualisieren der Cache-Header das Objekt `SlingHttpServletResponse` in benutzerdefiniertem Java™-Code (Sling-Servlet, Sling-Servlet-Filter). Die allgemeine Syntax lautet folgendermaßen:

```java
response.setHeader("Cache-Control", "private");
```
