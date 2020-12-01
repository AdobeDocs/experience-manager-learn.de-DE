---
title: Verwenden von Viewern für dynamische Medien mit Adobe Analytics und Adoben-Start
seo-title: Verwenden von Viewern für dynamische Medien mit Adobe Analytics und Adoben-Start
description: Mit der Erweiterung für Dynamic Media-Viewer für Adobe Launch und der Dynamic Media-Viewer-Version 5.13 können Kunden von Dynamic Media, Adobe Analytics und Adobe Launch Ereignisse und Daten verwenden, die für die Dynamic Media-Viewer in ihrer Adobe Launch-Konfiguration spezifisch sind.
seo-description: 'Mit der Erweiterung für Dynamic Media-Viewer für Adobe Launch und der Dynamic Media-Viewer-Version 5.13 können Kunden von Dynamic Media, Adobe Analytics und Adobe Launch Ereignisse und Daten verwenden, die für die Dynamic Media-Viewer in ihrer Adobe Launch-Konfiguration spezifisch sind. '
sub-product: dynamic-media, analytics
feature: asset-insights, media-player
topics: images, videos, renditions, authoring, integrations, publishing
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 32%

---


# Verwenden von Viewern für dynamische Medien mit Adobe Analytics und Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Für Kunden mit dynamischen Medien und Adobe Analytics können Sie jetzt die Nutzung von Dynamischen Medien-Viewern auf Ihrer Website mit der Dynamischen Media Viewer-Erweiterung verfolgen.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Führen Sie für diese Funktion Adobe Experience Manager im Scene7-Modus für dynamische Medien aus. Sie müssen außerdem [Adobe Experience Platform Launch in Ihre AEM-Instanz](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html) integrieren.

Mit der Einführung der Dynamischen Medien-Viewer-Erweiterung bietet Adobe Experience Manager jetzt erweiterte Analyseunterstützung für Assets, die mit Dynamischen Media-Viewern (5.13) bereitgestellt werden, und bietet so eine genauere Kontrolle über die Verfolgung von Ereignissen, wenn ein Dynamischer Media Viewer auf einer Siteseite verwendet wird.

Wenn Sie bereits über AEM Assets und Sites verfügen, können Sie Ihre Launch-Eigenschaft in Ihre AEM Autoreninstanz integrieren. Sobald Ihre Startintegration mit Ihrer Website verknüpft ist, können Sie Ihrer Seite eine dynamische Medienkomponente hinzufügen, bei der die Ereignis-Verfolgung für Viewer aktiviert ist.

Für Kunden, die nur AEM Assets oder Dynamic Media Classic verwenden, kann der Benutzer Einbettungscode für einen Viewer abrufen und dieser Seite hinzufügen. Startskriptbibliotheken können dann zur Verfolgung von Viewer-Ereignissen manuell zur Seite hinzugefügt werden.

In der folgenden Tabelle sind die Dynamic Media-Viewer-Ereignisse sowie die unterstützten Argumente aufgeführt:

<table>
   <tbody>
      <tr>
         <td>Name des Viewer-Ereignisses</td>
         <td>Argumentverweis</td>
      </tr>
      <tr>
         <td> HÄUFIG </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> BANNER <br></td>
         <td> %event.detail.dm.BANER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> ELEMENT </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> LADEN </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.Firma% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> METADATEN </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> MILESTONE </td>
         <td> %event.detail.dm.MIlESTONE.milestone% </td>
      </tr>
      <tr>
         <td> SEITE </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> PAUSE </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> PLAY </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> SPIN </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> STOP </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> FARBE </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> ZOOM </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## Zusätzliche Ressourcen{#additional-resources}

* [Integration von Adobe Experience Manager mit Adobe Launch](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Ausführen von Adobe Experience Manager im Scene7-Modus für dynamische Medien](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integrieren von Dynamic Media-Viewern mit Adobe Analytics und Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
