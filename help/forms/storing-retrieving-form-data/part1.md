---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Lernprogramm mit mehreren Teilen, um Sie durch die Schritte zum Speichern und Abrufen von Formulardaten zu führen
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 6%

---

# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, AEM die Integration mit externen Datenbanken zu ermöglichen. Eine der gebräuchlichsten und gängigsten Methoden der Datenbankintegration ist die Verwendung der Konfigurationseigenschaften von Apache Sling Connection Pooled DataSource über [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechenden [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM herunterzuladen und bereitzustellen.
Erstellen Sie Apache Sling Connection Pooled DataSource und stellen Sie die Eigenschaften wie im Screenshot unten angegeben bereit. Das Datenbank-Schema wird Ihnen im Rahmen dieses Lernprogramms bereitgestellt.

![data-source](assets/save-continue.PNG)

Datenbank hat eine Tabelle namens formdata mit den 3 Spalten, wie im Screenshot unten dargestellt.

![data-base](assets/data-base-tables.PNG)

Die SQL-Datei zum Erstellen des Schemas kann [von hier heruntergeladen werden. ](assets/form-data-db.sql) Sie müssen diese Datei mit MySql Workbench importieren, um das Schema und die Tabelle zu erstellen.

>[!NOTE]
>Vergewissern Sie sich, dass Sie Ihre Datenquelle **SaveAndContinue** benennen. Der Beispielcode verwendet den Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungsuri | jdbc:mysql://localhost:3306/aemformstutorial |


