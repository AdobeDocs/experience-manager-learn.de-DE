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
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 3%

---

# Dynamisches Hinzufügen von Elementen zur Auswahlgruppen-Komponente

AEM Forms 6.5 bietet die Möglichkeit, Elemente dynamisch zu einer Adaptive Forms-Auswahlgruppenkomponente hinzuzufügen, z. B. Kontrollkästchen, Optionsfelder und Bildliste.


Sie können Elemente je nach Anwendungsfall mithilfe des Visual Editor sowie des Code-Editors hinzufügen.

**Verwenden des Visual Editor:** Sie können die Elemente der Auswahlgruppe aus den Ergebnissen eines Funktionsaufrufs oder Dienstaufrufs ausfüllen. Sie können beispielsweise die Elemente der Auswahlgruppe festlegen, indem Sie die Antwort eines REST-API-Aufrufs nutzen.

Im folgenden Screenshot stellen wir die Optionen der Kreditlaufzeit(n) auf die Ergebnisse eines Dienstaufrufs namens getLoanPeriods ein.

![Regeleditor](assets/ruleeditor.png)

**Verwenden des Code-Editors**: Wenn Sie die Elemente in der Auswahlgruppe dynamisch basierend auf den im Formular eingegebenen Werten festlegen möchten. Beispielsweise setzt das folgende Codefragment die Elemente des Kontrollkästchens auf die Werte, die in die Felder &quot;applicant name&quot;und &quot;spouse&quot;des adaptiven Formulars eingegeben wurden.

Im Codeausschnitt legen wir die Elemente von WorkingMembers fest, die eine Kontrollkästchenkomponente sind. Das Array für die Elemente wird dynamisch erstellt, indem die Werte der Textfelder &quot;applicantName&quot;und &quot;spouse&quot;der adaptiven Formulare abgerufen werden.

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

So versuchen Sie es auf Ihrem System:

**Hinzufügen von Elementen mithilfe des Code-Editors**

* [Herunterladen der Assets](assets/usingthecodeeditor.zip)
* [Öffnen von Forms und Dokumenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf &quot;Erstellen&quot; | Datei-Upload&quot; und laden Sie die Datei hoch, die Sie im vorherigen Schritt heruntergeladen haben.
* [Vorschau der Formulare anzeigen](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Geben Sie den Namen des Antragstellers ein und wählen Sie den Familienstand zu Verheiratet aus.
* Ehepartner-Namen eingeben
* Klicken Sie auf Weiter
* Es sollte ein Kontrollkästchen mit dem Namen des Antragstellers und dem Namen des Ehegatten angezeigt werden, wenn der Familienstand verheiratet ist.

**Hinzufügen von Elementen mithilfe des Visual Editor**

* [Herunterladen der Assets](assets/usingthevisualeditor.zip)
* Installieren Sie Tomcat, falls noch nicht geschehen. [Anweisungen zur Installation von Tomcat finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Stellen Sie die in dieser ZIP-Datei enthaltene Datei SampleRest.war in Ihrem Tomcat bereit.](assets/sample-rest.zip)
* [Öffnen von Forms und Dokumenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf &quot;Erstellen&quot; | Datei-Upload&quot; und laden Sie die Datei hoch, die Sie im vorherigen Schritt heruntergeladen haben.
* [Vorschau der Formulare anzeigen](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Geben Sie den Darlehensbetrag und den Tab aus dem Feld ein. Dadurch wird die Regel Trigger, die das Feld Kreditzeitraum anzeigt.
* Wählen Sie den entsprechenden Kreditzeitraum aus (die Elemente für den Kreditzeitraum werden aus dem Rest-Aufruf ausgefüllt)
* Wählen Sie den Zinssatz aus und klicken Sie auf &quot;Amortisierungszeitplan abrufen&quot;.
* Die Abschreibungstabelle sollte ausgefüllt werden. Der Tilgungsplan wird mithilfe eines REST-Aufrufs abgerufen.

>[!NOTE]
> Es wird davon ausgegangen, dass Tomcat auf Port 8080 und AEM auf Port 4502 läuft
