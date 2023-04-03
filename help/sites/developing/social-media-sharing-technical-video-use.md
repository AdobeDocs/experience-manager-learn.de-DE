---
title: Verwenden der Freigabe in Social Media in AEM Sites
description: Erfahren Sie mehr über die Einrichtung und Verwendung der Social Media-Sharing-Komponente.
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 10%

---

# Social Media-Freigabe verwenden {#using-social-media-sharing-in-aem-sites}

Erfahren Sie mehr über die Einrichtung und Verwendung der Social Media-Sharing-Komponente.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

In diesem Video werden die folgenden Funktionen der Social Media-Sharing-Komponente (Teil von [AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)) mithilfe der [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) Beispiel-Website.

* 0:00 - Hinzufügen und Konfigurieren der Social Media-Sharing-Komponente
* 1:00 - Freigeben für Facebook
* 3:10 - Freigeben für Pinterest
* 6:25 - Verwenden der Social Media-Sharing-Komponente auf einer Produktseite

## Externalizer-Setup {#externalizer-setup}

![Day CQ Link Externalizer ](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM Externalizer](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) sollte sowohl in der AEM-Autoreninstanz als auch in der AEM-Veröffentlichungsinstanz eingerichtet werden, um den Veröffentlichungslaufmodus der öffentlich zugänglichen Domäne zuzuordnen, über die auf AEM Publish zugegriffen wird.

In diesem Video verwenden wir `/etc/hosts` zum Sporen *www.example.com* , um auf localhost aufzulösen, und verwenden Sie eine [grundlegende AEM Dispatcher-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) , damit www.example.com AEM Publish vorschalten kann.

## Unterstützende Materialien {#supporting-materials}

* [Herunterladen der AEM Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/releases)
* [We.Retail herunterladen](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installieren des Dispatchers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
