---
title: Erfassen von Fehlermeldungen im Formulardatenmodelldienst als Schritt im Workflow
seo-title: Erfassen von Fehlermeldungen im Formulardatenmodelldienst als Schritt im Workflow
description: Ab AEM Forms 6.5.1 können jetzt Fehlermeldungen erfasst werden, die beim Aufrufen des Formulardatenmodelldienstes als Schritt in AEM Arbeitsablauf generiert wurden. Workflow.
seo-description: Ab AEM Forms 6.5.1 können jetzt Fehlermeldungen erfasst werden, die beim Aufrufen des Formulardatenmodelldienstes als Schritt in AEM Arbeitsablauf generiert wurden. Workflow.
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 1%

---


# Erfassen von Fehlermeldungen im Dienstschritt Formulardatenmodell aufrufen

Ab AEM Forms 6.5.1 haben wir jetzt die Möglichkeit, Fehlermeldungen zu erfassen und Überprüfungsoptionen festzulegen. Der Dienstschritt &quot;Formulardatenmodell aufrufen&quot;wurde verbessert und bietet nun die folgenden Funktionen.

* Bereitstellung einer Option für die 3-Stufen-Überprüfung (&quot;OFF&quot;, &quot;BASIC&quot;und &quot;FULL&quot;) zur Verarbeitung der beim Aufrufen des Formulardatenmodelldienstes aufgetretenen Ausnahmen. Die 3 Optionen geben nacheinander eine strengere Version der Prüfung datenbankspezifischer Anforderungen an.
   ![validation-levels](assets/validation-level.PNG)

* Aktivieren Sie das Kontrollkästchen zum Anpassen der Ausführung des Workflows. Daher hat der Benutzer jetzt die Flexibilität, mit der Ausführung des Workflows fortzufahren, selbst wenn der Schritt Formulardatenmodell aufrufen Ausnahmen auslöst.

* Wichtige Informationen zu Fehlern, die aufgrund von Validierungsausnahmen auftreten, werden gespeichert. Es wurden drei Variablenselektoren vom Typ Autocomplete eingefügt, um relevante Variablen zur Speicherung von ErrorCode(String), ErrorMessage(String) und ErrorDetails(JSON) auszuwählen. Die ErrorDetails werden jedoch auf null gesetzt, wenn die Ausnahme keine DermisValidationException ist.
   ![Erfassen von Fehlermeldungen](assets/fdm-error-details.PNG)

Mit diesen Änderungen stellt der Dienstschritt Formulardatenmodell aufrufen sicher, dass die Eingabewerte den in der Swagger-Datei angegebenen Datenbeschränkungen entsprechen. Beispielsweise wird die folgende Fehlermeldung ausgegeben, wenn die Werte accountId und balance nicht den in der Swagger-Datei angegebenen Datenbeschränkungen entsprechen.

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


