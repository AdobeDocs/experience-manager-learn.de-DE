---
title: Datenbanktabellen erstellen
description: Erstellen einer Datenbank zur Verwendung durch das Formulardatenmodell
feature: Adaptive Formulare
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# Datenbanktabellen erstellen

Das Formulardatenmodell kann auf RDBMS-, RESTfull-, SOAP- oder OData-Quellen basieren. Der Schwerpunkt dieses Kurses liegt auf der Vorab-Einreichung des adaptiven Formulars mithilfe des Formulardatenmodells, das von der RDBMS-Datenquelle unterstützt wird. Für diese Anleitung wurde die MYSQL-Datenbank verwendet. Wir haben die folgenden beiden Tabellen erstellt, um den Anwendungsfall zu demonstrieren

* **** newhiretable - Diese Tabelle speichert die neuen Informationen

   ![newhire](assets/newhire-table.png)


* **** Begünstigter - Hier werden neue Begünstigte gespeichert.

   ![Begünstigte](assets/beneficiaries-table.png)

Sie können die [sql-Datei](assets/db-schema.sql) mithilfe von MySQL Workbench importieren, um sie in Tabellen mit Beispieldaten zu erstellen.