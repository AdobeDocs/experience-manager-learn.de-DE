---
title: Verwendung von Dynamic Media 360-Videos und benutzerdefinierten Video-Miniaturansichten mit AEM Assets
description: Die Dynamic Media Viewer-Verbesserungen in AEM 6.5 umfassen die Unterstützung von 360-Grad-Video-Rendern, 360-Grad-Media-Viewern (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Videominiaturen auszuwählen.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 691
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 100%

---

# Verwendung von Dynamic Media 360-Videos und benutzerdefinierten Video-Miniaturansichten mit AEM Assets

Die Dynamic Media Viewer-Verbesserungen in AEM 6.5 umfassen die Unterstützung von 360-Grad-Video-Rendern, 360-Grad-Media-Viewern (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Videominiaturen auszuwählen.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>Video setzt voraus, dass die AEM-Instanz im S7-Modus von Dynamic Media ausgeführt wird.  [Anweisungen zum Einrichten von AEM mit Dynamic Media finden Sie hier](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Wenn Sie ein Video hochladen, verarbeitet Dynamic Media dieses standardmäßig als 360-Grad-Video, wenn das Seitenverhältnis 2:1 beträgt. Das Verhältnis von Breite zu Höhe ist also 2:1.

>[!NOTE]
>
>Die Media-Komponenten von Dynamic Media 360 unterstützen nur 360-Videos.

## Dynamic Media 360-Videos

360-Grad-Videos, auch kugelförmige Videos genannt, sind Videoaufnahmen, bei denen eine Ansicht in jeder Richtung gleichzeitig aufgezeichnet wird, die mit einer Omnidirektionalkamera oder Kamerasammlung aufgenommen wurden. Bei der Wiedergabe auf einem Flachbildschirm kann die Benutzerin bzw. der Benutzer die Blickrichtung steuern, und die Wiedergabe auf mobilen Geräten nutzt in der Regel die integrierte Gyroskopsteuerung. Damit können Sie über die Grenzen der Einzelfotografie hinausgehen. Marketing-Fachkräfte können Benutzenden mithilfe von 360-Videos ein ansprechendes Erlebnis bieten. Fangen wir an. Das Kriterium des Seitenverhältnisses für Panoramabilder kann in der DMS7-Konfiguration des Unternehmens geändert werden, indem die doppelte Eigenschaft „s7PanoramicAR“ unter „/conf/global/settings/cloudconfigs/dmscene7/jcr:content“ angegeben wird.

## Dynamic Media 360-Videos

Dynamic Media-Video unterstützt jetzt die Auswahl einer benutzerdefinierten Miniaturansicht für Ihr Video. Eine Benutzerin bzw. ein Benutzer kann entweder ein vorhandenes Asset aus AEM Assets auswählen oder ein Videobild als Miniaturansicht auswählen.

## Dynamic Media 360-Viewer

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social-Viewer**</td>
      <td>**Video360VR-Viewer**</td>
   </tr>
   <tr>
      <td>Dynamic Media-Ausführungsmodus</td>
      <td>Nur Scene7-Modus von Dynamic Media</td>
      <td>Nur Scene7-Modus von Dynamic Media<br>
 <br>
      </td>
   </tr>
   <tr>
      <td>Nutzungsszenario</td>
      <td>
         <p>Für Websites und Geräte, die kein Gyroskop unterstützen</p>
         <p> </p>
      </td>
      <td>
         <p>Bietet ein Virtual Reality-Erlebnis für ein Gerät, das Gyroskop unterstützt </p>
      </td>
   </tr>
   <tr>
      <td>Audio – Stereo-Modus</td>
      <td>Nein</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Videowiedergabe</td>
      <td>Ja</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Blickwinkel-Navigation</td>
      <td>
         <ul>
            <li>Bewegen der Maus (auf Desktop-Systemen)</li>
            <li>Wischen (Touch-Geräte)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Maus- und Ziehoptionen sind deaktiviert</li>
            <li>Verwendet integriertes Gyroskop</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5-Player</td>
      <td>Ja</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Social-Sharing-Optionen</td>
      <td>Ja</td>
      <td>Nein</td>
   </tr>
</tbody>
</table>

## Zusätzliche Ressourcen{#additional-resources}

[Konfigurieren von Dynamic Media im Scene7-Modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
