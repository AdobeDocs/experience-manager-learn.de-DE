---
title: Adaptives Formular erstellen
description: Erstellen und konfigurieren Sie ein adaptives Formular, um den Vorbefüllungs-Dienst des Formulardatenmodells zu verwenden
feature: Adaptive Formulare
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: Entwicklung
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# Adaptives Formular erstellen

Bisher haben wir Folgendes erstellt:

* Datenbank mit 2 Tabellen: `newhire` und `beneficiaries`
* Apache Sling Connection Pooled DataSource konfiguriert
* RDBMS-basiertes Formulardatenmodell

Der nächste Schritt besteht darin, ein adaptives Formular zur Verwendung des Formulardatenmodells zu erstellen und zu konfigurieren.  Um den Head Start zu erhalten, können Sie [Beispielformular ](assets/fdm-demo-af.zip) herunterladen und importieren. Das Beispielformular enthält einen Abschnitt mit den Mitarbeiterdetails und einen weiteren Abschnitt, in dem die Empfänger des Mitarbeiters aufgelistet werden.

## Formular mit dem Formulardatenmodell verknüpfen

Das mit diesem Kurs bereitgestellte Beispielformular ist mit keinem Formulardatenmodell verknüpft. Gehen Sie wie folgt vor, um das Formular für die Verwendung des Formulardatenmodells zu konfigurieren:

* Wählen Sie das FDMDemo-Formular aus
* Klicken Sie auf _Eigenschaften_->_Formularmodell_
* Formulardatenmodell aus der Dropdown-Liste auswählen
* Suchen und wählen Sie das in der vorherigen Lektion erstellte Formulardatenmodell aus.
* Klicken Sie auf _Speichern und schließen_

## Vorbefüllungs-Dienst konfigurieren

Der erste Schritt besteht darin, den Vorbefüllungs-Dienst für das Formular zu verknüpfen. Gehen Sie wie folgt vor, um den Vorbefüllungs-Dienst zuzuordnen

* Wählen Sie das Formular `FDMDemo` aus.
* Klicken Sie auf _Bearbeiten_ , um das Formular im Bearbeitungsmodus zu öffnen.
* Wählen Sie Formularcontainer in der Inhaltshierarchie aus und klicken Sie auf das Schraubenschlüsselsymbol, um das Eigenschaftenblatt zu öffnen.
* Wählen Sie _Vorfülldienst für Formulardatenmodell_ aus der Dropdown-Liste Vorbefüllungs-Dienst .
* Klicken Sie auf den blauen ☑, um Ihre Änderungen zu speichern.

* ![prefill-service](assets/fdm-prefill.png)

## Mitarbeiterdetails konfigurieren

Der nächste Schritt ist die Bindung der Textfelder des adaptiven Formulars an Elemente des Formulardatenmodells. Sie müssen das Eigenschaftenblatt der folgenden Felder öffnen und bindRef wie unten gezeigt einstellen


| Feldname | Bind Ref |
|------------|--------------------|
| Vorname | /newhire/FirstName |
| Nachname | /newhire/lastName |

>[!NOTE]
>
>Sie können zusätzliche Textfelder hinzufügen und an entsprechende Formulardatenmodellelemente binden.

## Tabelle &quot;Empfänger konfigurieren&quot;

Der nächste Schritt besteht darin, die Empfänger der Arbeitnehmer tabellarisch darzustellen. Das bereitgestellte Beispielformular verfügt über eine Tabelle mit 4 Spalten und einer einzelnen Zeile. Wir müssen die Tabelle so konfigurieren, dass sie entsprechend der Anzahl der Empfänger wächst.

* Öffnen Sie das Formular im Bearbeitungsmodus.
* Erweitern Sie das Stammbedienfeld ->Ihre Empfänger->Tabelle
* Wählen Sie Zeile1 aus und klicken Sie auf das Schraubenschlüsselsymbol, um das Eigenschaftenblatt zu öffnen.
* Legen Sie die Bindungsverweis auf **/newhire/GetEmployeeBeneficiaries** fest.
* Legen Sie die Wiederholungseinstellungen - Mindestanzahl auf 1 und Höchstanzahl auf 5 fest.
* Ihre Row1-Konfiguration sollte wie der folgende Screenshot aussehen
   ![row-configure](assets/configure-row.PNG)
* Klicken Sie auf den blauen ☑, um Ihre Änderungen zu speichern.

## Zellen der Bindungszeile

Schließlich müssen wir die Zellen der Zeile an die Elemente des Formulardatenmodells binden.

* Erweitern Sie das Stammbedienfeld ->Ihre Begünstigten ->Tabelle ->Zeile1
* Legen Sie die Bindungsreferenz für jede Zeilenzelle gemäß der folgenden Tabelle fest

| Zeilenzelle | Bindungsverweis |
|------------|----------------------------------------------|
| Vorname | /newhire/GetEmployeeBeneficiaries/firstname |
| Nachname | /newhire/GetEmployeeBeneficiaries/lastname |
| Beziehung | /newhire/GetEmployeeBeneficiaries/relation |
| Prozentsatz | /newhire/GetEmployeeBeneficiaries/percent |

* Klicken Sie auf den blauen ☑, um Ihre Änderungen zu speichern.

## Testen des Formulars

Jetzt müssen wir das Formular mit der entsprechenden empID in der URL öffnen. Die folgenden beiden Links enthalten Formulare mit Informationen aus der Datenbank
[Formular mit empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formular mit empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Fehlerbehebung

Mein Formular ist leer und enthält keine Daten.

* Stellen Sie sicher, dass das Formulardatenmodell die richtigen Ergebnisse zurückgibt.
* Das Formular ist mit dem richtigen Formulardatenmodell verknüpft.
* Überprüfen der Feldbindungen
* Überprüfen Sie die Stdout-Protokolldatei. Sie sollten sehen, dass empID in die Datei geschrieben wurde. Wenn Sie diesen Wert nicht sehen, verwendet Ihr Formular möglicherweise nicht die benutzerdefinierte Vorlage, die bereitgestellt wurde.

Die Tabelle ist nicht ausgefüllt

* Überprüfen der Bindung für Zeile1
* Stellen Sie sicher, dass die Wiederholungseinstellungen für Zeile 1 korrekt festgelegt sind (Min = 1 und Max = 5 oder höher).

