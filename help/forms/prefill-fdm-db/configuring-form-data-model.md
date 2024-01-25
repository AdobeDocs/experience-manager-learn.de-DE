---
title: Konfigurieren eines Formulardatenmodells
description: Erstellen eines Formulardatenmodells basierend auf einer RDBMS-Datenquelle
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
duration: 118
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 100%

---

# Konfigurieren eines Formulardatenmodells

## Apache Sling Connection Pooled DataSource

Der erste Schritt beim Erstellen eines RDBMS-gestützten Formulardatenmodells besteht darin, die Apache Sling Connection Pooled DataSource zu konfigurieren. Gehen Sie wie folgt vor, um die Datenquelle zu konfigurieren:

* Lassen Sie Ihren Browser auf [configMgr](http://localhost:4502/system/console/configMgr) verweisen.
* Suchen Sie nach **Apache Sling Connection Pooled DataSource**
* Fügen Sie einen neuen Eintrag hinzu und geben Sie die Werte wie im Screenshot gezeigt an.
* ![data-source](assets/data-source.png)
* Speichern Sie Ihre Änderungen

>[!NOTE]
>Der JDBC-Verbindungs-URI, Benutzername und Kennwort ändern sich je nach MySQL-Datenbankkonfiguration.


## Erstellen eines Formulardatenmodells

* Lassen Sie Ihren Browser auf [Datenintegrationen](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) verweisen.
* Klicken Sie auf _Erstellen_->_Formulardatenmodell_.
* Geben Sie einen aussagekräftigen Namen und Titel für das Formulardatenmodell an, z. B. **Mitarbeitende**.
* Klicken Sie auf _Weiter_.
* Wählen Sie die Datenquelle aus, die im vorherigen Abschnitt (Foren) erstellt wurde.
* Klicken Sie auf _Erstellen > Bearbeiten_, um das neu erstellte Formulardatenmodell im Bearbeitungsmodus zu öffnen
* Erweitern Sie den _Forumsknoten_, um das Mitarbeiterschema anzuzeigen. Erweitern Sie den Mitarbeiterknoten, um die zwei Tabellen anzuzeigen.

## Hinzufügen von Entitäten zu Ihrem Modell

* Stellen Sie sicher, dass der Mitarbeiterknoten erweitert ist.
* Wählen Sie die Entitäten „Neueinstellungen“ und „Begünstigte“ aus und klicken Sie auf _Auswahl hinzufügen_

## Hinzufügen eines Lesediensts zur Entität „Neueinstellungen“

* Wählen Sie die Entität „Neueinstellungen“ aus
* Klicken Sie auf _Eigenschaften bearbeiten_
* Wählen Sie „get“ aus der Dropdown-Liste des Lesedienstes aus.
* Klicken Sie auf das Symbol „+“, um Parameter zum get-Dienst hinzuzufügen.
* Geben Sie die Werte wie im Screenshot gezeigt an
* ![get-service](assets/get-service.png)
>[!NOTE]
> Der get-Dienst erwartet einen Wert, der der empID-Spalte der Entität „Neueinstellungen“ zugeordnet ist. Es gibt mehrere Möglichkeiten, diesen Wert zu übergeben. In diesem Tutorial wird die empID über den Anfrageparameter empID übergeben.
* Klicken Sie auf _Fertig_, um die Argumente für den get-Dienst zu speichern
* Klicken Sie auf _Fertig_, um Änderungen am Formulardatenmodell zu speichern

## Hinzufügen einer Verknüpfung zwischen zwei Entitäten

Die zwischen Datenbankentitäten definierten Verknüpfungen werden nicht automatisch im Formulardatenmodell erstellt. Die Zuordnungen zwischen Entitäten müssen mithilfe des Formulardatenmodelleditors definiert werden. Jede Entität „Neueinstellungen“ kann ein oder mehrere Begünstigte haben. Wir müssen eine 1:n-Verknüpfung zwischen der Entität „Neueinstellungen“ und der Entität „Begünstigte“ definieren.
Die folgenden Schritte führen Sie durch den Prozess der Erstellung der 1:n-Verknüpfung

* Wählen Sie die Entität „Neueinstellungen“ aus und klicken Sie auf _Verknüpfung hinzufügen_
* Geben Sie einen aussagekräftigen Titel und eine Kennung für die Verknüpfung und andere Eigenschaften ein, wie im Screenshot unten dargestellt
  ![association](assets/association-entities-1.png)

* Klicken Sie auf das _Bearbeiten_-Symbol unter dem Abschnitt „Argumente“

* Geben Sie die Werte wie im Screenshot gezeigt an
* ![association-2](assets/association-entities.png)
* **Wir verknüpfen die beiden Entitäten mithilfe der Spalte empID der Entitäten „Neueinstellungen“ und „Begünstigte“.**
* Klicken Sie auf _Aktualisieren_, um Ihre Änderungen zu speichern

## Testen des Formulardatenmodells

Unser Formulardatenmodell verfügt jetzt über einen **_get_**-Dienst, der die empID akzeptiert und die Details der neu eingestellten Mitarbeitenden sowie ihre Begünstigten zurückgibt. Um den get-Dienst zu testen, führen Sie die unten aufgeführten Schritte aus.

* Wählen Sie die Entität „Neueinstellungen“ aus
* Klicken Sie auf _Modellobjekt testen_
* Geben Sie eine gültige empID an und klicken Sie auf _Testen_
* Sie sollten Ergebnisse erhalten, wie sie in der folgenden Abbildung zu sehen sind
* ![test-fdm](assets/test-form-data-model.png)

## Nächste Schritte

[Abrufen der empID von der URL](./get-request-parameter.md)