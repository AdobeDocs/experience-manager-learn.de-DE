---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank – Konfigurieren der Datenquelle
description: Mehrteiliges Tutorial, das Sie durch die Schritte zum Speichern und Abrufen von Formulardaten führt
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '193'
ht-degree: 100%

---

# Konfigurieren einer Datenquelle

Es gibt verschiedene Möglichkeiten, mit denen AEM die Integration in externe Datenbanken unterstützt. Bei einer der gängigsten und geläufigsten Methoden der Datenbankintegration werden die Konfigurationseigenschaften der Apache Sling Connection Pooled DataSource über [configMgr](http://localhost:4502/system/console/configMgr) verwendet.
Der erste Schritt besteht darin, die entsprechenden [MySql-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) herunterzuladen und in AEM bereitzustellen.
Erstellen Sie eine Apache Sling Connection Pooled DataSource und geben Sie die Eigenschaften so wie im Screenshot unten an. Das Datenbankschema wird Ihnen im Rahmen dieser Tutorial-Assets bereitgestellt.

![data-source](assets/save-continue.PNG)

Die Datenbank verfügt über eine Tabelle namens „formdata“ mit den drei im Screenshot unten dargestellten Spalten.

![data-base](assets/data-base-tables.PNG)

Die SQL-Datei zum Erstellen des Schemas kann [hier heruntergeladen werden](assets/form-data-db.sql). Sie müssen diese Datei mithilfe von MySQL Workbench importieren, um das Schema und die Tabelle zu erstellen.

>[!NOTE]
>Stellen Sie sicher, dass Sie Ihrer Datenquelle den Namen **SaveAndContinue** geben. Der Beispiel-Code verwendet diesen Namen, um eine Verbindung zur Datenbank herzustellen.

| Eigenschaftsname | Wert |
| ------------------------|---------------------------------------|
| Datenquellenname | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |
