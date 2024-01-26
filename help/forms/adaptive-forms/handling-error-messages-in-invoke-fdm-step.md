---
title: Erfassen von Fehlermeldungen im Formulardatenmodelldienst als Schritt im Workflow
description: Ab AEM Forms 6.5.1 können jetzt Fehlermeldungen erfasst werden, die beim Aufrufen des Formulardatenmodelldienstes als Schritt in einem AEM-Workflow generiert wurden. Workflow.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
duration: 61
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 100%

---

# Erfassen von Fehlermeldungen im Schritt „Formulardatenmodelldienst aufrufen“

Ab AEM Forms 6.5.1 haben wir jetzt die Möglichkeit, Fehlermeldungen zu erfassen und Validierungsoptionen festzulegen. Der Schritt „Formulardatenmodelldienst aufrufen“ wurde verbessert und bietet nun die folgenden Funktionen:

* Bereitstellung einer Option für eine dreistufige Validierung („OFF“, „BASIC“ und „FULL“) zur Behandlung von Ausnahmen, die beim Aufruf des Formulardatenmodelldienstes aufgetreten sind. Die drei Optionen geben in der Reihenfolge eine jeweils strengere Version der Überprüfung der datenbankspezifischen Anforderungen an.
  ![validation-levels](assets/validation-level.PNG)

* Bereitstellung eines Kontrollkästchens zum Anpassen der Ausführung des Workflows.  Daher haben die Benutzenden jetzt die Flexibilität, mit der Workflow-Ausführung fortzufahren, selbst wenn der Schritt „Formulardatenmodelldienst aufrufen“ Ausnahmen auslöst.

* Speicherung wichtiger Informationen über Fehler, die aufgrund von Validierungsausnahmen auftreten.  Es wurden drei Autocomplete-Variablenselektoren integriert, um relevante Variablen zum Speichern von ErrorCode(String), ErrorMessage(String) und ErrorDetails(JSON) auszuwählen.  Die ErrorDetails werden jedoch auf „null“ gesetzt, falls die Ausnahme keine DermisValidationException ist.
  ![Erfassen von Fehlermeldungen](assets/fdm-error-details.PNG)

Mit diesen Änderungen stellt der Schritt „Formulardatenmodelldienst aufrufen“ sicher, dass die Eingabewerte den in der Swagger-Datei angegebenen Datenbeschränkungen entsprechen. Beispielsweise wird die folgende Fehlermeldung ausgegeben, wenn die Werte „accountId“ und „balance“ nicht den in der Swagger-Datei angegebenen Datenbeschränkungen entsprechen.

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
