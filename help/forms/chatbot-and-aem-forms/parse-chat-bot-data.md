---
title: Verwenden von AEM Forms mit Chatbot
description: Analysieren von ChatBot-Daten
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# Analysieren von ChatBot-Daten

A [ChatBot-Webhook](https://www.chatbot.com/help/webhooks/what-are-webhooks/) wurde verwendet, um die ChatBot-Daten an ein AEM Servlet zu senden.
Die im ChatBot erfassten Daten liegen im JSON-Format vor, wobei der Benutzer wie unten gezeigt Daten im Attributobjekt eingegeben hat
![chatbot-data](assets/chat-bot-data.png)

Um die Daten mit der XDP-Vorlage zusammenzuführen, müssen wir die folgende XML erstellen. Beachten Sie das Stammelement der XML-Datei. Dies muss mit dem Stammelement der XDP-Vorlage übereinstimmen, damit die Daten erfolgreich zusammengeführt werden.


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

[Zusammenführen von Daten mit XDP-Vorlage](./merge-data-with-template.md)



