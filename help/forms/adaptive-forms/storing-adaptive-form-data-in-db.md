---
title: Speichern von adaptiven Formulardaten
seo-title: Speichern von adaptiven Formulardaten
description: Speichern adaptiver Formulardaten in DataBase als Teil Ihres AEM-Workflows
seo-description: Speichern adaptiver Formulardaten in DataBase als Teil Ihres AEM-Workflows
feature: Adaptives Forms,Workflow,Formulardatenmodell
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# Speichern von Übermittlungen adaptiver Formulare in der Datenbank

Es gibt mehrere Möglichkeiten, die gesendeten Formulardaten in der Datenbank Ihrer Wahl zu speichern. Eine JDBC-Datenquelle kann verwendet werden, um die Daten direkt in der Datenbank zu speichern. Ein benutzerdefiniertes OSGI-Bundle kann geschrieben werden, um die Daten in der Datenbank zu speichern. In diesem Artikel wird der benutzerdefinierte Prozessschritt in AEM Workflow zum Speichern der Daten verwendet.
Der Anwendungsfall besteht darin, einen AEM Workflow für die Übermittlung eines adaptiven Formulars Trigger und einen Schritt im Workflow zu speichern, um die übermittelten Daten in der Datenbank zu speichern.

**Führen Sie die unten beschriebenen Schritte aus, um diese Funktion auf Ihrem System verwenden zu können.**

* [Laden Sie die ZIP-Datei herunter und extrahieren Sie den Inhalt auf Ihre Festplatte](assets/storeafdataindb.zip)

   * Importieren Sie die Datei StoreAFInDBWorkflow.zip mithilfe des Package Managers in AEM. Das Paket enthält einen Beispiel-Workflow, der die AF-Daten in DB speichert. Öffnen Sie das Workflow-Modell. Der Workflow umfasst nur einen Schritt. Dieser Schritt ruft den im Bundle geschriebenen Code auf, um die AF-Daten in der Datenbank zu speichern. Ich übergebe ein einziges Argument an den Prozess. Dies ist der Name des adaptiven Formulars, dessen Daten gespeichert werden.
   * Stellen Sie die Datei &quot;insertdata.core-0.0.1-SNAPSHOT.jar&quot;mithilfe der Felix-Webkonsole bereit. Dieses Bundle enthält den Code zum Schreiben der gesendeten Formulardaten in die Datenbank

* Gehen Sie zu [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Suchen Sie nach &quot;JDBC Connection Pool&quot;. Erstellen Sie einen neuen Day Commons JDBC Connection Pool. Geben Sie die spezifischen Einstellungen für Ihre Datenbank an.

   * ![JDBC-Verbindungspool](assets/jdbc-connection-pool.png)
   * Suchen Sie nach &quot;**Formulardaten in DB** einfügen&quot;.
   * Geben Sie die spezifischen Eigenschaften für Ihre Datenbank an.
      * DataSourceName:Name der Datenquelle, die Sie zuvor konfiguriert haben.
      * TableName - Name der Tabelle, in der Sie die AF-Daten speichern möchten
      * FormName - Spaltenname für den Namen des Formulars
      * ColumnName - Spaltenname für die AF-Daten

   ![insertdata](assets/insertdata.PNG)

* Erstellen Sie ein adaptives Formular.

* Verknüpfen Sie das adaptive Formular mit AEM Workflow(StoreAFValuesinDB), wie im Screenshot unten dargestellt.

* Stellen Sie sicher, dass Sie &quot;data.xml&quot;im Datendateipfad angeben, wie im Screenshot unten dargestellt

   ![Sendung](assets/submissionafforms.png)

* Vorschau des Formulars anzeigen und senden

* Wenn alles gut funktioniert hat, sollten die Formulardaten in der von Ihnen angegebenen Tabelle und Spalte gespeichert sein



