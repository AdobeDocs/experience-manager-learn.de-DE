---
title: Datenquelle konfigurieren
description: Erstellen Sie DataSource mit Verweis auf die MySQL-Datenbank
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 4%

---


# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, mit denen AEM die Integration in externe Datenbanken ermöglicht. Eine der gängigsten und gängigsten Methoden der Datenbankintegration ist die Verwendung von Apache Sling Connection Pooled DataSource-Konfigurationseigenschaften über [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechenden [MySQL-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) herunterzuladen und AEM bereitzustellen.
Legen Sie dann die DataSource-Eigenschaften für die Sling Connection Pooled für Ihre Datenbank fest. Der folgende Screenshot zeigt die für dieses Tutorial verwendeten Einstellungen. Das Datenbankschema wird Ihnen im Rahmen dieses Tutorials bereitgestellt.

![data-source](assets/data-source.JPG)


* JDBC-Treiberklasse: `com.mysql.cj.jdbc.Driver`
* JDBC Connection URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Benennen Sie Ihre Datenquelle `StoreAndRetrieveAfData`, da dies der Name ist, der im OSGi-Dienst verwendet wird.


## Datenbank erstellen


Die folgende Datenbank wurde für die Zwecke dieses Anwendungsbeispiels verwendet. Die Datenbank verfügt über eine Tabelle mit dem Namen `formdatawithattachments` mit den vier Spalten, wie im Screenshot unten dargestellt.
![data-base](assets/table-schema.JPG)

* Die Spalte **afdata** enthält die Daten des adaptiven Formulars.
* Die Spalte **attachmentsInfo** enthält die Informationen zu den Formularanlagen.
* Die Spalten **telephoneNumber** enthalten die Mobiltelefonnummer der Person, die das Formular ausfüllt.

Erstellen Sie die Datenbank durch Import des [Datenbankschemas](assets/data-base-schema.sql)
Verwendung von MySQL Workbench.

## Formulardatenmodell erstellen

Erstellen Sie ein Formulardatenmodell und basieren Sie es auf der Datenquelle, die Sie im vorherigen Schritt erstellt haben.
Konfigurieren Sie den Dienst **get** dieses Formulardatenmodells, wie im Screenshot unten dargestellt.
Stellen Sie sicher, dass Sie kein Array im Dienst **get** zurückgeben.

Mit diesem Dienst **get** wird die Telefonnummer abgerufen, die mit der Anwendungs-ID verknüpft ist.

![get-service](assets/get-service.JPG)

Dieses Formulardatenmodell wird dann in **MyAccountForm** verwendet, um die Telefonnummer abzurufen, die mit der Anwendungs-ID verknüpft ist.
