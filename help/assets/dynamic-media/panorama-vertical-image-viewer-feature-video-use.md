---
title: Verwenden des Panorama- und Vertikalbild-Viewers mit AEM Assets Dynamic Media
description: Dynamic Media Viewer-Verbesserungen in AEM 6.4 umfassen das Hinzufügen von Viewer für Panoramabilder, Bild-Viewer für Panoramabilder mit virtueller Realität und Viewer für vertikale Bilder. Der Panorama-Viewer bietet eine einfache Möglichkeit, ein ansprechendes, interaktives Erlebnis des Zimmers, der Eigenschaft, des Standorts oder der Landschaft ohne benutzerdefinierte Entwicklung bereitzustellen.
sub-product: dynamic-media
feature: Videoprofile, Videoprofile, 360-Grad-VR-Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 3%

---


# Verwenden des Panorama- und Vertikalbild-Viewers mit AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Dynamic Media Viewer-Verbesserungen in AEM 6.4 umfassen das Hinzufügen von Viewer für Panoramabilder, Bild-Viewer für Panoramabilder mit virtueller Realität und Viewer für vertikale Bilder. Der Panorama-Viewer bietet eine einfache Möglichkeit, ein ansprechendes, interaktives Erlebnis des Zimmers, der Eigenschaft, des Standorts oder der Landschaft ohne benutzerdefinierte Entwicklung bereitzustellen.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>Video setzt voraus, dass Ihre AEM-Instanz im Dynamic Media S7-Modus ausgeführt wird. [Anweisungen zum Einrichten von AEM mit Dynamic Media finden Sie hier.](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## VR-Viewer für Panoramabilder und Panoramabilder

Ein Bild wird aufgrund seines Seitenverhältnisses oder seiner Schlüsselwörter als panoramisch betrachtet. Standardmäßig wird ein Bild mit einem Seitenverhältnis von 2 als Panoramabild betrachtet. Viewer-Vorgaben für Panoramabilder werden für eine Bildvorschau verfügbar, wenn sie die oben genannten Kriterien erfüllen. Das Kriterium des Seitenverhältnisses für Panoramabilder kann in der DMS7-Konfiguration des Unternehmens geändert werden, indem die doppelte Eigenschaft s7PanoramicAR unter /conf/global/settings/cloudconfigs/dmscene7/jcr:content angegeben wird. Die Suchbegriffe werden in der Eigenschaft dc:keyword des Metadatenknotens des Assets gespeichert. Wenn die Suchbegriffe eine der folgenden Kombinationen enthalten:

* Äquirechteckig,
* kugelförmig + panoramisch,
* Kugelpanorama,

es wird unabhängig vom Seitenverhältnis als Panorama-Bild-Asset betrachtet.

## Vertikaler Bild-Viewer

Bei horizontalen Farbfeldern sind die Farbfelder je nach Desktop-Bildschirmgröße des Benutzers manchmal erst sichtbar, wenn der Benutzer einen Bildlauf auf der Seite nach unten durchführt. Durch die Verwendung des vertikalen Bild-Viewers und die Platzierung vertikaler Farbfelder wird sichergestellt, dass die Farbfelder unabhängig von der Bildschirmgröße sichtbar sind. Außerdem wird die Größe des Hauptbilds maximiert. Bei horizontalen Farbfeldern war es erforderlich, Platz auf der Seite zu reservieren, um sicherzustellen, dass sie mit hoher Wahrscheinlichkeit sichtbar sind und die Größe des Hauptbilds verringern würden. Bei einem vertikalen Layout müssen Sie sich nicht darum kümmern, diesen Bereich zuzuweisen, und können daher die Größe des Hauptbilds maximieren.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Panorama- und VR-Viewer</td>
   <td>Vertikaler Bild-Viewer</td>
  </tr>
  <tr>
   <td>Dynamic Media-Ausführungsmodus</td>
   <td>Nur Dynamic Media Scene7-Modus</td>
   <td>DMS7 und Dynamic Media</td>
  </tr>
  <tr>
   <td>Nutzungsszenario   </td>
   <td><p>Panorama-Viewer und Virtual Reality-Viewer bieten Benutzern ein ansprechenderes Erlebnis. Ein Benutzer kann ein Hotelzimmer auschecken, bevor er eine Buchung vornimmt, oder ein Mietobjekt auschecken, ohne einen Termin planen zu müssen. Ein Benutzer kann einen Standort und viele weitere Möglichkeiten auschecken. Der Hauptschwerpunkt hier besteht darin, Verbrauchern ein besseres Erlebnis beim Besuch Ihrer Website zu bieten und schließlich Ihre Konversionsrate zu erhöhen.</p> <p> </p> </td> 
   <td><p>Mit dem vertikalen Bild-Viewer können Sie das Anzeigeerlebnis von Produktbildern maximieren, um den Verbrauchern die bestmögliche Darstellung des Produkts zu bieten, wodurch Konversionen gefördert und die Renditen minimiert werden.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Verfügbar </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Konfigurieren von Dynamic Media im Scene7-Modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Konfigurieren von Dynamic Media im Hybridmodus](https://helpx.adobe.com/de/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Panorama-Viewer arbeiten mit Panoramabildern und sind nicht für die Verwendung mit normalen Bildern vorgesehen.
