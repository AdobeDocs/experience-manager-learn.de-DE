---
title: Wasserzeichen in AEM Assets
description: AEM als Wasserzeichenfunktion eines Cloud Service ermöglicht es, benutzerdefinierte Bildausgabeformate mit einem beliebigen PNG-Bild mit Wasserzeichen zu versehen.
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

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
