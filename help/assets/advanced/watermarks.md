---
title: Wasserzeichen in AEM Assets
description: AEM als Wasserzeichenfunktion eines Cloud Service ermöglicht es, benutzerdefinierte Bildausgabeformate mit einem beliebigen PNG-Bild mit Wasserzeichen zu versehen.
feature: asset compute Microservices
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 3%

---


# Wasserzeichen

AEM als Wasserzeichenfunktion eines Cloud Service ermöglicht es, benutzerdefinierte Bildausgabeformate mit einem beliebigen PNG-Bild mit Wasserzeichen zu versehen.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi-Konfiguration

Die folgende OSGi-Konfigurationsstudie kann aktualisiert und zum Unterprojekt `ui.config` Ihres AEM-Projekts hinzugefügt werden.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
