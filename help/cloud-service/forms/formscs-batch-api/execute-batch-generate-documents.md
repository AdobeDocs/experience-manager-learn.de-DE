---
title: Ausführen der Batch-Konfiguration
description: Starten des Dokumenterstellungsprozesses durch Batch-Ausführung
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '87'
ht-degree: 100%

---

# Ausführen der Batch-Konfiguration

Stellen Sie zur Batch-Ausführung eine POST-Anfrage an die folgende API:

```xml
<baseURL>/confi/<configName>/execution
```

Diese API erwartet ein leeres JSON-Objekt als Parameter im Anfragetext.
Diese API gibt eine eindeutige URL im Antwort-Header zurück, gekennzeichnet durch den Schlüssel **location**.
Eine GET-Anfrage an diese eindeutige URL gibt Ihnen Auskunft über den Status der Batch-Ausführung

Das folgende Video zeigt, wie die Batch-Konfiguration ausgelöst wird

>[!VIDEO](https://video.tv.adobe.com/v/345714?quality=12&learn=on&captions=ger)
