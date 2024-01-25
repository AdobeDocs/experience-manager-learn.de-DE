---
title: Senden von Formularanhängen in einer E-Mail
description: Extrahieren und versenden Sie übermittelte Formularanhänge in einer E-Mail mit dem Power Automate-Workflow
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 394
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 100%

---

# Extrahieren von Formularanhängen aus übermittelten Formulardaten

Extrahieren Sie Formularanhänge und senden Sie die Anlagen in einer E-Mail in einem leistungsstarken automatisierten Workflow.
Im folgenden Video werden die Schritte erläutert, die zum Erstellen von Anlagen aus den gesendeten Daten erforderlich sind.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

Im Folgenden finden Sie das Schema des Anhangsobjekts, das Sie im Parse-Schritt des JSON-Schemas verwenden müssen

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
