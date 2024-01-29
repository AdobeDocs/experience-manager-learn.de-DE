---
title: Ausführen der Batch-Konfiguration
description: Starten des Dokumenterstellungsprozesses durch Batch-Ausführung
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 223
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '81'
ht-degree: 100%

---

# Ausführen der Batch-Konfiguration

Stellen Sie zur Batch-Ausführung eine POST-Anfrage an die folgende API:

```xml
<baseURL>/confi/<configName>/execution
```

Diese API erwartet ein leeres JSON-Objekt als Parameter im Anfragetext.
Diese API gibt eine eindeutige URL im Antwort-Header zurück, gekennzeichnet durch den Schlüssel **location**.
Eine GET-Anfrage an diese eindeutige URL gibt Ihnen Auskunft über den Status der Batch-Ausführung.

Das folgende Video zeigt, wie die Batch-Konfiguration ausgelöst wird.

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
