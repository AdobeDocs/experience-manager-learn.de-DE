---
title: Verwenden von Videos mit dynamischen Medien 360 und benutzerdefinierten Videominiaturen mit AEM Assets
seo-title: Verwenden von Videos mit dynamischen Medien 360 und benutzerdefinierten Videominiaturen mit AEM Assets
description: Zu den Verbesserungen des dynamischen Media Viewers in AEM 6.5 gehören die zusätzliche Unterstützung für das Rendern von 360 Videos, 360 Medien-Viewer (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Video-Miniaturansichten auszuwählen.
seo-description: Zu den Verbesserungen des dynamischen Media Viewers in AEM 6.5 gehören die zusätzliche Unterstützung für das Rendern von 360 Videos, 360 Medien-Viewer (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Video-Miniaturansichten auszuwählen.
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# Verwenden von Videos mit dynamischen Medien 360 und benutzerdefinierten Videominiaturen mit AEM Assets

Zu den Verbesserungen des dynamischen Media Viewers in AEM 6.5 gehören die zusätzliche Unterstützung für das Rendern von 360 Videos, 360 Medien-Viewer (video360Social und video360VR) und die Möglichkeit, benutzerdefinierte Video-Miniaturansichten auszuwählen.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>Video geht davon aus, dass Ihre AEM-Instanz im Modus Dynamische Medien S7 ausgeführt wird.  [Anweisungen zum Einrichten von AEM mit dynamischen Medien finden Sie hier](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Beim Hochladen eines Videos verarbeitet Dynamic Media das Filmmaterial standardmäßig als 360-Grad-Video, wenn das Seitenverhältnis 2:1 beträgt. das Verhältnis von Breite zu Höhe ist 2:1.

>[!NOTE]
>
>Die Komponenten für dynamische Medien 360 unterstützen nur 360 Videos.

## Videos zu dynamischen Medien 360

360-Grad-Videos, auch als &quot;sphärischen Videos&quot;bezeichnet, sind Videoaufnahmen, bei denen eine Ansicht in jeder Richtung gleichzeitig aufgezeichnet wird, die mit einer Allrichtungskamera oder einer Kamerasammlung aufgenommen wurden. Während der Wiedergabe auf einem Flachbildschirm hat der Benutzer die Steuerung der Anzeigerichtung, und die Wiedergabe auf Mobilgeräten nutzt in der Regel die integrierte Gyroskop-Steuerung.  Damit können Sie über die Grenzen der Einzelfotografie hinausgehen. Marketingexperten können mithilfe von 360 Videos eine ansprechende Benutzererfahrung bieten.  Fangen wir an! Das Seitenverhältnis des Panoramabilds kann in der DMS7-Konfiguration der Firma geändert werden, indem die Dublette-Eigenschaft s7PanoramicAR unter /conf/global/settings/cloudconfigs/dmscene7/jcr:content angegeben wird.

## Videos zu dynamischen Medien 360

Video zu dynamischen Medien unterstützt jetzt die Auswahl einer benutzerdefinierten Miniaturansicht für Ihr Video. Ein Benutzer kann entweder ein vorhandenes Asset aus AEM Assets auswählen oder einen Videobild als Miniaturansicht auswählen.

## Dynamische 360-Medien-Viewer

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR Viewer**</td>
   </tr>
   <tr>
      <td>Dynamischer Medienlaufmodus</td>
      <td>Nur Scene7-Modus für dynamische Medien</td>
      <td>Nur Scene7-Modus für dynamische Medien<br>
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

[Dynamische Medien im Scene7-Modus konfigurieren](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
