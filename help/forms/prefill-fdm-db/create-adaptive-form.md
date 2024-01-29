---
title: Erstellen eines adaptiven Formulars
description: Erstellen und Konfigurieren eines adaptiven Formulars, um den Vorbefüllungsdienst des Formulardatenmodells zu verwenden
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
duration: 164
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# Erstellen eines adaptiven Formulars

Bisher haben wir Folgendes erstellt:

* Datenbank mit zwei Tabellen: `newhire` und `beneficiaries`
* Konfigurierte Apache Sling Connection Pooled DataSource
* RDBMS-basiertes Formulardatenmodell

Der nächste Schritt besteht darin, ein adaptives Formular zur Verwendung des Formulardatenmodells zu erstellen und zu konfigurieren. Um sofort loszulegen, können Sie ein Beispielformular [herunterladen und importieren](assets/fdm-demo-af.zip). Das Beispielformular enthält einen Abschnitt mit den Mitarbeiterdetails und einen weiteren Abschnitt, in dem die Begünstigten der Mitarbeiterin bzw. des Mitarbeiters aufgelistet werden.

## Verknüpfen des Formulars mit dem Formulardatenmodell

Das mit diesem Kurs bereitgestellte Beispielformular ist mit keinem Formulardatenmodell verknüpft. Gehen Sie wie folgt vor, um das Formular zur Verwendung des Formulardatenmodells zu konfigurieren:

* Wählen Sie das Formular „FDMDemo“ aus.
* Klicken Sie auf _Eigenschaften_ > _Formularmodell_.
* Wählen Sie in der Dropdown-Liste die Option „Formulardatenmodell“ aus.
* Suchen Sie das in der vorherigen Lektion erstellte Formulardatenmodell und wählen Sie es aus.
* Klicken Sie auf _Speichern und schließen_

## Konfigurieren des Vorbefüllungsdienstes

Der erste Schritt besteht darin, den Vorbefüllungsdienst für das Formular zu verknüpfen. Gehen Sie wie folgt vor, um den Vorbefüllungsdienst zu verknüpfen:

* Wählen Sie das Formular `FDMDemo` aus.
* Klicken Sie auf _Bearbeiten_, um das Formular im Bearbeitungsmodus zu öffnen.
* Wählen Sie den Formular-Container in der Inhaltshierarchie aus und klicken Sie auf das Schraubenschlüsselsymbol, um das zugehörige Eigenschaftenblatt zu öffnen.
* Wählen Sie in der Dropdown-Liste „Vorbefüllungsdienst“ die Option _Vorbefüllungsdienst für Formulardatenmodell_ aus.
* Klicken Sie auf das blaue ☑, um die Änderungen zu speichern.

* ![prefill-service](assets/fdm-prefill.png)

## Konfigurieren der Mitarbeiterdetails

Der nächste Schritt besteht darin, die Textfelder des adaptiven Formulars an Elemente des Formulardatenmodells zu binden. Sie müssen das Eigenschaftenblatt der folgenden Felder öffnen und „bindRef“ wie unten gezeigt festlegen.


| Feldname | Bindungsverweis |
|------------|--------------------|
| Vorname | /newhire/FirstName |
| Nachname | /newhire/lastName |

>[!NOTE]
>
>Sie können zusätzliche Textfelder hinzufügen und an entsprechende Formulardatenmodell-Elemente binden.

## Konfigurieren der Begünstigtentabelle

Der nächste Schritt besteht darin, die Begünstigten der Mitarbeiterin bzw. des Mitarbeiters tabellarisch darzustellen. Das bereitgestellte Beispielformular verfügt über eine Tabelle mit vier Spalten und einer Zeile. Wir müssen die Tabelle so konfigurieren, dass sie entsprechend der Anzahl der Begünstigten wächst.

* Öffnen Sie das Formular im Bearbeitungsmodus.
* Erweitern Sie das Stammbedienfeld und dann „Ihre Begünstigten“ > „Tabelle“.
* Wählen Sie Zeile 1 aus und klicken Sie auf das Schraubenschlüsselsymbol, um das Eigenschaftenblatt zu öffnen.
* Legen Sie **/newhire/GetEmployeeBeneficiaries** als Bindungsverweis fest.
* Legen Sie in den Wiederholungseinstellungen für „Minimale Anzahl“ den Wert „1“ und für „Maximale Anzahl“ den Wert „5“ fest.
* Ihre Konfiguration für die erste Zeile sollte wie der folgende Screenshot aussehen:
  ![row-configure](assets/configure-row.PNG)
* Klicken Sie auf das blaue ☑, um Ihre Änderungen zu speichern.

## Binden von Zeilenzellen

Schließlich müssen wir die Zeilenzellen an die Elemente des Formulardatenmodells binden.

* Erweitern Sie das Stammbedienfeld und dann „Ihre Begünstigten“ > „Tabelle“ > „Zeile1“.
* Legen Sie den Bindungsverweis für jede Zeilenzelle gemäß der folgenden Tabelle fest.

| Zeilenzelle | Bindungsverweis |
|------------|----------------------------------------------|
| Vorname | /newhire/GetEmployeeBeneficiaries/firstname |
| Nachname | /newhire/GetEmployeeBeneficiaries/lastname |
| Beziehung | /newhire/GetEmployeeBeneficiaries/relation |
| Prozentsatz | /newhire/GetEmployeeBeneficiaries/percentage |

* Klicken Sie auf das blaue ☑, um Ihre Änderungen zu speichern.

## Testen des Formulars

Jetzt müssen wir das Formular mit der entsprechenden empID in der URL öffnen. Mit den beiden folgenden Links werden Formulare mit Informationen aus der Datenbank aufgefüllt:
[Formular mit empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formular mit empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Fehlerbehebung

Mein Formular ist leer und enthält keine Daten

* Stellen Sie sicher, dass das Formulardatenmodell die richtigen Ergebnisse zurückgibt.
* Stellen Sie sicher, dass das Formular mit dem richtigen Formulardatenmodell verknüpft ist.
* Überprüfen Sie die Feldbindungen.
* Überprüfen Sie die stdout-Protokolldatei. Sie sollten sehen, dass die empID in die Datei geschrieben wurde. Wenn Sie diesen Wert nicht sehen, verwendet Ihr Formular möglicherweise nicht die benutzerdefinierte Vorlage, die bereitgestellt wurde.

Tabelle ist nicht aufgefüllt

* Überprüfen Sie die Bindung für Zeile 1.
* Stellen Sie sicher, dass die Wiederholungseinstellungen für Zeile 1 korrekt festgelegt sind (Min = 1 und Max = 5 oder höher).
