---
title: Senden von Formularanlagen in einer E-Mail
description: Extrahieren und Senden gesendeter Formularanhänge per E-Mail mithilfe eines leistungsstarken automatisierten Workflows
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Extrahieren von Formularanlagen aus gesendeten Formulardaten

Extrahieren Sie Formularanhänge und senden Sie die Anlagen in einer E-Mail in einem leistungsstarken automatisierten Workflow.
Im folgenden Video werden die Schritte erläutert, die zum Erstellen von Anlagen aus den gesendeten Daten erforderlich sind.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

Im Folgenden finden Sie das Anlagenobjektschema, das Sie im Schritt Parse JSON schema verwenden müssen

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
