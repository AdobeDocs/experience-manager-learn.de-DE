---
title: Bereitstellen des Beispiels auf Ihrem lokalen Server
description: Ein mehrteiliges Tutorial, das Sie durch die Schritte führt, die beim Abfragen von im Azure-Portal gespeicherten Formularübermittlungen erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 44841a3c-85e0-447f-85e2-169a451d9c68
duration: 20
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 100%

---

# Bereitstellen des Beispiels auf Ihrem lokalen Server

Um diesen Anwendungsfall auf Ihrem lokalen Server zum Laufen zu bringen, befolgen Sie die unten aufgeführten Schritte. Es wird davon ausgegangen, dass Ihre AEM-Instanz auf localhost, Port 4502, ausgeführt wird.

* [Installieren Sie das Paket](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) mit dem Package Manager.

* Geben Sie die Anmeldedaten für das Azure-Portal mit dem OSGi configMgr an.
  ![Azure-Portal](assets/azure-portal-config.png) 
Stellen Sie sicher, dass der Speicher-URI mit einem Schrägstrich endet und das SAS-Token mit einem „?“ beginnt.
* Navigieren Sie zu [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo).

* Bearbeiten Sie die Authentifizierungseinstellungen der folgenden drei Datenquellen entsprechend Ihrer Umgebung.
  ![Datenquellen](assets/fdm-data-sources.png)

* Zeigen Sie das [Kontaktformular](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled) in der Vorschau an und senden Sie es ab.

* [Fragen Sie Ihre Formularübermittlung ab.](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)
