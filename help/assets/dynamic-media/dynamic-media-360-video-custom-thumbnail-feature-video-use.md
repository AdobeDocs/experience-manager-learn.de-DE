---
title: Verwenden von Dynamic Media 360 Videos und benutzerdefinierten Video-Miniaturbildern mit AEM Assets
description: Zu den Verbesserungen des Dynamic Media Viewers in AEM 6.5 gehören die zusätzliche Unterstützung von 360-Video-Rendering, 360-Medien-Viewern (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Videominiaturen auszuwählen.
sub-product: dynamic-media
feature: Videoprofile
version: 6.3, 6.4, 6.5
topic: Content Management
role: Geschäftspraktiker
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 5%

---


# Verwenden von Dynamic Media 360 Videos und benutzerdefinierten Video-Miniaturbildern mit AEM Assets

Zu den Verbesserungen des Dynamic Media Viewers in AEM 6.5 gehören die zusätzliche Unterstützung von 360-Video-Rendering, 360-Medien-Viewern (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Videominiaturen auszuwählen.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>Video geht davon aus, dass Ihre AEM Instanz im Dynamic Media S7-Modus ausgeführt wird.  [Anweisungen zur Einrichtung von AEM mit Dynamic Media finden Sie hier](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Beim Hochladen eines Videos verarbeitet Dynamic Media das Filmmaterial standardmäßig als 360-Grad-Video, wenn das Seitenverhältnis 2:1 beträgt. das Verhältnis von Breite zu Höhe ist 2:1.

>[!NOTE]
>
>Dynamic Media 360-Medienkomponenten unterstützen nur 360 Videos.

## Dynamic Media 360 Videos

360-Grad-Videos, auch als &quot;sphärischen Videos&quot;bezeichnet, sind Videoaufnahmen, bei denen eine Ansicht in jeder Richtung gleichzeitig aufgezeichnet wird, die mit einer Allrichtungskamera oder einer Kamerasammlung aufgenommen wurden. Während der Wiedergabe auf einem Flachbildschirm hat der Benutzer die Steuerung der Anzeigerichtung, und die Wiedergabe auf Mobilgeräten nutzt in der Regel die integrierte Gyroskop-Steuerung.  Damit können Sie über die Grenzen der Einzelfotografie hinausgehen. Marketingexperten können mithilfe von 360 Videos eine ansprechende Benutzererfahrung bieten.  Fangen wir an! Das Seitenverhältnis des Panoramabilds kann in der DMS7-Konfiguration der Firma geändert werden, indem die Dublette-Eigenschaft s7PanoramicAR unter /conf/global/settings/cloudconfigs/dmscene7/jcr:content angegeben wird.

## Dynamic Media 360 Videos

Dynamic Media-Video unterstützt jetzt die Auswahl einer benutzerdefinierten Miniaturansicht für Ihr Video. Ein Benutzer kann entweder ein vorhandenes Asset aus AEM Assets auswählen oder einen Videobild als Miniaturansicht auswählen.

## Dynamische 360-Medien-Viewer

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR Viewer**</td>
   </tr>
   <tr>
      <td>Dynamic Media-Ausführungsmodus</td>
      <td>Nur Dynamic Media Scene7-Modus</td>
      <td>Nur Dynamic Media Scene7 Mode<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Nutzungsszenario </td>
      <td>
         <p>Für Websites und Geräte, die Gyroskop nicht unterstützen</p>
         <p> </p>
      </td>
      <td>
         <p>Bietet eine virtuelle Realität für ein Gerät, das Gyroskop unterstützt </p>
      </td>
   </tr>
   <tr>
      <td>Audio - Stereomodus</td>
      <td>Nein</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Videowiedergabe</td>
      <td>Ja</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Ansicht</td>
      <td>
         <ul>
            <li>Maus ziehen (auf Desktop-Systemen)</li>
            <li>Blättern (Touch-Geräte)</li>
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
      <td>Social Sharing-Optionen</td>
      <td>Ja</td>
      <td>Nein</td>
   </tr>
</tbody>
</table>

## Zusätzliche Ressourcen{#additional-resources}

[Konfigurieren von Dynamic Media im Scene7-Modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
