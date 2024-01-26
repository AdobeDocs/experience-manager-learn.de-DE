---
title: Deaktivieren der CDN-Zwischenspeicherung
description: Erfahren Sie, wie Sie das Zwischenspeichern von HTTP-Antworten im CDN AEM as a Cloud Service deaktivieren.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 116
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 6%

---

# Deaktivieren der CDN-Zwischenspeicherung

Erfahren Sie, wie Sie das Zwischenspeichern von HTTP-Antworten im CDN AEM as a Cloud Service deaktivieren. Das Zwischenspeichern von Antworten wird durch `Cache-Control`, `Surrogate-Control`oder `Expires` HTTP-Antwort-Cache-Header.

Diese Cache-Header werden normalerweise in AEM Dispatcher-Vhost-Konfigurationen mithilfe von `mod_headers` festgelegt, können aber auch in benutzerdefiniertem Java™-Code festgelegt werden, der in AEM Publish selbst ausgeführt wird.

## Standard-Caching-Verhalten

Überprüfen Sie das standardmäßige Caching-Verhalten für AEM Veröffentlichung und Autor, wenn ein [AEM Projektarchetyp](./enable-caching.md#default-caching-behavior) wird AEM Projekt bereitgestellt.

## Zwischenspeicherung deaktivieren

Das Deaktivieren der Zwischenspeicherung kann sich negativ auf die Leistung Ihrer AEM as a Cloud Service Instanz auswirken. Gehen Sie daher beim Deaktivieren des standardmäßigen Caching-Verhaltens vorsichtig vor.

Es gibt jedoch einige Szenarien, in denen Sie die Zwischenspeicherung deaktivieren können, z. B.:

- Entwickeln einer neuen Funktion und möchten die Änderungen sofort sehen.
- Inhalte sind sicher (nur für authentifizierte Benutzer gedacht) oder dynamisch (Warenkorb, Bestelldetails) und sollten nicht zwischengespeichert werden.

Um die Zwischenspeicherung zu deaktivieren, können Sie die Cache-Header auf zwei Arten aktualisieren.

1. **Dispatcher-vhost-Konfiguration:** Nur für AEM Veröffentlichung verfügbar.
1. **Benutzerdefinierter Java™-Code:** Verfügbar für AEM Veröffentlichung und Autor.

Lassen Sie uns jede dieser Optionen überprüfen.

### Dispatcher-Vhost-Konfiguration

Diese Option ist der empfohlene Ansatz zum Deaktivieren der Zwischenspeicherung. Sie ist jedoch nur für AEM Veröffentlichung verfügbar. Um die Cache-Header zu aktualisieren, verwenden Sie die `mod_headers` -Modul und `<LocationMatch>` in der vhost-Datei des Apache HTTP-Servers. Die allgemeine Syntax lautet wie folgt:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### Beispiel

So deaktivieren Sie die CDN-Zwischenspeicherung des **CSS-Inhaltstypen** für einige Fehlerbehebungszwecke, führen Sie diese Schritte aus.

Beachten Sie, dass zum Umgehen des vorhandenen CSS-Cache eine Änderung in der CSS-Datei erforderlich ist, um einen neuen Cache-Schlüssel für die CSS-Datei zu generieren.

1. Suchen Sie in Ihrem AEM-Projekt die gewünschte Besucherdatei aus `dispatcher/src/conf.d/available_vhosts` Verzeichnis.
1. Aktualisieren Sie den vhost (z. B. `wknd.vhost`) wie folgt:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   Die vhost-Dateien in `dispatcher/src/conf.d/enabled_vhosts` Verzeichnis **symlinks** zu den Dateien in `dispatcher/src/conf.d/available_vhosts` -Verzeichnis erstellen. Stellen Sie daher sicher, dass Sie symlinks erstellen, falls nicht vorhanden.
1. Stellen Sie die vhost-Änderungen in der gewünschten AEM as a Cloud Service Umgebung mit dem [Cloud Manager - Web-Tier-Konfigurations-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) oder [RDE-Befehle](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Benutzerspezifischer Java™-Code

Diese Option ist sowohl für die AEM als auch für die Autoreninstanz verfügbar. Um die Cache-Header zu aktualisieren, verwenden Sie die `SlingHttpServletResponse` -Objekt in benutzerdefiniertem Java™-Code (Sling-Servlet, Sling-Servlet-Filter). Die allgemeine Syntax lautet wie folgt:

```java
response.setHeader("Cache-Control", "private");
```
