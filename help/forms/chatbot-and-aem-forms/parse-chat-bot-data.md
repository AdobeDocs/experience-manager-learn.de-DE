---
title: Verwenden von AEM Forms mit ChatBot
description: Analysieren von ChatBot-Daten
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 100%

---

# Analysieren von ChatBot-Daten

Ein [ChatBot-Webhook](https://www.chatbot.com/help/webhooks/what-are-webhooks/) wurde verwendet, um die ChatBot-Daten an ein AEM-Servlet zu senden.
Die im ChatBot erfassten Daten liegen im JSON-Format vor, wobei die Benutzenden wie unten gezeigt Daten im Attributobjekt eingegeben haben
![chatbot-data](assets/chat-bot-data.png)

Um die Daten mit der XDP-Vorlage zu fusionieren, müssen Sie die folgende XML-Datei erstellen. Beachten Sie das Stammelement der XML-Datei. Dies muss mit dem Stammelement der XDP-Vorlage übereinstimmen, damit die Daten erfolgreich fusioniert werden.


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![xdp-template](assets/xdp-template.png)

## Nächste Schritte

[Fusionieren von Daten mit der XDP-Vorlage](./merge-data-with-template.md)
