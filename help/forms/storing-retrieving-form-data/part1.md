---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Lernprogramm mit mehreren Teilen, um Sie durch die Schritte zum Speichern und Abrufen von Formulardaten zu führen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 3%

---

# Datenquelle konfigurieren

Es gibt viele Möglichkeiten, AEM die Integration mit externen Datenbanken zu ermöglichen. Eine der gebräuchlichsten und gängigsten Methoden der Datenbankintegration ist die Verwendung der Konfigurationseigenschaften von Apache Sling Connection Pooled DataSource über die [configMgr](http://localhost:4502/system/console/configMgr).
Der erste Schritt besteht darin, die entsprechenden [My SQL-Treiber](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM herunterzuladen und bereitzustellen.
Legen Sie dann die Eigenschaften Sling Connection Pooled DataSource fest. Diese Eigenschaften sind spezifisch für Ihre Datenbank. Der folgende Screenshot zeigt die Einstellungen, die für dieses Tutorial verwendet werden. Das Datenbank-Schema wird Ihnen im Rahmen dieses Lernprogramms bereitgestellt.
![data-source](assets/data-source.png)

Datenbank hat eine Tabelle namens formdata mit den 3 Spalten, wie im Screenshot unten![data-base dargestellt](assets/data-base-tables.PNG)
