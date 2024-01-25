---
title: Einrichten von ContextHub für die Personalisierung mit AEM Sites
description: ContextHub ist ein Framework zum Speichern, Ändern und Darstellen von Kontextdaten. Mit der ContextHub-JavaScript-API können Sie auf Speicher zugreifen, um Daten bei Bedarf zu erstellen, zu aktualisieren und zu löschen. Daher stellt ContextHub eine Datenschicht auf Ihren Seiten dar. Auf dieser Seite wird beschrieben, wie Sie Ihren AEM Seiten einen ContextHub hinzufügen.
feature: Context Hub
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 366
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 93%

---

# Einrichten von ContextHub für die Personalisierung {#set-up-contexthub}

ContextHub ist ein Framework zum Speichern, Ändern und Darstellen von Kontextdaten. Mit der ContextHub-JavaScript-API können Sie auf Speicher zugreifen, um Daten bei Bedarf zu erstellen, zu aktualisieren und zu löschen. Daher stellt ContextHub eine Datenschicht auf Ihren Seiten dar. Auf dieser Seite wird beschrieben, wie Sie ContextHub zu Ihren AEM Sites-Seiten hinzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>In diesem Video wird die WKND-Referenz-Site verwendet, die nicht Teil von AEM ist. Sie können die [aktuelle Version hier](https://github.com/adobe/aem-guides-wknd/releases) herunterladen.

Fügen Sie Ihren Seiten ContextHub hinzu, um die ContextHub-Funktionen zu aktivieren und eine Verknüpfung zu den ContextHub-JavaScript-Bibliotheken herzustellen. Die ContextHub-JavaScript-API ermöglicht den Zugriff auf die von ContextHub verwalteten Kontextdaten.

## Hinzufügen von ContextHub zu einer Seitenkomponente {#adding-contexthub-to-a-page-component}

Schließen Sie die `contexthub`-Komponente in den Abschnitt `<head>` Ihrer Web-Seite ein, um die ContextHub-Funktionen zu aktivieren und eine Verknüpfung zu den ContextHub-JavaScript-Bibliotheken herzustellen. Der HTL-Code für Ihre Seitenkomponente sieht in etwa wie im folgenden Beispiel aus:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Site-Konfiguration und ContextHub-Segmente {#site-configuration-and-contexthub-segments}

ContextHub enthält eine Segmentierungs-Engine, die Segmente verwaltet und bestimmt, welche Segmente für den aktuellen Kontext aufgelöst werden. Mehrere Segmente sind definiert. Sie können die JavaScript-API verwenden, um [aufgelöste Segmente zu bestimmen](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Aktivieren Sie die ContextHub-Segmente für Ihre Site unter [[!UICONTROL Configuration Browser]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=de).

## Erstellen von Segmenten {#create-segments}

Erstellen Sie AEM-Segmente, die als Regeln für die Teaser dienen. Das heißt, sie definieren, wann Inhalte innerhalb eines Teasers auf einer Web-Seite angezeigt werden. Inhalte können dann entsprechend den zutreffenden Segmenten speziell auf die Anforderungen und Interessen der Besucherin bzw. des Besuchers abgestimmt werden.

## Zuweisen von Cloud-Konfiguration, Segmentpfad und ContextHub-Pfad zu Ihrer Site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Weisen Sie den Cloud-Konfigurationspfad, den Segmentierungspfad und den ContextHub-Pfad zum Stammknoten Ihrer Site zu, damit Sie ein personalisiertes Erlebnis für Ihre Zielgruppe erstellen können. Mit ContextHub können Sie die Kontextdaten ändern und Ihre aufgelösten Segmente testen.

![CRXDE Lite](assets/crx-de-properties.png)

Weitere Informationen zu ContextHub und zur Segmentierung finden Sie unten:

* [ContextHub](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Hinzufügen von ContextHub zur Seite und Zugreifen auf Speicher](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Grundlegendes zur Segmentierung](https://helpx.adobe.com/de/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Konfigurieren der Segmentierung mit ContextHub](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/segmentation.html)
