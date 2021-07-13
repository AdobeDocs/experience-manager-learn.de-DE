---
title: Verwenden von Dynamic Media 360-Videos und benutzerdefinierten Videominiaturen mit AEM Assets
description: Dynamic Media Viewer-Verbesserungen in AEM 6.5 umfassen die Unterstützung von 360-Grad-Video-Rendering, 360-Grad-Media-Viewern (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Videominiaturen auszuwählen.
sub-product: dynamic-media
feature: Videoprofile
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 5%

---


# Verwenden von Dynamic Media 360-Videos und benutzerdefinierten Videominiaturen mit AEM Assets

Dynamic Media Viewer-Verbesserungen in AEM 6.5 umfassen die Unterstützung von 360-Grad-Video-Rendering, 360-Grad-Media-Viewern (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Videominiaturen auszuwählen.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>Video setzt voraus, dass Ihre AEM-Instanz im Dynamic Media S7-Modus ausgeführt wird.  [Anweisungen zum Einrichten von AEM mit Dynamic Media finden Sie hier](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Wenn Sie ein Video hochladen, verarbeitet Dynamic Media dieses standardmäßig als 360-Grad-Video, wenn das Seitenverhältnis 2:1 beträgt. Das Verhältnis von Breite zu Höhe ist also 2:1.

>[!NOTE]
>
>Dynamic Media 360 Media-Komponenten unterstützen nur 360 Videos.

## Videos zu Dynamic Media 360

360-Grad-Videos, auch kugelförmige Videos genannt, sind Videoaufnahmen, bei denen eine Ansicht in jeder Richtung gleichzeitig aufgezeichnet wird, die mit einer Omnidirektionalkamera oder Kamerasammlung aufgenommen wurden. Während der Wiedergabe auf einer flachen Anzeige hat der Benutzer die Kontrolle über die Anzeigerichtung, und die Wiedergabe auf Mobilgeräten nutzt in der Regel die integrierte Gyroskop-Steuerung.  Es ermöglicht Ihnen, über die Grenzen der Einzelfotografie hinaus zu erweitern. Marketingexperten können Benutzern mithilfe von 360 Videos ein ansprechendes Erlebnis bieten.  Fangen wir an! Das Kriterium des Seitenverhältnisses für Panoramabilder kann in der DMS7-Konfiguration des Unternehmens geändert werden, indem die doppelte Eigenschaft s7PanoramicAR unter /conf/global/settings/cloudconfigs/dmscene7/jcr:content angegeben wird.

## Videos zu Dynamic Media 360

Dynamic Media-Video unterstützt jetzt die Auswahl einer benutzerdefinierten Miniaturansicht für Ihr Video. Ein Benutzer kann entweder ein vorhandenes Asset aus AEM Assets auswählen oder einen Videobild als Miniaturansicht auswählen.

## Dynamic 360 Media-Viewer

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social-Viewer**</td>
      <td>**Video360VR-Viewer**</td>
   </tr>
   <tr>
      <td>Dynamic Media-Ausführungsmodus</td>
      <td>Nur Dynamic Media Scene7-Modus</td>
      <td>Nur Dynamic Media Scene7-Modus<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Nutzungsszenario   </td>
      <td>
         <p>Für Websites und Geräte, die kein Gyroskop unterstützen</p>
         <p> </p>
      </td>
      <td>
         <p>Bietet ein virtuelles Reality-Erlebnis für ein Gerät, das Gyroskop unterstützt </p>
      </td>
   </tr>
   <tr>
      <td>Audio - Stereo-Modus</td>
      <td>Nein</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Videowiedergabe</td>
      <td>Ja</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Navigation in der Ansicht</td>
      <td>
         <ul>
            <li>Ziehen Sie die Maus (auf Desktop-Systemen).</li>
            <li>Wischen (Touch-Geräte)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Die Optionen für Maus und Ziehen sind deaktiviert.</li>
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
