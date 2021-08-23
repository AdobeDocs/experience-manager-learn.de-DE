---
title: Verwenden der Freigabe in Social Media in AEM Sites
description: Erfahren Sie mehr über die Einrichtung und Verwendung der Social Media-Sharing-Komponente.
feature: Kernkomponenten
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 10%

---


# Social Media-Freigabe verwenden {#using-social-media-sharing-in-aem-sites}

Erfahren Sie mehr über die Einrichtung und Verwendung der Social Media-Sharing-Komponente.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

In diesem Video werden die folgenden Funktionen der Social Media-Sharing-Komponente (Teil von [AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)) mithilfe der Beispiel-Website [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) untersucht.

* 0:00 - Hinzufügen und Konfigurieren der Social Media-Sharing-Komponente
* 1:00 - Freigeben für Facebook
* 3:10 - Freigeben für Pinterest
* 6:25 - Verwenden der Social Media-Sharing-Komponente auf einer Produktseite

## Externalizer-Setup {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) externalizer sollten sowohl in der AEM-Autoreninstanz als auch in der AEM-Veröffentlichungsinstanz eingerichtet werden, um den Veröffentlichungs-Ausführungsmodus der öffentlich zugänglichen Domäne zuzuordnen, die für den Zugriff auf AEM Publish verwendet wird.

In diesem Video verwenden wir `/etc/hosts`, um *www.example.com* zum Auflösen in localhost zu verwenden, und verwenden eine [grundlegende AEM Dispatcher-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html), um www.example.com zu ermöglichen, AEM Publish zu unterstützen.

## Unterstützende Materialien {#supporting-materials}

* [Herunterladen der AEM Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/releases)
* [We.Retail herunterladen](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Installieren des Dispatchers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
