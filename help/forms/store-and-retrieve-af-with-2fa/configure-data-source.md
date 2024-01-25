---
title: Konfigurieren einer Datenquelle
description: Erstellen einer Datenquelle mit Verweis auf die MySQL-Datenbank
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 77
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 100%

---

# Konfigurieren einer Datenquelle

Es gibt viele Möglichkeiten, wie AEM die Integration mit einer externen Datenbank ermöglicht. Bei einer der gängigsten und geläufigsten Methoden der Datenbankintegration werden die Konfigurationseigenschaften der Apache Sling Connection Pooled DataSource über [configMgr](http://localhost:4502/system/console/configMgr) verwendet.
Der erste Schritt besteht darin, die entsprechenden [MySQL-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) herunterzuladen und in AEM bereitzustellen.
Legen Sie dann die Sling Connection Pooled DataSource-Eigenschaften speziell für Ihre Datenbank fest.  Der folgende Screenshot zeigt die für dieses Tutorial verwendeten Einstellungen. Das Datenbankschema wird Ihnen im Rahmen dieser Tutorial-Assets bereitgestellt.

>[!NOTE]
>Bitte stellen Sie sicher, dass Sie Ihre Datenquelle `StoreAndRetrieveAfData` benennen, da dies der Name ist, der im OSGi-Dienst verwendet wird.


![data-source](assets/data-source.JPG)

| Eigenschaftsname | Eigenschaftswert |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Datenquellenname | StoreAndRetrieveAfData |   |
| JDBC-Laufwerkklasse | jdbc:mysql://localhost:3306/aemformstutorial |   |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&amp;autoReconnect=true |   |
|                     |                                                                                    |   |


## Erstellen einer Datenbank


Die folgende Datenbank wurde für diesen Anwendungsfall verwendet. Die Datenbank verfügt über eine Tabelle mit dem Namen `formdatawithattachments` mit den 4 Spalten, wie im Screenshot unten dargestellt.
![data-base](assets/table-schema.JPG)

* Die Spalte **afdata** enthält die Daten des adaptiven Formulars.
* Die Spalte **attachmentsInfo** enthält die Informationen zu den Formularanlagen.
* Die Spalte **telephoneNumber** enthält die Mobiltelefonnummer der Person, die das Formular ausfüllt.

Erstellen Sie die Datenbank durch Import des [Datenbankschemas](assets/data-base-schema.sql)
unter Verwendung von MySQL Workbench.

## Erstellen eines Formulardatenmodells

Erstellen Sie das Formulardatenmodell und basieren Sie es auf der Datenquelle, die Sie im vorherigen Schritt erstellt haben.
Konfigurieren Sie den **get**-Dienst dieses Formulardatenmodells wie im folgenden Screenshot gezeigt.
Stellen Sie sicher, dass Sie im **get**-Dienst kein Array zurückgeben.

Der Zweck dieses **get**-Dienstes besteht darin, die mit der Anwendungs-ID verbundene Telefonnummer abzurufen.

![get-service](assets/get-service.JPG)

Dieses Formulardatenmodell wird dann in **MyAccountForm** verwendet, um die mit der Anwendungs-ID verknüpfte Telefonnummer abzurufen.

## Nächste Schritte

[Schreiben von Code zum Speichern von Formularanlagen](./store-form-attachments.md)
