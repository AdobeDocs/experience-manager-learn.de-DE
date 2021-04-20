---
title: Verwenden von Dynamic Media Viewern mit Adobe Analytics und Adobe Launch
description: Mit der Erweiterung für Dynamic Media-Viewer für Adobe Launch und der Dynamic Media-Viewer-Version 5.13 können Kunden von Dynamic Media, Adobe Analytics und Adobe Launch Ereignisse und Daten verwenden, die für die Dynamic Media-Viewer in ihrer Adobe Launch-Konfiguration spezifisch sind.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 26%

---


# Verwenden von Dynamic Media Viewern mit Adobe Analytics und Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Für Kunden mit Dynamic Media und Adobe Analytics können Sie jetzt die Nutzung von Dynamic Media Viewern auf Ihrer Website mit der Dynamic Media Viewer Extension verfolgen.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Führen Sie für diese Funktion Adobe Experience Manager im Dynamic Media Scene7-Modus aus. Sie müssen außerdem [Adobe Experience Platform Launch in Ihre AEM-Instanz](https://docs.adobe.com/content/help/de/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html) integrieren.

Mit der Einführung der Dynamic Media Viewer-Erweiterung bietet Adobe Experience Manager jetzt erweiterte Analytics-Unterstützung für Assets, die mit Dynamic Media-Viewern (5.13) bereitgestellt werden, und bietet eine genauere Kontrolle über die Verfolgung von Ereignissen, wenn ein Dynamic Media Viewer auf einer Siteseite verwendet wird.

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
* [Adobe Experience Manager im Dynamic Media Scene7-Modus ausführen](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integrieren von Dynamic Media-Viewern mit Adobe Analytics und Adobe Experience Platform Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
