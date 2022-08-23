---
title: Erfassen von Fehlermeldungen im Formulardatenmodelldienst als Schritt im Workflow
description: Ab AEM Forms 6.5.1 können jetzt Fehlermeldungen erfasst werden, die beim Aufrufen des Formulardatenmodelldienstes als Schritt in AEM Workflow generiert wurden. Workflow.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# Erfassen von Fehlermeldungen im Schritt &quot;Formulardatenmodelldienst aufrufen&quot;

Ab AEM Forms 6.5.1 haben wir jetzt die Möglichkeit, Fehlermeldungen zu erfassen und Validierungsoptionen festzulegen. Der Schritt &quot;Formulardatenmodelldienst aufrufen&quot;wurde verbessert und bietet nun die folgenden Funktionen.

* Bereitstellung einer Option für die 3-Tier-Validierung (&quot;OFF&quot;, &quot;BASIC&quot;und &quot;FULL&quot;) zur Verarbeitung von Ausnahmen, die beim Aufrufen des Formulardatenmodelldienstes aufgetreten sind. Die drei Optionen geben nacheinander eine strengere Version der Datenbankspezifischen Anforderungen an.
   ![Validierungsstufen](assets/validation-level.PNG)

* Aktivieren Sie das Kontrollkästchen zum Anpassen der Ausführung des Workflows. Daher hat der Benutzer jetzt die Flexibilität, mit der Workflow-Ausführung fortzufahren, selbst wenn der Schritt Formulardatenmodell aufrufen Ausnahmen auslöst.

* Wichtige Informationen zu Fehlern, die aufgrund von Validierungsausnahmen auftreten, werden gespeichert. Drei Variablenselektoren vom Typ Autocomplete wurden hinzugefügt, um relevante Variablen zum Speichern von ErrorCode(String), ErrorMessage(String) und ErrorDetails(JSON) auszuwählen. Die ErrorDetails werden jedoch auf null gesetzt, falls die Ausnahme keine DermisValidationException ist.
   ![Erfassen von Fehlermeldungen](assets/fdm-error-details.PNG)

Mit diesen Änderungen stellt der Schritt Formulardatenmodelldienst aufrufen sicher, dass die Eingabewerte den in der Swagger-Datei angegebenen Datenbeschränkungen entsprechen. Beispielsweise wird die folgende Fehlermeldung ausgegeben, wenn die Werte accountId und balance nicht mit den in der Swagger-Datei angegebenen Datenbeschränkungen konform sind.

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```
