---
title: Acroforms mit AEM Forms
description: Ein Tutorial, das durch das Erstellen eines adaptiven Formulars mit Acroform und das Zusammenführen der Daten führt, um eine PDF zu erhalten. Die PDF mit den zusammengeführten Daten kann dann zum Signieren mit Acrobat Sign gesendet werden.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.5
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 4%

---


# Erstellen adaptiver Forms aus Acroforms

Organisationen haben eine große Auswahl an Formularen. Einige dieser Formulare werden in Microsoft Word erstellt und in PDF konvertiert. Diese Formulare können standardmäßig nicht mit Adobe Reader oder Acrobat ausgefüllt werden. Damit diese Formulare mit Acrobat oder Reader ausgefüllt werden können, müssen diese Formulare in Acroform konvertiert werden. Acroforms sind Formulare, die mit Acrobat erstellt werden. Dieser Artikel erläutert die Erstellung eines adaptiven Formulars aus Acroform und die Zusammenführung der Daten in Acroform, um die PDF zu erhalten. Die PDF mit den zusammengeführten Daten kann auch zum Signieren mit Acrobat Sign gesendet werden.

>[!NOTE]
>
>Wenn Sie AEM Forms 6.5 verwenden, verwenden Sie bitte die Automated forms conversion-Funktion.

## Voraussetzungen

* AEM Forms 6.3 oder 6.4 installiert und konfiguriert
* Zugriff auf Adobe Acrobat
* die AEM/AEM Forms.

### Die folgenden Schritte sind erforderlich, damit diese Funktion auf Ihrem System funktioniert

* Herunterladen und Bereitstellen der Bundles mithilfe des [Felix-Webkonsole](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Laden Sie dieses Paket herunter und importieren Sie es in AEM](assets/acro-form-aem-form.zip). Dieses Paket enthält den Beispiel-Workflow und die HTML-Seite zum Erstellen von XSD aus Acrobat
* Öffnen Sie die [configMgr](http://localhost:4502/system/console/configMgr)
   * Suchen Sie nach &quot;Apache Sling Service User Mapper Service&quot;und klicken Sie auf , um die Eigenschaften zu öffnen.
   * Klicken Sie auf `+` Symbol (Plus) zum Hinzufügen der folgenden Dienstzuordnung
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Klicken Sie auf &quot;Speichern&quot;
