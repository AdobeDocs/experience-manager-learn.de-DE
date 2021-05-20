---
title: AEM Forms mit JSON-Schema und -Daten[Teil 1]
seo-title: AEM Forms mit JSON-Schema und Daten[Teil1]
description: Mehrteilige Anleitung, um Sie durch die Schritte zu führen, die zum Erstellen eines adaptiven Formulars mit JSON-Schema und zum Abfragen der gesendeten Daten erforderlich sind.
seo-description: Mehrteilige Anleitung, um Sie durch die Schritte zu führen, die zum Erstellen eines adaptiven Formulars mit JSON-Schema und zum Abfragen der gesendeten Daten erforderlich sind.
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 3%

---


# Erstellen eines adaptiven Formulars basierend auf dem JSON-Schema


Die Möglichkeit, adaptive Forms auf der Grundlage eines JSON-Schemas zu erstellen, wurde mit AEM Forms 6.3 eingeführt. Die Details zum Erstellen von Adaptive Forms mit JSON-Schema werden in diesem [Artikel](https://helpx.adobe.com/de/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html) ausführlich erläutert.

Nachdem Sie ein auf einem JSON-Schema basierendes adaptives Formular erstellt haben, besteht der nächste Schritt darin, die gesendeten Daten in der Datenbank zu speichern. Zu diesem Zweck verwenden wir den neuen JSON-Datentyp, der von verschiedenen Datenbankanbietern eingeführt wurde. Für die Zwecke dieses Artikels werden wir die MySql 8 Datenbank verwenden, um die übermittelten Daten zu speichern.

MySql 8 Datenbank wurde für diesen Artikel verwendet. MySQL hat einen neuen Datentyp namens [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html) eingeführt. Dies erleichtert die Speicherung und Abfrage von JSON-Objekten. Die übermittelten Daten werden in einer Spalte vom Typ JSON in unserer Datenbank gespeichert.

Der folgende Screenshot zeigt die gesendeten Formulardaten, die im JSON-Datentyp gespeichert sind. Die Spalte &quot;formdata&quot;weist den Typ JSON auf. Wir haben auch den Namen des mit den Daten verknüpften Formulars im Spaltenformnamen gespeichert.

>[!NOTE]
>
>Stellen Sie sicher, dass Ihre JSON-Schemadatei entsprechend benannt ist. Sie muss beispielsweise im folgenden Format benannt werden: &lt;name>schema.json. Ihre Schemadatei kann daher &quot;mortgage.schema.json&quot;oder &quot;credit.schema.json&quot;lauten.


![datastored](assets/datastored.gif)


[Beispiel-JSON-Schemas, die zum Erstellen adaptiver Forms verwendet werden können.](assets/samplejsonschemas.zip). Laden Sie die ZIP-Datei herunter und entpacken Sie sie, um die JSON-Schemas abzurufen.

