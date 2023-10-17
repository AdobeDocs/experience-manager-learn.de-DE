---
title: Verwenden des Panorama- und Vertikalbild-Viewers mit AEM Assets Dynamic Media
description: Dynamic Media Viewer-Verbesserungen in AEM 6.4 umfassen das Hinzufügen des Viewers für Panoramabilder, für Panoramabilder mit virtueller Realität und für vertikale Bilder. Der Panorama-Viewer bietet eine einfache Möglichkeit, ein ansprechendes, interaktives Erlebnis des Zimmers, des Grundstücks, des Standorts oder der Landschaft ohne benutzerdefinierte Entwicklung bereitzustellen.
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '533'
ht-degree: 100%

---

# Verwenden des Panorama- und Vertikalbild-Viewers mit AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Dynamic Media Viewer-Verbesserungen in AEM 6.4 umfassen das Hinzufügen des Viewers für Panoramabilder, für Panoramabilder mit virtueller Realität und für vertikale Bilder. Der Panorama-Viewer bietet eine einfache Möglichkeit, ein ansprechendes, interaktives Erlebnis des Zimmers, des Grundstücks, des Standorts oder der Landschaft ohne benutzerdefinierte Entwicklung bereitzustellen.

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>Video setzt voraus, dass die AEM-Instanz im S7-Modus von Dynamic Media ausgeführt wird. [Anweisungen zum Einrichten von AEM mit Dynamic Media finden sich hier.](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Panorama- und Panorama-VR-Viewer

Ein Bild wird aufgrund seiner Seitenverhältnisse oder seiner Schlüsselwörter als Panoramabild betrachtet. Standardmäßig wird ein Bild mit einem Seitenverhältnis von 2 als Panoramabild betrachtet. Vorgaben für den Panoramabilder-Viewer sind für eine Bildvorschau verfügbar, wenn sie die oben genannten Kriterien erfüllt. Das Kriterium des Seitenverhältnisses für Panoramabilder kann in der DMS7-Konfiguration des Unternehmens geändert werden, indem die doppelte Eigenschaft s7PanoramicAR unter /conf/global/settings/cloudconfigs/dmscene7/jcr:content angegeben wird. Die Suchbegriffe werden in der Eigenschaft dc:keyword des Metadatenknotens des Assets gespeichert. Wenn die Suchbegriffe eine der folgenden Kombinationen enthalten:

* rechteckig gleichseitig,
* kugelförmig + panoramisch,
* kugelförmig + Panorama,

wird es unabhängig vom Seitenverhältnis als Panoramabild-Asset betrachtet.

## Vertikalbild-Viewer

Bei horizontalen Farbfeldern sind die Farbfelder je nach Desktop-Bildschirmgröße der Benutzenden manchmal erst sichtbar, wenn die Seite nach unten gescrollt wird. Durch die Verwendung des Vertikalbild-Viewers und die Platzierung vertikaler Farbfelder wird sichergestellt, dass die Farbfelder unabhängig von der Bildschirmgröße sichtbar sind. Außerdem wird die Größe des Hauptbilds maximiert. Bei horizontalen Farbfeldern war es erforderlich, Platz auf der Seite zu reservieren, um sicherzustellen, dass sie mit hoher Wahrscheinlichkeit sichtbar sind, und um die Größe des Hauptbilds zu verringern. Bei einem vertikalen Layout müssen Sie sich nicht darum kümmern, diesen Bereich zuzuweisen, und können daher die Größe des Hauptbilds maximieren.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Panorama- und VR-Viewer</td>
   <td>Vertikalbild-Viewer</td>
  </tr>
  <tr>
   <td>Dynamic Media-Ausführungsmodus</td>
   <td>Nur Scene7-Modus von Dynamic Media</td>
   <td>DMS7 und Dynamic Media</td>
  </tr>
  <tr>
   <td>Nutzungsszenario</td>
   <td><p>Der Panorama-Viewer und der Virtual-Reality-Viewer bieten Benutzenden ein ansprechenderes Erlebnis. Benutzende können sich ein Hotelzimmer ansehen, bevor sie eine Buchung vornehmen, oder sich ein Mietobjekt ansehen, ohne einen Termin planen zu müssen. Benutzende können einen Standort und viele weitere Möglichkeiten ansehen. Der Hauptschwerpunkt hier besteht darin, Verbraucherinnen und Verbrauchern ein besseres Erlebnis beim Besuch der Website zu bieten und dadurch die Konversionsrate zu erhöhen.</p> <p> </p> </td> 
   <td><p>Mit dem Vertikalbild-Viewer lässt sich das Anzeigeerlebnis von Produktbildern maximieren, um den Verbraucherinnen und Verbrauchern die bestmögliche Darstellung des Produkts zu bieten, wodurch Konvertierungen gefördert und Rückgaben minimiert werden.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Verfügbar </td>
   <td>Vorkonfiguriert</td>
   <td>Vorkonfiguriert</td>
  </tr>
 </tbody>
</table>

[Konfigurieren von Dynamic Media im Scene7-Modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Konfigurieren von Dynamic Media im Hybridmodus](https://helpx.adobe.com/de/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Panorama-Viewer arbeiten mit Panoramabildern und sind nicht für die Verwendung mit normalen Bildern vorgesehen.
