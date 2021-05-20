---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Mehrteiliges Tutorial, das Sie durch die Schritte führt, die zum Speichern und Abrufen von Formulardaten erforderlich sind
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 6%

---

# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, mit denen AEM die Integration in externe Datenbanken ermöglicht. Eine der gängigsten und gängigsten Methoden der Datenbankintegration ist die Verwendung von Apache Sling Connection Pooled DataSource-Konfigurationseigenschaften über [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechenden [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM herunterzuladen und bereitzustellen.
Erstellen Sie Apache Sling Connection Pooled DataSource und geben Sie die Eigenschaften wie im Screenshot unten angegeben an. Das Datenbankschema wird Ihnen im Rahmen dieses Tutorials bereitgestellt.

![data-source](assets/save-continue.PNG)

Datenbank hat eine Tabelle namens formdata mit den drei Spalten, wie im Screenshot unten dargestellt.

![data-base](assets/data-base-tables.PNG)

Die SQL-Datei zum Erstellen des Schemas kann [von hier heruntergeladen werden](assets/form-data-db.sql). Sie müssen diese Datei mithilfe von MySql Workbench importieren, um das Schema und die Tabelle zu erstellen.

>[!NOTE]
>Benennen Sie Ihre Datenquelle **SaveAndContinue**. Der Beispielcode verwendet den Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |


