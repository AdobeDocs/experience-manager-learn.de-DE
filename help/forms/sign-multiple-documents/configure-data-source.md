---
title: AEM Datenquelle konfigurieren
description: MySQL-gesicherte Datenquelle zum Speichern und Abrufen von Formulardaten konfigurieren
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 7%

---

# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, AEM die Integration mit einer externen Datenbank zu ermöglichen. Eine der gängigsten Methoden zur Integration einer Datenbank ist die Verwendung der Konfigurationseigenschaften von Apache Sling Connection Pooled DataSource über [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechenden [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM herunterzuladen und bereitzustellen.
Erstellen Sie Apache Sling Connection Pooled DataSource und stellen Sie die Eigenschaften wie im Screenshot unten angegeben bereit. Das Datenbank-Schema wird Ihnen im Rahmen dieses Lernprogramms bereitgestellt.

![data-source](assets/data-source.PNG)

Datenbank hat eine Tabelle namens formdata mit den 3 Spalten, wie im Screenshot unten dargestellt.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Vergewissern Sie sich, dass Sie Ihre Datenquelle **aemformstutorial** benennen. Der Beispielcode verwendet den Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungsuri | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Die SQL-Datei zum Erstellen des Schemas kann [von hier heruntergeladen werden. ](assets/sign-multiple-forms.sql) Sie müssen diese Datei mit MySql Workbench importieren, um das Schema und die Tabelle zu erstellen.


