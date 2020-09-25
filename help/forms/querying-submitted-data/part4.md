---
title: AEM Forms mit JSON-Schema und -Daten[Teil 4]
seo-title: AEM Forms mit JSON-Schema und -Daten[Teil 4]
description: Mehrteilige Übung, um Sie durch die Schritte zu führen, die beim Erstellen eines adaptiven Formulars mit JSON-Schema und beim Abfragen der gesendeten Daten erforderlich sind.
seo-description: Mehrteilige Übung, um Sie durch die Schritte zu führen, die beim Erstellen eines adaptiven Formulars mit JSON-Schema und beim Abfragen der gesendeten Daten erforderlich sind.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# Abfragen gesendeter Daten


Der nächste Schritt besteht in der Abfrage der gesendeten Daten und der Tabellenanzeige. Dazu verwenden wir die folgende Software

[QueryBuilder](https://querybuilder.js.org/) - UI-Komponente zum Erstellen von Abfragen

[Datentabellen](https://datatables.net/): Die Anzeige der Abfrage erfolgt tabellarisch.

Die folgende Benutzeroberfläche wurde erstellt, um die Abfrage der gesendeten Daten zu ermöglichen. Nur die im JSON-Schema als erforderlich markierten Elemente stehen der Abfrage zur Verfügung. Im folgenden Screenshot fragen wir nach allen Übermittlungen, bei denen die Auslieferung SMS ist.

Die Beispielbenutzeroberfläche zur Abfrage der gesendeten Daten verwendet nicht alle in QueryBuilder verfügbaren erweiterten Funktionen. Es wird empfohlen, sie selbst auszuprobieren.

![QueryBuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>Die aktuelle Version dieses Lernprogramms unterstützt nicht die Abfrage mehrerer Spalten.

Wenn Sie ein Formular auswählen, um Ihre Abfrage auszuführen, wird ein GET an **/bin/getdatakeysformSchema** gesendet. Dieser GET-Aufruf gibt die erforderlichen Felder zurück, die mit dem Schema des Formulars verknüpft sind. Die erforderlichen Felder werden dann in der Dropdown-Liste von QueryBuilder ausgefüllt, damit Sie die Abfrage erstellen können.

Im folgenden Codefragment wird die Methode getRequiredColumnsFromSchema des JSONSchemaOperations-Dienstes aufgerufen. Wir übergeben die Eigenschaften und erforderlichen Elemente des Schemas an diesen Methodenaufruf. Das Array, das von diesem Funktionsaufruf zurückgegeben wird, wird dann verwendet, um die Dropdown-Liste Abfrage Builder zu füllen

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

Wenn auf die Schaltfläche GetResult geklickt wird, wird ein Get-Aufruf an **&quot;/bin/querydata&quot;** gesendet. Die von der Benutzeroberfläche von QueryBuilder erstellte Abfrage wird über den Parameter &quot;Abfrage&quot;an das Servlet übergeben. Das Servlet massiert dann diese Abfrage in eine SQL-Abfrage, die zur Abfrage der Datenbank verwendet werden kann. Wenn Sie z. B. alle Produkte mit dem Namen &quot;Maus&quot;abrufen möchten, lautet die Abfrage der Abfrage Builder $.productName = &#39;Maus&#39;. Diese Abfrage wird dann in die folgende

WÄHLEN SIE * aus aemformswithjson aus.  formsubmissions where JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Maus&#39;

Das Ergebnis dieser Abfrage wird dann zurückgegeben, um die Tabelle in der Benutzeroberfläche zu füllen.

So führen Sie dieses Beispiel auf Ihrem lokalen System aus

1. [Vergewissern Sie sich, dass Sie alle hier genannten Schritte ausgeführt haben](part2.md)
1. [Importieren Sie die Datei &quot;Dashboard2.zip&quot;mit AEM Package Manager.](assets/dashboardv2.zip) Dieses Paket enthält alle erforderlichen Bundles, Konfigurationseinstellungen, benutzerdefinierte Senden- und Beispieldaten zur Abfrage.
1. Erstellen eines adaptiven Formulars mit dem JSON-Schema
1. Konfigurieren des adaptiven Formulars für die Übermittlung an benutzerdefinierte Übermittlungsaktion &quot;customsubmithelpx&quot;
1. Ausfüllen des Formulars und Senden
1. Verweisen Sie Ihren Browser auf [Dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Wählen Sie das Formular aus und führen Sie eine einfache Abfrage durch

