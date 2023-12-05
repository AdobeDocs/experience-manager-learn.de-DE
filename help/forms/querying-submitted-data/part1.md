---
title: AEM Forms mit JSON-Schema und -Daten [Teil 1]
description: Mehrteiliges Tutorial, um Sie durch die Schritte zu führen, die zum Erstellen eines adaptiven Formulars mit JSON-Schema und zum Abfragen der gesendeten Daten erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 70
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 100%

---

# Erstellen eines adaptiven Formulars basierend auf einem JSON-Schema


Die Möglichkeit, adaptive Formulare auf der Grundlage eines JSON-Schemas zu erstellen, wurde mit AEM Forms 6.3 eingeführt. Die Details zum Erstellen adaptiver Formulare mit einem JSON-Schema werden in diesem [Artikel](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html?lang=de) ausführlich erläutert.

Nachdem Sie ein auf einem JSON-Schema basierendes adaptives Formular erstellt haben, besteht der nächste Schritt darin, die gesendeten Daten in der Datenbank zu speichern. Zu diesem Zweck verwenden wir den neuen JSON-Datentyp, der von verschiedenen Datenbankanbietern eingeführt wurde. Für die Zwecke dieses Artikels werden wir die Datenbank von MySql 8 verwenden, um die übermittelten Daten zu speichern.

Die Datenbank von MySql 8 wurde für diesen Artikel verwendet. MySQL hat einen neuen Datentyp namens [JSON](https://dev.mysql.com/doc/refman/8.0/en/security.html) eingeführt. Dies erleichtert die Speicherung und Abfrage von JSON-Objekten. Wir speichern die gesendeten Daten in einer Spalte vom Typ JSON in unserer Datenbank.

Der folgende Screenshot zeigt die gesendeten Formulardaten, die im JSON-Datentyp gespeichert sind. Die Spalte „formdata“ weist den JSON-Typ auf. Wir haben auch den Namen des Formulars, das mit den Daten verbunden ist, in der Spalte „formname“ gespeichert.

>[!NOTE]
>
>Stellen Sie sicher, dass Ihre JSON-Schemadatei entsprechend benannt ist. Sie muss beispielsweise im folgenden Format benannt werden: &lt;name>schema.json. Ihre Schemadatei kann daher „hypothek.schema.json“ oder „kredit.schema.json“ lauten.


![datastored](assets/datastored.gif)


[Beispiele für JSON-Schemata, die zum Erstellen adaptiver Formulare verwendet werden können.](assets/samplejsonschemas.zip). Laden Sie die Zip-Datei herunter und entpacken Sie sie, um die JSON-Schemata zu erhalten
