---
title: Erstellen von Datenbanktabellen
description: Datenbank erstellen, die vom Formulardatenmodell verwendet wird
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Entwicklung
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 3%

---


# Erstellen von Datenbanktabellen

Das Formulardatenmodell kann auf RDBMS-, RESTfull-, SOAP- oder OData-Quellen basieren. Der Schwerpunkt dieses Kurses liegt auf der Voranmeldung adaptiver Formulare mit Formulardatenmodell, das von der RDBMS-Datenquelle unterstützt wird. Für diese Übung wurde die MYSQL-Datenbank verwendet. Die folgenden zwei Tabellen veranschaulichen den Anwendungsfall

* **** newhiretable - Diese Tabelle speichert die neuen Informationen

   ![newhire](assets/newhire-table.png)


* **** Begünstigter: Hier werden neue Begünstigte gespeichert

   ![Begünstigte](assets/beneficiaries-table.png)

Sie können die Datei [sql](assets/db-schema.sql) mithilfe der MySQL-Workbench importieren, um sie in Tabellen mit einigen Musterdaten zu erstellen.