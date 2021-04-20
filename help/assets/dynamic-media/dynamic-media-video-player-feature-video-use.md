---
title: Verwenden des Video-Players in AEM Dynamic Media
description: AEM Dynamic Media-Videoplayer, der früher zur Unterstützung des adaptiven Video-Streaming auf Desktop-Clients und Browsern auf Flash-Laufzeitumgebung angewiesen war, wurde beim Flash-basierten Content-Streaming aggressiver. Mit der Einführung von HLS (Apple's HTTP Live Streaming Video Versand Protocol) können Inhalte jetzt gestreamt werden, ohne auf Flash angewiesen zu sein.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 34%

---


# Verwenden des Video-Players in AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media-Videoplayer, der früher zur Unterstützung des adaptiven Video-Streaming auf Desktop-Clients und Browsern auf Flash-Laufzeitumgebung angewiesen war, wurde beim Flash-basierten Content-Streaming aggressiver. Mit der Einführung von HLS (Apple&#39;s HTTP Live Streaming Video Versand Protocol) können Inhalte jetzt gestreamt werden, ohne auf Flash angewiesen zu sein.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Schneller Einblick in den Videoplayer für Nicht-Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

Die folgende HLS-Browserunterstützung gilt für nicht unterstützte Browser als Progressiv-Video-Versand

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
   <td> <p>HLS-Video-Streaming</p> </td>
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
   <td> <p>HLS-Video-Streaming</p> </td>
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