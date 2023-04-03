---
title: Ausführen der Batch-Konfiguration
description: Starten Sie den Dokumenterstellungsprozess, indem Sie den Batch ausführen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Ausführen der Batch-Konfiguration

Um den Batch auszuführen, stellen Sie eine POST-Anfrage an die folgende API

```xml
<baseURL>/confi/<configName>/execution
```

Diese API erwartet ein leeres JSON-Objekt als Parameter im Anfragetext.
Diese API gibt eine eindeutige URL im Antwortheader zurück, die von **location** Schlüssel.
Eine GET-Anfrage an diese eindeutige URL zeigt Ihnen den Status der Batch-Ausführung an

Das folgende Video zeigt das Auslösen der Batch-Konfiguration

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
