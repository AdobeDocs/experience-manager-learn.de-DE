---
title: Wasserzeichen in AEM Assets
description: AEM als Wasserzeichenfunktion eines Cloud Service können benutzerdefinierte Bilddarstellungen mit einem beliebigen PNG-Bild mit einem Wasserzeichen versehen werden.
feature: Asset Compute Microservices
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

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
