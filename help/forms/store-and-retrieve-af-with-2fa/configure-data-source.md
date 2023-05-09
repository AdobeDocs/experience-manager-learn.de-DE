---
title: Konfigurieren einer Datenquelle
description: Erstellen Sie DataSource mit Verweis auf die MySQL-Datenbank
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 5%

---

# Konfigurieren einer Datenquelle

Es gibt viele Möglichkeiten, AEM die Integration in eine externe Datenbank zu ermöglichen. Eine der gängigsten und gängigsten Methoden der Datenbankintegration ist die Verwendung der Konfigurationseigenschaften von Apache Sling Connection Pooled DataSource über die [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechende [MySQL-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEM.
Legen Sie dann die DataSource-Eigenschaften der Sling Connection Pooled für Ihre Datenbank fest. Der folgende Screenshot zeigt die für dieses Tutorial verwendeten Einstellungen. Das Datenbankschema wird Ihnen im Rahmen dieses Tutorials bereitgestellt.

![data-source](assets/data-source.JPG)


* JDBC-Treiberklasse: `com.mysql.cj.jdbc.Driver`
* JDBC Connection URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Benennen Sie Ihre Datenquelle. `StoreAndRetrieveAfData` da dies der Name ist, der im OSGi-Dienst verwendet wird.


## Datenbank erstellen


Die folgende Datenbank wurde für die Zwecke dieses Anwendungsbeispiels verwendet. Die Datenbank verfügt über eine Tabelle mit dem Namen `formdatawithattachments` mit den 4 Spalten, wie im Screenshot unten dargestellt.
![data-base](assets/table-schema.JPG)

* Die Spalte **afdata** enthält die Daten des adaptiven Formulars.
* Die Spalte **attachmentsInfo** enthält die Informationen zu den Formularanlagen.
* Die Spalten **telephoneNumber** enthält die Mobiltelefonnummer der Person, die das Formular ausfüllt.

Erstellen Sie die Datenbank durch Import der [Datenbankschema](assets/data-base-schema.sql)
Verwendung von MySQL Workbench.

## Erstellen von Formulardatenmodellen

Erstellen Sie das Formulardatenmodell und basieren Sie es auf der Datenquelle, die Sie im vorherigen Schritt erstellt haben.
Konfigurieren Sie die **get** -Dienst dieses Formulardatenmodells verwenden, wie im Screenshot unten dargestellt.
Stellen Sie sicher, dass Sie kein Array im **get** Dienst.

Zweck dieser **get** -Dienst ist es, die mit der Anwendungs-ID verknüpfte Telefonnummer abzurufen.

![get-service](assets/get-service.JPG)

Dieses Formulardatenmodell wird dann im **MyAccountForm** um die mit der Anwendungs-ID verknüpfte Telefonnummer abzurufen.

## Nächste Schritte

[Schreiben von Code zum Speichern von Formularanlagen](./store-form-attachments.md)
