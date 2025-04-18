---
title: Extrahieren einer Antwort aus benutzerdefinierter Übermittlung
description: Extrahieren einer benutzerdefinierten Antwort bei erfolgreicher Formularübermittlung
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
duration: 16
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '51'
ht-degree: 100%

---

# Extrahieren des JSON-Objekts aus der Antwort

Wenn Sie ein adaptives Headless-Formular an einen benutzerdefinierten Übermittlungs-Handler senden, können die vom Übermittlungs-Handler zurückgegebenen Daten extrahiert und in Ihrer React-App angezeigt werden. Das folgende Code-Snippet zeigt

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
