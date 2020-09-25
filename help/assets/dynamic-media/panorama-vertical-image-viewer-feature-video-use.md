---
title: Verwenden von Panorama- und Vertikal-Bild-Viewern mit dynamischen AEM Assets-Medien
seo-title: Verwenden von Panorama- und Vertikal-Bild-Viewern mit dynamischen AEM Assets-Medien
description: Zu den Verbesserungen des dynamischen Media Viewers in AEM 6.4 zählen der Panorama-Bild-Viewer, der Panorama-Viewer für die virtuelle Realität und der vertikale Bild-Viewer. Der Panorama-Viewer bietet eine einfache Möglichkeit, ein eindrucksvolles Erlebnis im Raum, der Immobilie, der Lage oder der Landschaft ohne eigene Entwicklung zu bieten.
seo-description: Zu den Verbesserungen des dynamischen Media Viewers in AEM 6.4 zählen der Panorama-Bild-Viewer, der Panorama-Viewer für die virtuelle Realität und der vertikale Bild-Viewer. Der Panorama-Viewer bietet eine einfache Möglichkeit, ein eindrucksvolles Erlebnis im Raum, der Immobilie, der Lage oder der Landschaft ohne eigene Entwicklung zu bieten.
sub-product: dynamic-media
feature: video-profiles, video-profiles, vr-360
topics: videos, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# Verwenden von Panorama- und Vertikal-Bild-Viewern mit dynamischen AEM Assets-Medien{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Zu den Verbesserungen des dynamischen Media Viewers in AEM 6.4 zählen der Panorama-Bild-Viewer, der Panorama-Viewer für die virtuelle Realität und der vertikale Bild-Viewer. Der Panorama-Viewer bietet eine einfache Möglichkeit, ein eindrucksvolles Erlebnis im Raum, der Immobilie, der Lage oder der Landschaft ohne eigene Entwicklung zu bieten.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>Video geht davon aus, dass Ihre AEM-Instanz im Modus &quot;Dynamische Medien - S7&quot;ausgeführt wird. [Anweisungen zum Einrichten von AEM mit dynamischen Medien finden Sie hier.](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Panoramaanzeige und Panoramaanzeige

Ein Bild wird aufgrund des Seitenverhältnisses oder der Schlüsselwörter als panoramisch betrachtet. Standardmäßig wird ein Bild mit einem Seitenverhältnis von 2 als Panoramabild betrachtet. Panorama-Image-Viewer-Vorgaben stehen für eine Vorschau zur Verfügung, wenn sie die oben genannten Kriterien erfüllen. Das Seitenverhältnis des Panoramabilds kann in der DMS7-Konfiguration der Firma geändert werden, indem die Dublette-Eigenschaft s7PanoramicAR unter /conf/global/settings/cloudconfigs/dmscene7/jcr:content angegeben wird. Die Suchbegriffe werden in der Eigenschaft dc:keyword des Metadaten-Knotens des Assets gespeichert. Wenn die Suchbegriffe eine der folgenden Kombinationen enthalten:

* gleichwertig,
* kugelförmig + panoramisch,
* kugelförmig + Panorama,

es wird unabhängig vom Seitenverhältnis als Panorama-Bild-Asset betrachtet.

## Vertikaler Bild-Viewer

Bei horizontalen Farbfeldern sind die Farbfelder je nach Desktop-Bildschirmgröße des Benutzers manchmal erst sichtbar, wenn der Benutzer einen Bildlauf nach unten durchführt. Durch die Verwendung des vertikalen Bild-Viewers und die Platzierung vertikaler Farbfelder wird sichergestellt, dass die Farbfelder unabhängig von der Bildschirmgröße sichtbar sind. Außerdem wird die Größe des Hauptbilds maximiert. Bei horizontalen Farbfeldern war es notwendig, Platz auf der Seite zu reservieren, um sicherzustellen, dass sie mit hoher Wahrscheinlichkeit sichtbar sind und die Größe des Hauptbilds verringern würden. Bei einem vertikalen Layout müssen Sie sich keine Gedanken darüber machen, diesen Bereich zuzuweisen, und können daher die Größe des Hauptbilds maximieren.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Panorama- und VR-Viewer</td>
   <td>Vertikaler Bild-Viewer</td>
  </tr>
  <tr>
   <td>Dynamischer Medienlaufmodus</td>
   <td>Nur Scene7-Modus für dynamische Medien</td>
   <td>DMS7 und dynamische Medien</td>
  </tr>
  <tr>
   <td>Nutzungsszenario      </td>
   <td><p>Panoramabilder und der Viewer für die virtuelle Realität bieten Benutzern ein ansprechenderes Erlebnis. Ein Benutzer kann ein Hotelzimmer auschecken, bevor er eine Buchung vornimmt, oder eine Mietwohnung auschecken, ohne einen Termin planen zu müssen. Ein Benutzer kann einen Ort und viele weitere Möglichkeiten auschecken. Der Hauptschwerpunkt liegt hier darin, dem Verbraucher beim Besuch Ihrer Website ein besseres Erlebnis zu bieten und schließlich Ihren Konversionsrate zu erhöhen.</p> <p> </p> </td> 
   <td><p>Der vertikale Bild-Viewer unterstützt Sie bei der Maximierung des Ansichtserlebnisses von Produktbildern, um den Verbrauchern die bestmögliche Darstellung des Produkts zu geben, wodurch Konversionen gefördert und die Renditen minimiert werden.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Verfügbar </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Dynamische Medien im Scene7-Modus konfigurieren](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Konfigurieren dynamischer Medien im Hybridmodus](https://helpx.adobe.com/de/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Panorama-Viewer arbeiten mit Panoramabildern und sind nicht für die Verwendung mit normalen Bildern vorgesehen.
