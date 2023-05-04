---
title: Erstellen von Datenbanktabellen
description: Erstellen einer Datenbank zur Verwendung durch das Formulardatenmodell
feature: Adaptive Forms
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 4%

---

# Datenbanktabellen erstellen

Das Formulardatenmodell kann auf RDBMS-, RESTfull-, SOAP- oder OData-Quellen basieren. Der Schwerpunkt dieses Kurses liegt auf der Vorab-Einreichung des adaptiven Formulars mithilfe des Formulardatenmodells, das von der RDBMS-Datenquelle unterstützt wird. Für diese Anleitung wurde die MYSQL-Datenbank verwendet. Wir haben die folgenden beiden Tabellen erstellt, um den Anwendungsfall zu demonstrieren

* **newhire** table - Diese Tabelle speichert die neuen Informationen

   ![newhire](assets/newhire-table.png)


* **Begünstigte** table - Hier werden Neubegünstigte gespeichert

   ![Begünstigte](assets/beneficiaries-table.png)

Sie können die [sql-Datei](assets/db-schema.sql) Verwendung von MySQL Workbench zum Erstellen in Tabellen mit Beispieldaten.

## Nächste Schritte

[Formulardatenmodell konfigurieren](./configuring-form-data-model.md)
