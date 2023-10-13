---
title: Antwort aus benutzerdefinierter Übermittlung extrahieren
description: Benutzerdefinierte Antwort bei erfolgreicher Formularübermittlung extrahieren
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# Extrahieren Sie das JSON-Objekt aus der Antwort

Wenn Sie ein Headless-adaptives Formular an einen benutzerdefinierten Submit-Handler senden, können die vom Submit-Handler zurückgegebenen Daten extrahiert und in Ihrer React-App angezeigt werden. Das folgende Codefragment zeigt

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
