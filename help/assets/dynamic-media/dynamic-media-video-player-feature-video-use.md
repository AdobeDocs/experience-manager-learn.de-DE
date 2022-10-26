---
title: Verwenden des Video-Players in AEM Dynamic Media
description: AEM Dynamic Media-Videoplayer, der bisher zur Unterstützung des adaptiven Video-Streaming auf Desktop-Clients und Browsern auf Flash-Laufzeitumgebung angewiesen war, wurde beim Flash-basierten Content-Streaming aggressiver. Mit der Einführung von HLS (Apples HTTP Live Streaming Video-Versandprotokoll) können Inhalte jetzt gestreamt werden, ohne auf Flash angewiesen zu sein.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 29%

---


# Verwenden des Video-Players in AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media-Videoplayer, der bisher zur Unterstützung des adaptiven Video-Streaming auf Desktop-Clients und Browsern auf Flash-Laufzeitumgebung angewiesen war, wurde beim Flash-basierten Content-Streaming aggressiver. Mit der Einführung von HLS (Apples HTTP Live Streaming Video-Versandprotokoll) können Inhalte jetzt gestreamt werden, ohne auf Flash angewiesen zu sein.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Schnellzugriff auf den Flash-Videoplayer {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

Die HLS-Browserunterstützung sieht wie folgt aus: Für nicht unterstützte Browser wechseln wir zurück zur progressiven Videobereitstellung

>[!NOTE]
>
> Dynamic Media Hybrid unterstützt ab 15. März 2022 NICHT mehr Video-Streaming in Internet Explorer 11. Führen Sie ein Upgrade auf Version 6.5.12 oder höher durch, um auf die progressive Wiedergabe in IE 11 zurückzukehren.

<table> 
 <thead> 
  <tr> 
   <th> <p>Gerät</p> </th>
   <th> <p>Browser</p> </th>
   <th > <p>Videowiedergabemodus</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 9 und 10</p> </td>
   <td> <p>Progressiver Download</p> </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media - Scene7-Modus: HLS-Video-Streaming</p> 
        <p>Dynamic Media - Hybridmodus: Progressiver Download</p>
   </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Progressiver Download</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 45 oder höher</p> </td>
   <td> <p>HLS Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>HLS Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>HLS Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Chrome (Android 6 oder früher)</p> </td>
   <td> <p>Progressiver Download</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Chrome (Android 7 oder neuer)</p> </td>
   <td> <p>HLS Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Android (Standardbrowser)</p> </td>
   <td> <p>Progressiver Download</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>HLS Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>HLS Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS Video-Streaming</p> </td>
  </tr>
 </tbody>
</table>
