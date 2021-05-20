---
title: Variablen in AEM Workflow[Teil 4]
seo-title: Variablen in AEM Workflow[Teil 4]
description: Verwenden von Variablen des Typs xml,json,arraylist,document im AEM-Workflow
seo-description: Verwenden von Variablen des Typs xml,json,arraylist,document im AEM-Workflow
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# ArrayList-Variable in AEM Workflow

Variablen des Typs ArrayList wurden in AEM Forms 6.5 eingeführt. Ein gängiges Anwendungsbeispiel für die Verwendung der Variablen ArrayList besteht darin, benutzerdefinierte Routen zu definieren, die in AssignTask verwendet werden sollen.

Um die Variable ArrayList in einem AEM Workflow zu verwenden, müssen Sie ein adaptives Formular erstellen, das sich wiederholende Elemente in den gesendeten Daten generiert. Eine gängige Praxis besteht darin, ein Schema zu definieren, das ein Array-Element enthält. Für die Zwecke dieses Artikels habe ich ein einfaches JSON-Schema erstellt, das Array-Elemente enthält. Der Anwendungsfall besteht darin, dass ein Mitarbeiter einen Ausgabenbericht ausfüllt. Im Spesenbericht erfassen wir den Vorlagennamen des Absenders und den Vorgesetzten-Namen. Die Namen des Managers werden in einem Array namens &quot;managerchain&quot;gespeichert. Der folgende Screenshot zeigt das Ausgabeberichtsformular und die Daten aus der adaptiven Forms-Übermittlung.

![Ausgabenbericht](assets/expensereport.jpg)

Im Folgenden finden Sie die Daten aus der Übermittlung des adaptiven Formulars. Das adaptive Formular basierte auf einem JSON-Schema, bei dem die an das Schema gebundenen Daten im Datenelement afBoundData gespeichert werden. Die Verwaltungskette ist ein Array und wir müssen die ArrayList mit dem name -Element des Objekts innerhalb des managerchain-Arrays füllen.

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

Um die ArrayList-Variable des Untertypstrings zu initialisieren, können Sie entweder die JSON Dot Notation oder den XPath-Zuordnungsmodus verwenden. Der folgende Screenshot zeigt Ihnen, wie Sie eine ArrayList-Variable namens CustomRoutes mit der JSON Dot Notation ausfüllen. Vergewissern Sie sich, dass Sie auf ein Element in einem Array-Objekt verweisen, wie im Screenshot unten dargestellt. Wir füllen die CustomRoutes ArrayList mit den Namen des Managerchain-Array-Objekts.
Die CustomRoutes ArrayList wird dann zum Ausfüllen der Routen in der AssignTask-Komponente verwendet
![customroutes](assets/arraylist.jpg)
Sobald die Variable CustomRoutes ArrayList mit den Werten aus den gesendeten Daten initialisiert wurde, werden die Routen der Komponente AssignTask mithilfe der Variablen CustomRoutes gefüllt. Der folgende Screenshot zeigt die benutzerdefinierten Routen in einer AssignTask
![asingtask](assets/customactions.jpg)

Gehen Sie wie folgt vor, um diesen Workflow auf Ihrem System zu testen:

* Laden Sie die Datei ArrayListVariable.zip herunter und speichern Sie sie in Ihrem Dateisystem.
* [Importieren Sie die ZIP-Datei ](assets/arraylistvariable.zip) mit dem AEM Package Manager.
* [Öffnen Sie das Formular TravelExpenseReport .](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Geben Sie einige Ausgaben und die Namen der beiden Manager ein.
* Senden-Schaltfläche aufrufen
* [Posteingang öffnen](http://localhost:4502/aem/inbox)
* Es sollte eine neue Aufgabe mit dem Titel &quot;Spesenadministrator zuweisen&quot;angezeigt werden.
* Öffnen Sie das Formular, das der Aufgabe zugeordnet ist
* Es sollten zwei benutzerdefinierte Routen mit den Namen der Manager angezeigt werden.
   [Durchsuchen Sie den Workflow ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Dieser Workflow verwendet die Variable ArrayList, die Variable JSON-Typ, den Regeleditor in der Komponente Or-Split
