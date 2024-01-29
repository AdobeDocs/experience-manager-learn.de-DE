---
title: Hinzufügen von Elementen zur Auswahlgruppen-Komponente
description: Dynamisches Hinzufügen von Elementen zur Auswahlgruppen-Komponente
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 362
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '456'
ht-degree: 100%

---

# Dynamisches Hinzufügen von Elementen zur Auswahlgruppen-Komponente

AEM Forms 6.5 bietet die Möglichkeit, Elemente dynamisch zu einer Auswahlgruppen-Komponente adaptiver Formulare (z. B. Kontrollkästchen, Optionsfeld und Bildliste) hinzuzufügen.


Sie können Elemente je nach Anwendungsfall mithilfe des visuellen Editors und des Code-Editors hinzufügen.

**Verwenden des visuellen Editors:** Sie können die Elemente der Auswahlgruppe anhand der Ergebnisse eines Funktions- oder Dienstaufrufs auffüllen lassen. Sie können beispielsweise die Elemente der Auswahlgruppe festlegen, indem Sie die Antwort eines REST-API-Aufrufs nutzen.

Im folgenden Screenshot stellen wir die Optionen für die Kreditlaufzeit in Jahren für die Ergebnisse eines Dienstaufrufs namens „getLoanPeriods“ ein.

![Regeleditor](assets/ruleeditor.png)

**Verwenden des Code-Editors**: Wenn Sie die Elemente in der Auswahlgruppe dynamisch basierend auf den im Formular eingegebenen Werten festlegen möchten. Beispielsweise setzt das folgende Codesnippet die Elemente des Kontrollkästchens auf die Werte, die im adaptiven Formular in die Felder für den Namen der Antragsstellerin bzw. des Antragsstellers und für die Ehegattin bzw. den Ehegatten eingegeben wurden.

Im Codesnippet legen wir die Elemente von WorkingMembers fest. Dabei handelt es sich um eine Kontrollkästchen-Komponente. Das Array für die Elemente wird dynamisch erstellt, indem die Werte der Textfelder für den Namen der Antragsstellerin bzw. des Antragsstellers und für die Ehegattin bzw. den Ehegatten in den adaptiven Formularen abgerufen werden.

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

Die übermittelten Daten lauten wie folgt:

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**Hinzufügen von Elementen mit dem Regeleditor**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Hinzufügen von Elementen mit dem Code-Editor**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Gehen Sie wie folgt vor, um dies auf Ihrem System auszuprobieren:

**Hinzufügen von Elementen mithilfe des Code-Editors**

* [Laden Sie die Assets herunter.](assets/usingthecodeeditor.zip)
* [Öffnen Sie „Formulare und Dokumente“.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf „Erstellen“ > „Datei hochladen“ und laden Sie die im vorherigen Schritt heruntergeladene Datei hoch.
* [Zeigen Sie die Formulare in der Vorschau an.](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Geben Sie den Namen der Antragsstellerin bzw. des Antragstellers ein und wählen Sie „Verheiratet“ als Familienstand aus.
* Geben Sie den Namen der Ehegattin bzw. des Ehegatten ein.
* Klicken Sie auf „Weiter“
* Es sollte ein Kontrollkästchen mit dem Namen der Antragsstellerin bzw. des Antragstellers und der Ehegattin bzw. des Ehegatten angezeigt werden, wenn als Familienstand „Verheiratet“ angegeben ist.

**Hinzufügen von Elementen mithilfe des visuellen Editors**

* [Laden Sie die Assets herunter.](assets/usingthevisualeditor.zip)
* Installieren Sie Tomcat, falls noch nicht geschehen. [Anweisungen zur Installation von Tomcat finden Sie hier.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=de)
* [Stellen Sie die in der ZIP-Datei enthaltene Datei „SampleRest.war“ in Ihrer Tomcat-Installation bereit.](assets/sample-rest.zip)
* [Öffnen Sie „Formulare und Dokumente“.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf „Erstellen“ > „Datei hochladen“ und laden Sie die im vorherigen Schritt heruntergeladene Datei hoch.
* [Zeigen Sie die Formulare in der Vorschau an.](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Geben Sie den Kreditbetrag ein und verlassen Sie das Feld durch Drücken der Tabulatortaste. Auf diese Weise wird die Regel ausgelöst, durch die das Feld für den Kreditzeitraum angezeigt wird.
* Wählen Sie den entsprechenden Kreditzeitraum aus (die Elemente für den Kreditzeitraum werden über den Rest-Aufruf aufgefüllt).
* Wählen Sie den Zinssatz aus und klicken Sie auf die Option zum Abrufen des Tilgungsplans.
* Die Tilgungstabelle sollte ausgefüllt worden sein. Der Tilgungsplan wird mithilfe eines REST-Aufrufs abgerufen.

>[!NOTE]
> Es wird davon ausgegangen, dass Tomcat auf Port 8080 und AEM auf Port 4502 ausgeführt wird.
