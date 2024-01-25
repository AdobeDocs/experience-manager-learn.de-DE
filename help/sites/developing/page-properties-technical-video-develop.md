---
title: Erweitern der Seiteneigenschaften in AEM Sites
description: Erfahren Sie, wie Sie die Metadatenfelder der Seiteneigenschaften in Adobe Experience Manager Sites erweitern. In diesem Video wird beschrieben, wie Ihnen dies am effektivsten mithilfe der Sling Resource Merger-Funktionen gelingt.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 492
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 100%

---

# Erweitern der Seiteneigenschaften {#extending-page-properties-in-aem-sites}

Die Metadatenfelder für Seiteneigenschaften anzupassen, ist eine gängige Anforderung in jeder Sites-Implementierung. In diesem Video wird beschrieben, wie Ihnen dies am effektivsten mithilfe der Sling Resource Merger-Funktionen gelingt.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

Das obige Video zeigt, wie Sie die Seiteneigenschaften für die [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd) anpassen.

## WKND-Seiteneigenschaften-Beispielpaket

Sie können das bereitgestellte [WKND-Seiteneigenschaften-Beispielpaket](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) mit den im obigen Video gezeigten Anpassungen für die Registerkarten **WKND** und **Basic** verwenden. Die Anpassung für die Registerkarte **Social Media** wird nicht zur Verfügung gestellt, da die [WKND-Seitenkomponente](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) nun die V3-Version der WCM-Kernkomponenten nutzt und in der V3-Version [Social Sharing nicht weiter unterstützt](https://github.com/adobe/aem-core-wcm-components/pull/1930) wird.

Zu Lernzwecken können Sie die WKND-Seitenkomponente jedoch mithilfe des `sling:resourceSuperType`-Eigenschaftswerts auf die V2-Version der WCM-Kernkomponenten verweisen und die Registerkarte [Social Media](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) einblenden. Weitere Informationen finden Sie unter [Konfigurieren der Seiteneigenschaften](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=de#configuring-your-page-properties).

Dieses Beispielpaket sollte zu Lernzwecken in der lokalen AEM SDK- oder AEM 6.X.X-Instanz installiert werden.
