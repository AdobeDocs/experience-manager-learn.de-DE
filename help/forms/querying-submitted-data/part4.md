---
title: AEM Forms mit JSON-Schema und -Daten[Teil 4]
seo-title: AEM Forms with JSON Schema and Data[Part4]
description: Mehrteilige Anleitung, um Sie durch die Schritte zu führen, die zum Erstellen eines adaptiven Formulars mit JSON-Schema und zum Abfragen der gesendeten Daten erforderlich sind.
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Abfrage gesendeter Daten


Der nächste Schritt besteht darin, die gesendeten Daten abzufragen und die Ergebnisse tabellarisch anzuzeigen. Dazu werden wir die folgende Software verwenden

[QueryBuilder](https://querybuilder.js.org/) - UI-Komponente zum Erstellen von Abfragen

[Datentabellen](https://datatables.net/)- Zum Anzeigen der Abfrageergebnisse in Tabellenform.

Die folgende Benutzeroberfläche wurde erstellt, um die Abfrage der gesendeten Daten zu ermöglichen. Nur die im JSON-Schema als erforderlich markierten Elemente werden für die Abfrage zur Verfügung gestellt. Im folgenden Screenshot werden alle Übermittlungen abgefragt, bei denen der Versandpref SMS ist.

Die Beispielbenutzeroberfläche zur Abfrage der gesendeten Daten verwendet nicht alle in QueryBuilder verfügbaren erweiterten Funktionen. Es wird empfohlen, sie selbst auszuprobieren.

![QueryBuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>Die aktuelle Version dieses Tutorials unterstützt nicht die Abfrage mehrerer Spalten.

Wenn Sie ein Formular zur Durchführung Ihrer Abfrage auswählen, wird ein GET-Aufruf an **/bin/getdatakeysfromschema**. Dieser GET-Aufruf gibt die erforderlichen Felder zurück, die mit dem Formularschema verknüpft sind. Die erforderlichen Felder werden dann in der Dropdown-Liste von QueryBuilder ausgefüllt, damit Sie die Abfrage erstellen können.

Das folgende Codefragment führt einen Aufruf der Methode getRequiredColumnsFromSchema des JSONSchemaOperations-Dienstes durch. Wir übergeben die Eigenschaften und erforderlichen Elemente des Schemas an diesen Methodenaufruf. Das Array, das von diesem Funktionsaufruf zurückgegeben wird, wird dann zum Ausfüllen der Dropdown-Liste &quot;Query Builder&quot;verwendet

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

Wenn auf die Schaltfläche &quot;GetResult&quot;geklickt wird, wird Get aufgerufen an **&quot;/bin/querydata&quot;**. Die von der QueryBuilder-Benutzeroberfläche erstellte Abfrage wird über den Abfrageparameter an das Servlet übergeben. Das Servlet massiert diese Abfrage in eine SQL-Abfrage, die zum Abfragen der Datenbank verwendet werden kann. Wenn Sie beispielsweise alle Produkte mit dem Namen &quot;Maus&quot;abrufen möchten, lautet die Abfragezeichenfolge von Query Builder $.productName = &#39;Maus&#39;. Diese Abfrage wird dann in die folgende

SELECT &#42; von aemformswithjson .  formsubmissions, wobei JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Maus&#39;

Das Ergebnis dieser Abfrage wird dann zurückgegeben, um die Tabelle in der Benutzeroberfläche zu füllen.

Führen Sie die folgenden Schritte aus, um dieses Beispiel auf Ihrem lokalen System auszuführen

1. [Vergewissern Sie sich, dass Sie alle hier genannten Schritte ausgeführt haben](part2.md)
1. [Importieren Sie die Datei &quot;Dashboards v2.zip&quot;mit AEM Package Manager.](assets/dashboardv2.zip) Dieses Paket enthält alle erforderlichen Bundles, Konfigurationseinstellungen, benutzerdefinierte Übermittlung und Beispielseite zur Abfrage von Daten.
1. Erstellen eines adaptiven Formulars mit dem JSON-Beispielschema
1. Konfigurieren des adaptiven Formulars für die Übermittlung an die benutzerdefinierte Sendeaktion &quot;customsubmithelpx&quot;
1. Formular ausfüllen und senden
1. Zeigen Sie Ihren Browser auf [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Wählen Sie das Formular aus und führen Sie eine einfache Abfrage durch
