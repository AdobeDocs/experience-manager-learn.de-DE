---
title: Speichern von adaptiven Formulardaten
seo-title: Speichern von adaptiven Formulardaten
description: Speichern adaptiver Formulardaten in DataBase als Teil Ihres AEM-Workflows
seo-description: Speichern adaptiver Formulardaten in DataBase als Teil Ihres AEM-Workflows
feature: adaptive-forms,workflow
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 1%

---


# Speichern von Übermittlungen adaptiver Formulare in der Datenbank

Es gibt mehrere Möglichkeiten, die gesendeten Formulardaten in der Datenbank Ihrer Wahl zu speichern. Eine JDBC-Datenquelle kann verwendet werden, um die Daten direkt in der Datenbank zu speichern. Ein benutzerdefiniertes OSGI-Bundle kann geschrieben werden, um die Daten in der Datenbank zu speichern. In diesem Artikel werden die Daten mithilfe eines benutzerdefinierten Prozessschritts im Arbeitsablauf gespeichert, der AEM Arbeitsablauf.
Der Anwendungsfall besteht darin, einen AEM Workflow bei einer Übermittlung des adaptiven Formulars auszulösen und die gesendeten Daten in der Datenbank in einem Schritt im Workflow zu speichern.

**Gehen Sie wie folgt vor, um dieses Programm auf Ihrem System zu verwenden**

* [Laden Sie die ZIP-Datei herunter und extrahieren Sie den Inhalt auf Ihre Festplatte](assets/storeafdataindb.zip)

   * Importieren Sie die Datei &quot;StoreAFInDBWorkflow.zip&quot;mit dem Package Manager in AEM. Das Paket verfügt über einen Beispielarbeitsablauf, in dem die AF-Daten in DB gespeichert werden. Öffnen Sie das Workflow-Modell. Der Workflow umfasst nur einen Schritt. Dieser Schritt ruft den im Bundle geschriebenen Code auf, um die AF-Daten in der Datenbank zu speichern. Ich übergebe ein einziges Argument an den Prozess. Dies ist der Name des adaptiven Formulars, dessen Daten gespeichert werden.
   * Stellen Sie mithilfe der Felix-Webkonsole die Datei &quot;insertData.core-0.0.1-SNAPSHOT.jar&quot;bereit. Dieses Bundle enthält den Code zum Schreiben der gesendeten Formulardaten in die Datenbank

* Zu [ConfigMgr wechseln](http://localhost:4502/system/console/configMgr)

   * Suchen Sie nach &quot;JDBC Connection Pool&quot;. Erstellen Sie einen neuen Day Commons JDBC Connection Pool. Geben Sie die Einstellungen für Ihre Datenbank an.

   * ![JDBC-Verbindungspool](assets/jdbc-connection-pool.png)
   * Suchen Sie nach &quot;Formulardaten **in DB** einfügen&quot;.
   * Geben Sie die für Ihre Datenbank spezifischen Eigenschaften an.
      * DataSourceName:Name der Datenquelle, die Sie zuvor konfiguriert haben.
      * TableName - Name der Tabelle, in der Sie die AF-Daten speichern möchten
      * FormName - Spaltenname für den Namen des Formulars
      * ColumnName - Spaltenname für die AF-Daten

   ![insertData](assets/insertdata.PNG)

* Erstellen Sie ein adaptives Formular.

* Verknüpfen Sie das adaptive Formular mit AEM Workflow(StoreAFValuesinDB), wie im Screenshot unten dargestellt.

* Vergewissern Sie sich, dass Sie &quot;data.xml&quot;im Datendateipfad angeben, wie im Screenshot unten gezeigt.

   ![submit](assets/submissionafforms.png)

* Vorschau des Formulars und Senden

* Wenn alles gut funktioniert hat, sollten Sie sehen, wie die Formulardaten in der von Ihnen angegebenen Tabelle und Spalte gespeichert werden



