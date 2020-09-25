---
title: AEM Forms mit JSON-Schema und -Daten[Teil 1]
seo-title: AEM Forms mit JSON-Schema und -Daten[Teil1]
description: Mehrteilige Übung, um Sie durch die Schritte zu führen, die beim Erstellen eines adaptiven Formulars mit JSON-Schema und beim Abfragen der gesendeten Daten erforderlich sind.
seo-description: Mehrteilige Übung, um Sie durch die Schritte zu führen, die beim Erstellen eines adaptiven Formulars mit JSON-Schema und beim Abfragen der gesendeten Daten erforderlich sind.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 2%

---


# Adaptives Formular basierend auf JSON-Schema erstellen


Die Möglichkeit, adaptive Forms auf Basis von JSON-Schema zu erstellen, wurde mit AEM Forms 6.3 eingeführt. Die Einzelheiten zum Erstellen von Adaptiven Forms mit JSON-Schema werden in diesem [Artikel](https://helpx.adobe.com/de/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)ausführlich erläutert.

Nachdem Sie ein adaptives Formular auf der Grundlage des JSON-Schemas erstellt haben, müssen Sie die gesendeten Daten als Nächstes in einer Datenbank speichern. Zu diesem Zweck verwenden wir den neuen JSON-Datentyp, der von verschiedenen Datenbankherstellern eingeführt wurde. Für die Zwecke dieses Artikels verwenden wir die MySql 8 Datenbank, um die gesendeten Daten zu speichern.

MySql 8 Datenbank wurde für diesen Artikel verwendet. MySQL hat einen neuen Datentyp namens [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)eingeführt. Dies erleichtert die Speicherung und Abfrage von JSON-Objekten. Wir werden die gesendeten Daten in einer Spalte des Typs JSON in unserer Datenbank speichern.

Der folgende Screenshot zeigt die gesendeten Formulardaten, die im JSON-Datentyp gespeichert sind. Die Spalte &quot;formdata&quot;ist vom Typ JSON. Wir haben auch den Namen des Formulars gespeichert, das mit den Daten im Spaltenformname verknüpft ist

>[!NOTE]
>
>Vergewissern Sie sich, dass Ihre JSON-Schema-Datei entsprechend benannt ist. Sie muss beispielsweise im folgenden Format benannt werden: Ihre Schema-Datei kann also mortgage.Schema.json oder credit.Schema.json lauten.


![datastored](assets/datastored.gif)


[Beispiel-JSON-Schema, die zum Erstellen eines adaptiven Forms verwendet werden können.](assets/samplejsonschemas.zip). Laden Sie die ZIP-Datei herunter und dekomprimieren Sie sie, um die JSON-Schema abzurufen.

