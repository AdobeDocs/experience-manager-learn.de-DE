---
title: Bereitstellen des Beispiels auf Ihrem lokalen Server
description: Mehrteiliges Tutorial, um Sie durch die Schritte zu führen, die für die Abfrage von Formularübermittlungen im Azure Portal erforderlich sind
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Bereitstellen des Beispiels auf Ihrem lokalen Server

Damit dieser Anwendungsfall auf Ihrem lokalen Server funktioniert, führen Sie die unten aufgeführten Schritte aus. Es wird davon ausgegangen, dass Ihre AEM-Instanz auf localhost, 4502 Port ausgeführt wird.

* [Installieren Sie das Paket](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) mit Package Manager.

* Stellen Sie die Azure Portal-Anmeldedaten mithilfe des OSGi configMgr bereit.
  ![azure-portal](assets/azure-portal-config.png)
Stellen Sie sicher, dass der Speicher-URI in einem Schrägstrich endet und das SAS-Token mit einem ? beginnt.
* Navigieren Sie zu [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Bearbeiten Sie die Authentifizierungseinstellungen der folgenden 3 Datenquellen entsprechend Ihrer Umgebung.
  ![data-sources](assets/fdm-data-sources.png)

* Vorschau erstellen und senden [Kontaktformular](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Formularübermittlung abfragen](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

