---
title: AEM Datenquelle konfigurieren
description: MySQL-gesicherte Datenquelle zum Speichern und Abrufen von Formulardaten konfigurieren
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 6%

---

# Konfigurieren einer Datenquelle

Es gibt viele Möglichkeiten, mit denen AEM die Integration in eine externe Datenbank ermöglicht. Eine der häufigsten Möglichkeiten zur Integration einer Datenbank besteht darin, die Konfigurationseigenschaften der Apache Sling Connection Pooled DataSource über die [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechende [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Erstellen Sie Apache Sling Connection Pooled DataSource und geben Sie die Eigenschaften wie im Screenshot unten angegeben an. Das Datenbankschema wird Ihnen im Rahmen dieses Tutorials bereitgestellt.

![data-source](assets/data-source.PNG)

Datenbank hat eine Tabelle namens formdata mit den drei Spalten, wie im Screenshot unten dargestellt.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Benennen Sie Ihre Datenquelle. **aemformstutorial**. Der Beispielcode verwendet den Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert  |
| ------------------------|--------------------------------------- |
| Datenquellenname | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Die SQL-Datei, die das Schema erstellen soll, kann [heruntergeladen von hier](assets/sign-multiple-forms.sql). Sie müssen diese Datei mithilfe von MySql Workbench importieren, um das Schema und die Tabelle zu erstellen.

## Nächste Schritte

[Erstellen eines OSGi-Dienstes zum Speichern und Abrufen von Daten in der Datenbank](./create-osgi-service.md)
