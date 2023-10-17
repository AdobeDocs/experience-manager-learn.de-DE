---
title: Integrieren von Dynamic Media-Viewern und Adobe Analytics bzw. Adobe Launch
description: Mit der Erweiterung für Dynamic Media-Viewer für Adobe Experience Platform Launch und der neuen Dynamic Media-Viewer-Version 5.13 können Kundinnen und Kunden von Dynamic Media, Adobe Analytics und Adobe Experience Platform Launch Ereignisse und Daten verwenden, die für die Dynamic Media-Viewer in ihrer Adobe Experience Platform Launch-Konfiguration spezifisch sind.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '360'
ht-degree: 100%

---

# Integrieren von Dynamic Media-Viewern und Adobe Analytics bzw. Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Für Kundinnen und Kunden mit Dynamic Media und Adobe Analytics können Sie jetzt die Nutzung von Dynamic Media-Viewern auf Ihrer Website mithilfe der Dynamic Media Viewer-Erweiterung verfolgen.

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> Führen Sie für diese Funktionalität Adobe Experience Manager im Dynamic Media Scene7-Modus aus. Sie müssen außerdem [Adobe Experience Platform Launch in Ihre AEM-Instanz integrieren](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=de).

Mit der Einführung der Dynamic Media Viewer-Erweiterung bietet Adobe Experience Manager jetzt erweiterte Analyseunterstützung für Assets, die mit Dynamic Media-Viewern (5.13) bereitgestellt werden, was eine granularere Kontrolle über die Ereignisverfolgung bedeutet, wenn ein Dynamic Media-Viewer auf einer Sites-Seite verwendet wird.

Wenn Sie bereits über AEM Assets und Sites verfügen, können Sie Ihre Launch-Eigenschaft in Ihre AEM-Autoreninstanz integrieren. Sobald Ihre Launch-Integration mit Ihrer Website verknüpft ist, können Sie Ihrer Seite Komponenten für dynamische Medien hinzufügen, bei denen die Ereignisverfolgung für Viewer aktiviert ist.

Für Kundinnen und Kunden, die nur AEM Assets bzw. die Dynamic Media Classic verwenden, können Benutzende Einbettungs-Code für einen Viewer abrufen und ihn zur Seite hinzufügen. Launch-Skriptbibliotheken können dann der Seite manuell zur Verfolgung von Viewer-Ereignissen hinzugefügt werden.

In der folgenden Tabelle sind die Dynamic Media Viewer-Ereignisse sowie die unterstützten Argumente aufgeführt:

<table>
   <tbody>
      <tr>
         <td>Name des Viewer-Ereignisses</td>
         <td>Argumentverweis</td>
      </tr>
      <tr>
         <td> Allgemein </td>
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
         <td> Banner <br></td>
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
         <td> Element </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> Laden </td>
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
         <td> Metadaten </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> Meilenstein </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> Seite </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> Anhalten </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> Wiedergeben </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> Drehen </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> Stoppen </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> Tauschen </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> Farbfeld </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> Ziel </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> Zoom </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## Zusätzliche Ressourcen{#additional-resources}

* [Integrieren von Adobe Experience Manager mit Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=de)
* [Ausführen von Adobe Experience Manager im Scene7-Modus von Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=de)
* [Integrieren von Dynamic Media-Viewern mit Adobe Analytics und Adobe Experience Platform Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html?lang=de)
