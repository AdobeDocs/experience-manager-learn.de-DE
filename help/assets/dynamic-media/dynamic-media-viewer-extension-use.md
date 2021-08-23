---
title: Verwenden von Dynamic Media-Viewern mit Adobe Analytics und Adobe Launch
description: Mit der Erweiterung für Dynamic Media-Viewer für Adobe Launch und der Dynamic Media-Viewer-Version 5.13 können Kunden von Dynamic Media, Adobe Analytics und Adobe Launch Ereignisse und Daten verwenden, die für die Dynamic Media-Viewer in ihrer Adobe Launch-Konfiguration spezifisch sind.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 22%

---


# Verwenden von Dynamic Media-Viewern mit Adobe Analytics und Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Für Kunden mit Dynamic Media und Adobe Analytics können Sie jetzt die Nutzung von Dynamic Media-Viewern auf Ihrer Website mithilfe der Dynamic Media Viewer-Erweiterung verfolgen.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Führen Sie für diese Funktion Adobe Experience Manager im Dynamic Media Scene7-Modus aus. Sie müssen auch [Adobe Experience Platform Launch in Ihre AEM-Instanz](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html) integrieren.

Mit der Einführung der Dynamic Media Viewer-Erweiterung bietet Adobe Experience Manager jetzt erweiterte Analyseunterstützung für Assets, die mit Dynamic Media-Viewern (5.13) bereitgestellt werden, und bietet eine granularere Kontrolle über die Ereignisverfolgung, wenn ein Dynamic Media-Viewer auf einer Sites-Seite verwendet wird.

Wenn Sie bereits über AEM Assets und Sites verfügen, können Sie Ihre Launch-Eigenschaft in Ihre AEM-Autoreninstanz integrieren. Sobald Ihre Launch-Integration mit Ihrer Website verknüpft ist, können Sie Ihrer Seite Komponenten für dynamische Medien hinzufügen, bei denen die Ereignisverfolgung für Viewer aktiviert ist.

Für Kunden, die nur AEM Assets verwenden, oder Dynamic Media Classic können Benutzer Einbettungscode für einen Viewer abrufen und ihn zur Seite hinzufügen. Launch-Skriptbibliotheken können dann der Seite manuell zur Verfolgung von Viewer-Ereignissen hinzugefügt werden.

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
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
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
         <td> %event.detail.dm.LOAD.company% </td>
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
         <td> %event.detail.dm.MILESTONE.milestone% </td>
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
         <td> SWATCH </td>
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

* [Integrieren von Adobe Experience Manager mit Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [Ausführen von Adobe Experience Manager im Dynamic Media Scene7-Modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integrieren von Dynamic Media-Viewern mit Adobe Analytics und Adobe Experience Platform Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
