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
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 5%

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

    &quot;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    # Entfernt die Antwortheader dieses Namens, sofern vorhanden. Wenn mehrere Header mit demselben Namen vorhanden sind, werden alle entfernt.
    Header unset Cache-Control
    Header unset Läuft ab
    
    # Weist das CDN an, die Antwort nicht zwischenzuspeichern.
    Header-Set Cache-Control &quot;private&quot;
    &lt;/locationmatch>
    &quot;

#### Beispiel

So deaktivieren Sie die CDN-Zwischenspeicherung des **CSS-Inhaltstypen** für einige Fehlerbehebungszwecke, führen Sie diese Schritte aus.

Beachten Sie, dass zum Umgehen des vorhandenen CSS-Cache eine Änderung in der CSS-Datei erforderlich ist, um einen neuen Cache-Schlüssel für die CSS-Datei zu generieren.

1. Suchen Sie in Ihrem AEM-Projekt die gewünschte Besucherdatei aus `dispatcher/src/conf.d/available_vhosts` Verzeichnis.
1. Aktualisieren Sie den vhost (z. B. `wknd.vhost`) wie folgt:

       &quot;conf
       &lt;locationmatch etc.clientlibs=&quot;&quot;>*\.(css)$&quot;>
       # Entfernt die Antwortheader dieses Namens, sofern vorhanden. Wenn mehrere Header mit demselben Namen vorhanden sind, werden alle entfernt.
       Header unset Cache-Control
       Header unset Läuft ab
       
       # Weist das CDN an, die Antwort nicht zwischenzuspeichern.
       Header-Set Cache-Control &quot;private&quot;
       &lt;/locationmatch>
       &quot;
   Die vhost-Dateien in `dispatcher/src/conf.d/enabled_vhosts` Verzeichnis **symlinks** zu den Dateien in `dispatcher/src/conf.d/available_vhosts` -Verzeichnis erstellen. Stellen Sie daher sicher, dass Sie symlinks erstellen, falls nicht vorhanden.
1. Stellen Sie die vhost-Änderungen in der gewünschten AEM as a Cloud Service Umgebung mit dem [Cloud Manager - Web-Tier-Konfigurations-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) oder [RDE-Befehle](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Benutzerspezifischer Java™-Code

Diese Option ist sowohl für die AEM als auch für die Autoreninstanz verfügbar. Um die Cache-Header zu aktualisieren, verwenden Sie die `SlingHttpServletResponse` -Objekt in benutzerdefiniertem Java™-Code (Sling-Servlet, Sling-Servlet-Filter). Die allgemeine Syntax lautet wie folgt:

    &quot;java
    response.setHeader(&quot;Cache-Control&quot;, &quot;private&quot;);
    &quot;
