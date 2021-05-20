---
title: AEM Datenquelle konfigurieren
description: MySQL-gesicherte Datenquelle zum Speichern und Abrufen von Formulardaten konfigurieren
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 7%

---

# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, mit denen AEM die Integration in eine externe Datenbank ermöglicht. Eine der häufigsten Möglichkeiten zur Integration einer Datenbank besteht darin, die Konfigurationseigenschaften der Apache Sling Connection Pooled DataSource über die Konfigurationseigenschaften [configMgr](http://localhost:4502/system/console/configMgr) zu verwenden.
Der erste Schritt besteht darin, die entsprechenden [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM herunterzuladen und bereitzustellen.
Erstellen Sie Apache Sling Connection Pooled DataSource und geben Sie die Eigenschaften wie im Screenshot unten angegeben an. Das Datenbankschema wird Ihnen im Rahmen dieses Tutorials bereitgestellt.

![data-source](assets/data-source.PNG)

Datenbank hat eine Tabelle namens formdata mit den drei Spalten, wie im Screenshot unten dargestellt.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Benennen Sie Ihre Datenquelle **aemformstutorial**. Der Beispielcode verwendet den Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Die SQL-Datei zum Erstellen des Schemas kann [von hier heruntergeladen werden](assets/sign-multiple-forms.sql). Sie müssen diese Datei mithilfe von MySql Workbench importieren, um das Schema und die Tabelle zu erstellen.


