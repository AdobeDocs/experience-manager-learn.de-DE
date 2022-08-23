---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank - Datenquelle konfigurieren
description: Mehrteiliges Tutorial, das Sie durch die Schritte führt, die zum Speichern und Abrufen von Formulardaten erforderlich sind
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 5%

---

# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, mit denen AEM die Integration in externe Datenbanken ermöglicht. Eine der gängigsten und gängigsten Methoden der Datenbankintegration ist die Verwendung der Konfigurationseigenschaften von Apache Sling Connection Pooled DataSource über die [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechende [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Erstellen Sie Apache Sling Connection Pooled DataSource und geben Sie die Eigenschaften wie im Screenshot unten angegeben an. Das Datenbankschema wird Ihnen im Rahmen dieses Tutorials bereitgestellt.

![data-source](assets/save-continue.PNG)

Datenbank hat eine Tabelle namens formdata mit den drei Spalten, wie im Screenshot unten dargestellt.

![data-base](assets/data-base-tables.PNG)

Die SQL-Datei zum Erstellen des Schemas kann [heruntergeladen von hier](assets/form-data-db.sql). Sie müssen diese Datei mithilfe von MySql Workbench importieren, um das Schema und die Tabelle zu erstellen.

>[!NOTE]
>Benennen Sie Ihre Datenquelle. **SaveAndContinue**. Der Beispielcode verwendet den Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert |
| ------------------------|---------------------------------------|
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |
