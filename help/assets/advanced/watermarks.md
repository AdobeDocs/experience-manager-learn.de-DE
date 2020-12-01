---
title: Wasserzeichen in AEM Assets
description: AEM als Wasserzeichenfunktion eines Cloud Service können benutzerdefinierte Bilddarstellungen mit einem beliebigen PNG-Bild mit einem Wasserzeichen versehen werden.
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# Wasserzeichen

AEM als Wasserzeichenfunktion eines Cloud Service können benutzerdefinierte Bilddarstellungen mit einem beliebigen PNG-Bild mit einem Wasserzeichen versehen werden.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi-Konfiguration

Der folgende OSGi-Konfigurationsabschnitt kann aktualisiert und dem Unterprojekt `ui.config` Ihres AEM hinzugefügt werden.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
