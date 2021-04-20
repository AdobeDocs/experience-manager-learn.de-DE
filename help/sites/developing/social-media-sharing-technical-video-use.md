---
title: Weitergeben in sozialen Medien in AEM Sites
description: Erfahren Sie, wie Sie die Komponente Social Media Sharing einrichten und verwenden.
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 11%

---


# Freigeben in sozialen Medien {#using-social-media-sharing-in-aem-sites}

Erfahren Sie, wie Sie die Komponente Social Media Sharing einrichten und verwenden.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

In diesem Video werden die folgenden Funktionen der Komponente &quot;Social Media Sharing&quot;(Teil von [AEM Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html)) mithilfe der Beispiel-Website [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) untersucht.

* 0:00 - Hinzufügen und Konfigurieren der Komponente &quot;Social Media Sharing&quot;
* 1:00 - Freigeben auf Facebook
* 3:10 - An Pinterest freigeben
* 6:25 - Verwenden der Komponente &quot;Social Media Sharing&quot;auf einer Produktseite

## Externalizer-Setup {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) externalizerare sollten sowohl für AEM Author als auch für AEM Publish eingerichtet werden, um den Veröffentlichungsmodus der öffentlich zugänglichen Domäne zuzuordnen, die für den Zugriff auf AEM Publish verwendet wird.

In diesem Video verwenden wir `/etc/hosts` zum Spoof *www.example.com* zum Auflösen auf localhost und verwenden eine [grundlegende AEM Dispatcher-Konfiguration](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html), damit www.example.com AEM Publish vorführen kann.

## Unterstützende Materialien {#supporting-materials}

* [AEM Hauptkomponenten herunterladen](https://github.com/adobe/aem-core-wcm-components/releases)
* [Wir.Retail herunterladen](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installieren des Dispatchers](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
