---
title: Konfigurieren des Formulardatenmodells
description: Formulardatenmodell basierend auf RDBMS-Datenquelle erstellen
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 4%

---



# Konfigurieren des Formulardatenmodells

## Apache Sling Connection Pooled DataSource

Der erste Schritt beim Erstellen eines RDBMS-gestützten Formulardatenmodells besteht darin, die Apache Sling Connection Pooled DataSource zu konfigurieren. Gehen Sie wie folgt vor, um die Datenquelle zu konfigurieren:

* Zeigen Sie Ihren Browser auf [configMgr](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach **Apache Sling Connection Pooled DataSource**
* Fügen Sie einen neuen Eintrag hinzu und geben Sie die Werte wie im Screenshot gezeigt an.
* ![data-source](assets/data-source.png)
* Speichern Sie Ihre Änderungen

>[!NOTE]
>Der JDBC-Verbindungs-URI, Benutzername und Kennwort ändern sich je nach MySQL-Datenbankkonfiguration.


## Erstellen eines Formulardatenmodells

* Zeigen Sie Ihren Browser auf [Datenintegrationen](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm).
* Klicken Sie auf _Erstellen_->_Formulardatenmodell_
* Geben Sie einen aussagekräftigen Namen und Titel für das Formulardatenmodell an, z. B. **Employee**
* Klicken Sie auf _Weiter_
* Wählen Sie die Datenquelle aus, die im vorherigen Abschnitt (Foren) erstellt wurde.
* Klicken Sie auf _Erstellen_->Bearbeiten , um das neu erstellte Formulardatenmodell im Bearbeitungsmodus zu öffnen.
* Erweitern Sie den Knoten _forums_ , um das Mitarbeiterschema anzuzeigen. Erweitern Sie den Mitarbeiterknoten, um die 2 Tabellen anzuzeigen.

## Entitäten zu Ihrem Modell hinzufügen

* Stellen Sie sicher, dass der Mitarbeiterknoten erweitert ist.
* Wählen Sie die neuen Entitäten und Empfänger aus und klicken Sie auf _Ausgewählte hinzufügen_.

## Lesedienst zu neuer Entität hinzufügen

* Neuen Entität auswählen
* Klicken Sie auf _Eigenschaften bearbeiten_
* Wählen Sie get aus der Dropdown-Liste Read Service .
* Klicken Sie auf das Symbol + , um Parameter zum get-Dienst hinzuzufügen.
* Geben Sie die Werte wie im Screenshot gezeigt an
* ![get-service](assets/get-service.png)
>[!NOTE]
> Der get -Dienst erwartet einen Wert, der der empID-Spalte einer neuen Entität zugeordnet ist. Es gibt mehrere Möglichkeiten, diesen Wert zu übergeben. In diesem Tutorial wird die empID über den Anforderungsparameter empID übergeben.
* Klicken Sie auf _Fertig_ , um die Argumente für den get-Dienst zu speichern.
* Klicken Sie auf _Fertig_, um Änderungen am Formulardatenmodell zu speichern.

## Verknüpfung zwischen zwei Entitäten hinzufügen

Die zwischen Datenbankentitäten definierten Verknüpfungen werden nicht automatisch im Formulardatenmodell erstellt. Die Zuordnungen zwischen Entitäten müssen mithilfe des Formulardatenmodell-Editors definiert werden. Jedes neue Unternehmen kann einen oder mehrere Begünstigte haben. Wir müssen eine Eins-zu-viele-Verbindung zwischen den neuen und den begünstigten Einrichtungen definieren.
Die folgenden Schritte führen Sie durch den Prozess der Erstellung der 1:n-Zuordnung

* Wählen Sie eine neue Entität aus und klicken Sie auf _Verknüpfung hinzufügen_
* Geben Sie einen aussagekräftigen Titel und eine Kennung für die Zuordnung und andere Eigenschaften ein, wie im Screenshot unten dargestellt
   ![Vereinigung](assets/association-entities-1.png)

* Klicken Sie auf das Symbol _edit_ unter dem Abschnitt Argumente .

* Geben Sie die Werte wie im Screenshot gezeigt an
* ![verband-2](assets/association-entities.png)
* **Wir verknüpfen die beiden Entitäten mithilfe der Spalte empID der Empfänger und neuer Entitäten.**
* Klicken Sie auf _Fertig_, um Ihre Änderungen zu speichern.

## Testen des Formulardatenmodells

Unser Formulardatenmodell verfügt jetzt über den Dienst **_get_** , der empID akzeptiert und die Details des Neuen und seiner Empfänger zurückgibt. Um den get-Dienst zu testen, führen Sie die unten aufgeführten Schritte aus.

* Neuen Entität auswählen
* Klicken Sie auf _Testmodellobjekt_
* Geben Sie eine gültige empID ein und klicken Sie auf _Test_
* Sie sollten die Ergebnisse wie im Screenshot unten dargestellt erhalten
* ![test-fdm](assets/test-form-data-model.png)
