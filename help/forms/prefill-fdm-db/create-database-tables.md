---
title: Erstellen von Datenbanktabellen
description: Datenbank erstellen, die vom Formulardatenmodell verwendet wird
feature: adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---


# Erstellen von Datenbanktabellen

Das Formulardatenmodell kann auf RDBMS-, RESTfull-, SOAP- oder OData-Quellen basieren. Der Schwerpunkt dieses Kurses liegt auf der Voranmeldung adaptiver Formulare mit Formulardatenmodell, das von der RDBMS-Datenquelle unterstützt wird. Für diese Übung wurde die MYSQL-Datenbank verwendet. Die folgenden zwei Tabellen veranschaulichen den Anwendungsfall

* **** newhiretable - Diese Tabelle speichert die neuen Informationen

   ![newhire](assets/newhire-table.png)


* **** Begünstigter: Hier werden neue Begünstigte gespeichert

   ![Begünstigte](assets/beneficiaries-table.png)

Sie können die Datei [sql](assets/db-schema.sql) mithilfe der MySQL-Workbench importieren, um sie in Tabellen mit einigen Musterdaten zu erstellen.