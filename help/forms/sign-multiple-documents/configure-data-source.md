---
title: Konfigurieren einer AEM-Datenquelle
description: Konfigurieren einer MySQL-gesicherten Datenquelle zum Speichern und Abrufen von Formulardaten
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 47
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 100%

---

# Konfigurieren einer Datenquelle

Es gibt viele Möglichkeiten, mit denen AEM die Integration in eine externe Datenbank erlaubt. Eine der häufigsten Möglichkeiten zur Integration einer Datenbank besteht darin, die Konfigurationseigenschaften der Apache Sling Connection Pooled DataSource über [configMgr](http://localhost:4502/system/console/configMgr) zu verwenden.
Der erste Schritt besteht darin, die entsprechenden [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) herunterzuladen und in AEM bereitzustellen.
Erstellen Sie eine Apache Sling Connection Pooled DataSource und geben Sie die Eigenschaften so wie im Screenshot unten an. Das Datenbankschema wird Ihnen im Rahmen dieser Tutorial-Assets bereitgestellt.

![data-source](assets/data-source.PNG)

Die Datenbank verfügt über eine Tabelle namens „formdata“ mit den drei im Screenshot unten dargestellten Spalten.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Vergewissern Sie sich, dass Sie Ihre Datenquelle **aemformstutorial** benennen. Der Beispiel-Code verwendet diesen Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert |
| ------------------------|--------------------------------------- |
| Datenquellenname | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Die SQL-Datei zum Erstellen des Schemas kann [hier heruntergeladen werden](assets/sign-multiple-forms.sql). Sie müssen diese Datei mithilfe von MySQL Workbench importieren, um das Schema und die Tabelle zu erstellen.

## Nächste Schritte

[Erstellen eines OSGi-Dienstes zum Speichern und Abrufen von Daten in der Datenbank](./create-osgi-service.md)
