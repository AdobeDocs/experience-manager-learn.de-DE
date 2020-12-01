---
title: Datenquelle konfigurieren
description: DataSource-Verweis auf die MySQL-Datenbank erstellen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 3%

---


# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, AEM die Integration mit externen Datenbanken zu ermöglichen. Eine der gebräuchlichsten und gängigsten Methoden der Datenbankintegration ist die Verwendung der Konfigurationseigenschaften von Apache Sling Connection Pooled DataSource über [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechenden [MySQL-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) herunterzuladen und für AEM bereitzustellen.
Legen Sie dann die für Ihre Datenbank spezifischen Eigenschaften von Sling Connection Pooled DataSource fest. Der folgende Screenshot zeigt die Einstellungen, die für dieses Tutorial verwendet werden. Das Datenbank-Schema wird Ihnen im Rahmen dieses Lernprogramms bereitgestellt.

![data-source](assets/data-source.JPG)


* JDBC-Treiberklasse: `com.mysql.cj.jdbc.Driver`
* JDBC Connection URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Achten Sie darauf, dass Sie Ihrer Datenquelle `StoreAndRetrieveAfData` einen Namen geben, da dies der Name ist, der im OSGi-Dienst verwendet wird.


## Datenbank erstellen


Für diesen Verwendungsfall wurde die folgende Datenbank verwendet. Die Datenbank hat eine Tabelle mit dem Namen `formdatawithattachments` mit den 4 Spalten, wie im Screenshot unten dargestellt.
![data-base](assets/table-schema.JPG)

* Die Spalte **afdata** enthält die Daten des adaptiven Formulars.
* Die Spalte **attachmentsInfo** enthält die Informationen zu den Formularanlagen.
* Die Spalten **telephoneNumber** enthalten die Mobiltelefonnummer der Person, die das Formular ausfüllt.

Erstellen Sie die Datenbank, indem Sie das [Datenbankmodul](assets/data-base-schema.sql) importieren.
mit MySQL Workbench.

## Formulardatenmodell erstellen

Erstellen Sie ein Formulardatenmodell und basieren Sie es auf der Datenquelle, die Sie im vorherigen Schritt erstellt haben.
Konfigurieren Sie den **get**-Dienst dieses Formulardatenmodells, wie im Screenshot unten dargestellt.
Stellen Sie sicher, dass Sie kein Array im **get**-Dienst zurückgeben.

Mit diesem Dienst **get** wird die Telefonnummer abgerufen, die der Anwendungs-ID zugeordnet ist.

![get-service](assets/get-service.JPG)

Dieses Formulardatenmodell wird dann in **MyAccountForm** verwendet, um die mit der Anwendungs-ID verknüpfte Telefonnummer abzurufen.
