---
title: Einrichten von ContextHub für die Personalisierung mit AEM Sites
description: ContextHub ist ein Framework zum Speichern, Ändern und Darstellen von Kontextdaten. Mit der ContextHub-JavaScript-API können Sie auf Speicher zugreifen, um Daten bei Bedarf zu erstellen, zu aktualisieren und zu löschen. Daher stellt ContextHub eine Datenschicht auf Ihren Seiten dar. Auf dieser Seite wird beschrieben, wie Sie Ihren AEM Seiten einen ContextHub hinzufügen.
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 24%

---

# Einrichten von ContextHub für die Personalisierung {#set-up-contexthub}

ContextHub ist ein Framework zum Speichern, Ändern und Darstellen von Kontextdaten. Mit der ContextHub-JavaScript-API können Sie auf Speicher zugreifen, um Daten bei Bedarf zu erstellen, zu aktualisieren und zu löschen. Daher stellt ContextHub eine Datenschicht auf Ihren Seiten dar. Auf dieser Seite wird beschrieben, wie Sie Ihren AEM Seiten einen ContextHub hinzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>Wir verwenden die WKND-Referenz-Website für dieses Video und ist nicht Teil AEM Veröffentlichung. Sie können die [aktuelle version hier](https://github.com/adobe/aem-guides-wknd/releases).

Fügen Sie Ihren Seiten ContextHub hinzu, um die ContextHub-Funktionen zu aktivieren und eine Verknüpfung mit den ContextHub-JavaScript-Bibliotheken herzustellen. Die ContextHub-JavaScript-API bietet Zugriff auf die von ContextHub verwalteten Kontextdaten.

## Hinzufügen von ContextHub zu einer Seitenkomponente {#adding-contexthub-to-a-page-component}

Um die ContextHub-Funktionen zu aktivieren und eine Verknüpfung mit den ContextHub-JavaScript-Bibliotheken herzustellen, fügen Sie die `contexthub` -Komponente in `<head>` auf Ihrer Webseite. Der HTL-Code für Ihre Seitenkomponente ähnelt dem folgenden Beispiel:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Site-Konfiguration und ContextHub-Segmente {#site-configuration-and-contexthub-segments}

ContextHub enthält eine Segmentierungsmaschine, die Segmente verwaltet und bestimmt, welche Segmente für den aktuellen Kontext aufgelöst werden. Mehrere Segmente sind definiert. Sie können die JavaScript-API verwenden, um [aufgelöste Segmente bestimmen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Aktivieren Sie die ContextHub-Segmente für Ihre Site unter [[!UICONTROL Konfigurationsbrowser]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=de).

## Segmente erstellen {#create-segments}

Erstellen Sie AEM Segmente, die als Regeln für die Teaser dienen. Das heißt, sie definieren, wann Inhalte in einem Teaser auf einer Webseite angezeigt werden. Inhalte können dann entsprechend den jeweiligen Segmenten speziell auf die Bedürfnisse und Interessen des Besuchers abgestimmt werden.

## Zuweisen von Cloud-Konfiguration, Segmentpfad und ContextHub-Pfad zu Ihrer Site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Weisen Sie den Cloud-Konfigurationspfad, den Segmentierungspfad und den ContextHub-Pfad Ihrem Site-Stammknoten zu, damit Sie ein personalisiertes Erlebnis für Ihre Zielgruppe erstellen können. Mithilfe des ContextHub können Sie die Kontextdaten bearbeiten und Ihre aufgelösten Segmente testen.

![CRXDE Lite](assets/crx-de-properties.png)   

Weitere Informationen zu ContextHub und zur Segmentierung finden Sie unten:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Hinzufügen von ContextHub zur Seite und Zugreifen auf Speicher](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Grundlegendes zur Segmentierung](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Konfigurieren der Segmentierung mit ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
