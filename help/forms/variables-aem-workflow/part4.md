---
title: Variablen im AEM-Workflow [Teil 4]
description: Verwenden von Variablen des Typs „XML“, „JSON“, „ArrayList“ und „Document“ in einem AEM-Workflow
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: 269e43f7-24cf-4786-9439-f51bfe91d39c
duration: 120
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '437'
ht-degree: 100%

---

# ArrayList-Variable im AEM-Workflow

In AEM Forms 6.5 wurden Variablen des Typs „ArrayList“ eingeführt. Ein gängiger Anwendungsfall für die Verwendung der ArrayList-Variablen ist die Festlegung benutzerdefinierter Routen, die in der AssignTask-Komponente verwendet werden sollen.

Um die ArrayList-Variable in einem AEM-Workflow zu verwenden, müssen Sie ein adaptives Formular erstellen, durch das sich wiederholende Elemente in den übermittelten Daten generiert werden. Eine gängige Praxis ist dabei, ein Schema zu definieren, das ein Array-Element enthält. Für die Zwecke dieses Artikels wurde ein einfaches JSON-Schema mit Array-Elementen erstellt. Der Anwendungsfall besteht darin, dass eine Mitarbeiterin oder ein Mitarbeiter einen Ausgabenbericht ausfüllt. Im Ausgabenbericht erfassen wir, wie die oder der Vorgesetzte der Absenderin bzw. des Absenders und die Managerin oder der Manager dieser bzw. dieses Vorgesetzten heißen. Die Namen dieser Führungskräfte werden in einem Array namens „managerchain“ gespeichert. Der folgende Screenshot zeigt das Ausgabenberichtsformular und die Daten aus dem übermittelten adaptiven Formular.

![Ausgabenbericht](assets/expensereport.jpg)

Im Folgenden finden Sie die Daten aus dem übermittelten adaptiven Formular. Das adaptive Formular hat auf einem JSON-Schema basiert und die an das Schema gebundenen Daten sind im Datenelement von „afBoundData“ gespeichert. „managerchain“ ist ein Array, und wir müssen „ArrayList“ mit dem name-Element des Objekts innerhalb des managerchain-Arrays auffüllen.

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

Um die ArrayList-Variable der Untertyp-Zeichenfolge zu initialisieren, können Sie entweder die JSON Dot-Notation oder den XPath-Zuordnungsmodus verwenden. Der folgende Screenshot zeigt Ihnen, wie Sie eine ArrayList-Variable namens „CustomRoutes“ mit der JSON Dot-Notation auffüllen. Vergewissern Sie sich, dass Sie auf ein Element in einem Array-Objekt verweisen, wie im Screenshot unten dargestellt. Wir füllen „CustomRoutes ArrayList“ mit den Namen des Array-Objekts „managerchain“ auf.
„CustomRoutes ArrayList“ wird dann zum Auffüllen der Routen in der AssignTask-Komponente verwendet.
![CustomRoutes](assets/arraylist.jpg)
Sobald die Variable „CustomRoutes ArrayList“ mit den Werten aus den übermittelten Daten initialisiert wurde, werden die Routen der AssignTask-Komponente mithilfe der Variablen „CustomRoutes“ aufgefüllt. Der folgende Screenshot zeigt die benutzerdefinierten Routen in einer AssignTask-Komponente.
![AssignTask](assets/customactions.jpg)

Gehen Sie wie folgt vor, um diesen Workflow auf Ihrem System zu testen:

* Laden Sie die Datei „ArrayListVariable.zip“ herunter und speichern Sie sie in Ihrem Dateisystem.
* [Importieren Sie die ZIP-Datei](assets/arraylistvariable.zip) mit AEM Package Manager.
* [Öffnen Sie das Formular für die Reisekostenabrechnung.](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Geben Sie einige Ausgaben sowie den Namen der oder des Vorgesetzten und den Namen der Managerin oder des Managers der bzw. des Vorgesetzten ein.
* Klicken Sie auf die Schaltfläche „Absenden“.
* [Öffnen Sie Ihren Posteingang.](http://localhost:4502/aem/inbox)
* Es sollte sich dort eine neue Aufgabe zum Zuweisen der für die Spesenabrechnung zuständigen Person befinden.
* Öffnen Sie das der Aufgabe zugeordnete Formular.
* Es sollten zwei benutzerdefinierte Routen mit dem Namen der oder des Vorgesetzten und dem Namen der Managerin oder des Managers der bzw. des Vorgesetzten zu sehen sein.
  [Sehen Sie sich den Workflow „ReviewExpenseReportWorkflow“ an.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Dieser Workflow verwendet die Variable „ArrayList“, eine Variable vom Typ „JSON“ und den Regeleditor in der Komponente „ODER-Teilung“.
