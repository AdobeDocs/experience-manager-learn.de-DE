---
title: Setup ContextHub for Personalization with AEM Sites
description: ContextHub ist ein Framework zum Speichern, Ändern und Darstellen von Kontextdaten. Mit der ContextHub-JavaScript-API können Sie auf Speicher zugreifen, um Daten bei Bedarf zu erstellen, zu aktualisieren und zu löschen. Daher stellt ContextHub eine Datenschicht auf Ihren Seiten dar. This page describes how to add context hub to your AEM site pages.
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
source-git-commit: 9e4b01173f5d89075ad64adfb8b982b2297e2c39
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 35%

---

# Setup ContextHub for Personalization {#set-up-contexthub}

ContextHub ist ein Framework zum Speichern, Ändern und Darstellen von Kontextdaten. Mit der ContextHub-JavaScript-API können Sie auf Speicher zugreifen, um Daten bei Bedarf zu erstellen, zu aktualisieren und zu löschen. Daher stellt ContextHub eine Datenschicht auf Ihren Seiten dar. This page describes how to add context hub to your AEM site pages.

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>We use the WKND reference site for this video and it is not part of AEM release. [](https://github.com/adobe/aem-guides-wknd/releases)

Add ContextHub to your pages to enable the ContextHub features and to link to the ContextHub JavaScript libraries. The ContextHub JavaScript API provides access to the context data that ContextHub manages.

## Hinzufügen von ContextHub zu einer Seitenkomponente {#adding-contexthub-to-a-page-component}

`contexthub``<head>` The HTL code for your page component resembles the following example:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Site Configuration and ContextHub Segments {#site-configuration-and-contexthub-segments}

ContextHub enthält eine Segmentations-Engine, die Segmente verwaltet und bestimmt, welche Segmente für den aktuellen Kontext aufgelöst werden. Mehrere Segmente sind definiert. Sie können die Javascript-API verwenden, um [aufgelöste Segmente zu ermitteln](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). [](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=de)

## Create Segments {#create-segments}

Create AEM segments that act as rules for the teasers. That is, they define when content within a teaser appears on a web page. Inhalte können dann entsprechend den zutreffenden Segmenten speziell auf die Anforderungen und Interessen des Besuchers abgestimmt werden.

## Assigning Cloud Configuration, Segment path and ContextHub path to your site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Assigning the Cloud configuration path, segmentation path and ContextHub path to your site root node so you can create a personalized experience for your audience. Using the ContextHub, you can manipulate the context data and test your resolved segments.

![CRXDE Lite](assets/crx-de-properties.png) 

You can read more about ContextHub and segmentation below:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Grundlegendes zur Segmentierung](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Konfigurieren der Segmentierung mit ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
