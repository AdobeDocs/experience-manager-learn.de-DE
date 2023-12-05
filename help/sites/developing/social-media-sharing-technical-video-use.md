---
title: Verwenden der Social-Media-Freigabe in AEM Sites
description: Erfahren Sie mehr über die Einrichtung und Verwendung der Komponente zur Social-Media-Freigabe.
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 530
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 100%

---

# Verwenden der Social-Media-Freigabe {#using-social-media-sharing-in-aem-sites}

Erfahren Sie mehr über die Einrichtung und Verwendung der Komponente zur Social-Media-Freigabe.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

In diesem Video werden die folgenden Funktionen der Komponente zur Social-Media-Freigabe (Teil der [AEM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)) mithilfe der Beispiel-Website [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) beschrieben.

* 0:00 – Hinzufügen und Konfigurieren der Komponente zur Social-Media-Freigabe
* 1:00 – Freigeben für Facebook
* 3:10 – Freigeben für Pinterest
* 6:25 – Verwenden der Komponente zur Social-Media-Freigabe auf einer Produktseite

## Externalizer-Setup {#externalizer-setup}

![Day CQ Link Externalizer ](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

Der [AEM-Externalizer](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/externalizer.html) sollte sowohl in AEM Author als auch in AEM Publish eingerichtet werden, um den Veröffentlichungs-Ausführungsmodus der öffentlich zugänglichen Domain zuzuordnen, über die auf AEM Publish zugegriffen wird.

In diesem Video verwenden wir `/etc/hosts` zum Spoofen von *www.example.com*, um zu „localhost“ aufzulösen. Über eine [grundlegende AEM Dispatcher-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=de) wird der Website „www.example.com“ zudem „publish“ (für AEM Publish) vorangestellt.

## Hilfsmaterialien {#supporting-materials}

* [Herunterladen der AEM-Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/releases)
* [Herunterladen von We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installieren des Dispatchers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=de)
