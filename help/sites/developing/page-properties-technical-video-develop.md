---
title: Erweitern der Seiteneigenschaften in AEM Sites
description: Erfahren Sie, wie Sie die Metadatenfelder der Seiteneigenschaften in Adobe Experience Manager Sites erweitern. In diesem Video wird beschrieben, wie Sie dies am effektivsten mithilfe der Funktionen des Sling Resource Merger erreichen.
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# Erweitern der Seiteneigenschaften {#extending-page-properties-in-aem-sites}

Das Anpassen der Metadatenfelder für die Seiteneigenschaften ist eine gängige Anforderung in jeder Sites-Implementierung. In diesem Video wird beschrieben, wie Sie dies am effektivsten mithilfe der Funktionen des Sling Resource Merger erreichen.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

Das obige Video zeigt, wie die Seiteneigenschaften für die [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd).

## Beispiel für das WKND-Seiteneigenschaftenpaket

Sie können die bereitgestellte [Beispiel-WKND-Seiteneigenschaftenpaket](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) contain **WKND** und **Allgemein** Registerkartenanpassungen, die im obigen Video gezeigt werden. Die **SocialMedia** Registerkartenanpassung wird nicht bereitgestellt, da [WKND-Seitenkomponente](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) verwendet jetzt die V3-Version der WCM-Kernkomponenten und in der V3-Version die [Social Sharing ist veraltet](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Zu Lernzwecken können Sie die WKND-Seitenkomponente jedoch mithilfe der `sling:resourceSuperType` Eigenschaftswert und Überlagerung der [Social Media](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) Registerkarte. Weitere Informationen finden Sie unter [Konfigurieren der Seiteneigenschaften](https://experienceleague.adobe.com/docs/experience-manager-64/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Dieses Beispielpaket sollte zu Lernzwecken auf der lokalen AEM SDK- oder AEM 6.X.X-Instanz installiert werden.
