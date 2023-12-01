---
title: Verwenden des Video-Players in AEM Dynamic Media
description: Der AEM Dynamic Media-Videoplayer war früher auf die Flash-Laufzeitumgebung angewiesen, um das adaptive Video-Streaming auf Desktop-Clients zu unterstützen, und die Browser wurden beim Streaming von Flash-basierten Inhalten immer aggressiver. Mit der Einführung von HLS (Apple-Protokoll zur Bereitstellung von HTTP-Live-Streaming-Videos) können Inhalte jetzt gestreamt werden, ohne auf Flash angewiesen zu sein.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 100%

---


# Verwenden des Video-Players in AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

Der AEM Dynamic Media-Videoplayer war früher auf die Flash-Laufzeitumgebung angewiesen, um das adaptive Video-Streaming auf Desktop-Clients zu unterstützen, und die Browser wurden beim Streaming von Flash-basierten Inhalten immer aggressiver. Mit der Einführung von HLS (Apple-Protokoll zur Bereitstellung von HTTP-Live-Streaming-Videos) können Inhalte jetzt gestreamt werden, ohne auf Flash angewiesen zu sein.

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## Kurzer Blick auf Videoplayer, die nicht Flash verwenden {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

Die HLS-Browser-Unterstützung sieht wie folgt aus. Bei nicht unterstützten Browsern wird auf die progressive Videobereitstellung zurückgegriffen.

>[!NOTE]
>
> Dynamic Media Hybrid unterstützt seit 15. März 2022 NICHT mehr das Video-Streaming in Internet Explorer 11. Führen Sie ein Upgrade auf Version 6.5.12 oder höher durch, um zur progressiven Wiedergabe in IE 11 zurückzukehren.

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
   <td> <p>Scene7-Modus von Dynamic Media: HLS-Video-Streaming</p> 
        <p>Hybridmodus von Dynamic Media: progressiver Download</p>
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
   <td> <p>HLS-Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgeräte</p> </td>
   <td> <p>Chrome (Android 6 oder früher)</p> </td>
   <td> <p>Progressiver Download</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Chrome (Android 7 oder neuer)</p> </td>
   <td> <p>HLS-Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgeräte</p> </td>
   <td> <p>Android (Standard-Browser)</p> </td>
   <td> <p>Progressiver Download</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgerät</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>HLS-Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgeräte</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>HLS-Video-Streaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobilgeräte</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS-Video-Streaming</p> </td>
  </tr>
 </tbody>
</table>
