---
title: Erstellen von Datenbanktabellen
description: Erstellen einer Datenbank für das Formulardatenmodell
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
duration: 21
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '105'
ht-degree: 100%

---

# Erstellen von Datenbanktabellen

Das Formulardatenmodell kann auf RDBMS-, RESTfull-, SOAP- oder OData-Quellen basieren. Der Schwerpunkt dieses Kurses liegt auf dem Vorausfüllen des adaptiven Formulars mithilfe eines Formulardatenmodells, das von einer RDBMS-Datenquelle unterstützt wird. Für dieses Tutorial wurde die MYSQL-Datenbank verwendet. Wir haben die beiden folgenden Tabellen erstellt, um den Anwendungsfall zu demonstrieren.

* Tabelle **newhire**: In dieser Tabelle werden die Informationen zu neuen Mitarbeitenden gespeichert.

  ![Tabelle „newhire“](assets/newhire-table.png)


* Tabelle **beneficiaries**: Hier werden die Begünstigten der neuen Mitarbeitenden gespeichert.

  ![Tabelle „beneficiaries“](assets/beneficiaries-table.png)

Sie können die [SQL-Datei](assets/db-schema.sql) mithilfe von MySQL Workbench importieren, um Tabellen mit Beispieldaten zu erstellen.

## Nächste Schritte

[Konfigurieren eines Formulardatenmodells](./configuring-form-data-model.md)
