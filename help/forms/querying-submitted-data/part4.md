---
title: AEM Forms mit JSON-Schema und Daten [Teil 4]
description: Mehrteiliges Tutorial, um Sie durch die Schritte zu führen, die zum Erstellen eines adaptiven Formulars mit JSON-Schema und zum Abfragen der gesendeten Daten erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 116
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 100%

---

# Abfrage gesendeter Daten


Der nächste Schritt besteht darin, die gesendeten Daten abzufragen und die Ergebnisse in Tabellenform anzuzeigen. Dazu verwenden wir die folgende Software:

[QueryBuilder](https://querybuilder.js.org/) – Benutzeroberflächen-Komponente zum Erstellen von Abfragen

[Datentabellen](https://datatables.net/) – zum Anzeigen der Abfrageergebnisse in Tabellenform.

Die folgende Benutzeroberfläche wurde erstellt, um die Abfrage der gesendeten Daten zu ermöglichen. Nur die im JSON-Schema als erforderlich markierten Elemente werden für die Abfrage zur Verfügung gestellt. Im untenstehenden Screenshot werden alle Übermittlungen abgefragt, bei denen die Versandpräferenz SMS ist.

Die Beispielbenutzeroberfläche zur Abfrage der gesendeten Daten verwendet nicht alle in QueryBuilder verfügbaren erweiterten Funktionen. Wir empfehlen Ihnen, sie selbst auszuprobieren.

![QueryBuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>Die aktuelle Version dieses Tutorials unterstützt nicht die Abfrage mehrerer Spalten.

Wenn Sie ein Formular zur Durchführung Ihrer Abfrage auswählen, erfolgt ein GET-Aufruf an **/bin/getdatakeysfromschema**. Dieser GET-Aufruf gibt die erforderlichen Felder zurück, die mit dem Formularschema verknüpft sind. Die erforderlichen Felder werden dann in der Dropdown-Liste von Query Builder ausgefüllt, damit Sie die Abfrage erstellen können.

Das folgende Code-Snippet ruft die Methode getRequiredColumnsFromSchema des JSONSchemaOperations-Dienstes auf. Wir übergeben die Eigenschaften und erforderlichen Elemente des Schemas an diesen Methodenaufruf. Das Array, das von diesem Funktionsaufruf zurückgegeben wird, wird dann zum Ausfüllen der Dropdown-Liste von QueryBuilder verwendet

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

Wenn auf die Schaltfläche „GetResult“ geklickt wird, erfolgt ein Get-Aufruf an **„/bin/querydata“**. Die von der QueryBuilder-Benutzeroberfläche erstellte Abfrage wird über den Abfrageparameter an das Servlet übergeben. Das Servlet wandelt diese Abfrage dann in eine SQL-Abfrage um, die zum Abfragen der Datenbank verwendet werden kann. Wenn Sie beispielsweise alle Produkte mit dem Namen „Maus“ abrufen möchten, lautet die Zeichenfolge von Query Builder `$.productname = 'Mouse'`. Diese Abfrage wird dann wie folgt konvertiert.

SELECT &#42; from  aemformswithjson . formsubmissions  where JSON_EXTRACT(  formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Maus&#39;

Das Ergebnis dieser Abfrage wird dann zurückgegeben, um die Tabelle in der Benutzeroberfläche zu füllen.

Führen Sie die folgenden Schritte aus, um dieses Beispiel auf Ihrem lokalen System auszuführen

1. [Vergewissern Sie sich, dass Sie alle hier genannten Schritte ausgeführt haben](part2.md)
1. [Importieren Sie die Datei „Dashboards v2.zip“ mit AEM Package Manager.](assets/dashboardv2.zip) Dieses Paket enthält alle erforderlichen Bundles, die Konfigurationseinstellungen, die benutzerdefinierte Übermittlung und die Beispielseite zur Abfrage von Daten.
1. Erstellen Sie ein adaptives Formular mit dem JSON-Beispielschema.
1. Konfigurieren Sie das adaptive Formular für die Übermittlung an die benutzerdefinierte Sendeaktion „customsubmithelpx“.
1. Füllen Sie das Formular aus und senden Sie es ab.
1. Verweisen Sie Ihren Browser auf [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html).
1. Wählen Sie das Formular aus und führen Sie eine einfache Abfrage durch
