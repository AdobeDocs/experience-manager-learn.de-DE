---
title: Erstellen von Datenbanktabellen
description: Datenbank erstellen, die vom Formulardatenmodell verwendet wird
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# Erstellen von Datenbanktabellen

Das Formulardatenmodell kann auf RDBMS-, RESTfull-, SOAP- oder OData-Quellen basieren. Der Schwerpunkt dieses Kurses liegt auf der Voranmeldung adaptiver Formulare mit Formulardatenmodell, das von der RDBMS-Datenquelle unterstützt wird. Für diese Übung wurde die MYSQL-Datenbank verwendet. Die folgenden zwei Tabellen veranschaulichen den Anwendungsfall

* **newhire** table - Diese Tabelle speichert die neuesten Informationen

   ![newhire](assets/newhire-table.png)


* **Tabelle &quot;Begünstigte** &quot;: Hier werden neue Begünstigte gespeichert

   ![Begünstigte](assets/beneficiaries-table.png)

Sie können die [SQL-Datei](assets/db-schema.sql) mit MySQL Workbench importieren, um sie in Tabellen mit einigen Musterdaten zu erstellen.