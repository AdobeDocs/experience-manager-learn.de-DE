---
title: Acroforms mit AEM Forms
description: Ein Lernprogramm, das schrittweise durch die Erstellung eines adaptiven Formulars mit Acroform und das Zusammenführen der Daten führt, um eine PDF-Datei zu erhalten. Die PDF-Datei mit den zusammengeführten Daten kann dann zum Signieren mit Adobe Sign gesendet werden.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 4%

---


# Adaptives Forms aus Acroforms erstellen

Organisationen haben eine Vielzahl von Formularen. Einige dieser Formulare werden in Microsoft Word erstellt und in PDF konvertiert. Diese Formulare können standardmäßig nicht mit Adobe Reader oder Acrobat ausgefüllt werden. Damit diese Formulare mit Acrobat oder Reader ausgefüllt werden können, müssen wir diese Formulare in Acroform konvertieren. Acroforms sind Formulare, die mit Acrobat erstellt wurden. Dieser Artikel erläutert schrittweise das Erstellen eines adaptiven Formulars aus Acroform und das Zusammenführen der Daten in Acroform, um die PDF-Datei abzurufen. Die PDF-Datei mit den zusammengeführten Daten kann auch zum Signieren mit Adobe Sign gesendet werden.

>[!NOTE]
>
>Wenn Sie AEM Forms 6.5 verwenden, verwenden Sie bitte die Automated forms conversion-Funktion.

## Voraussetzungen

* AEM Forms 6.3 oder 6.4 installiert und konfiguriert
* Zugang zu Adobe Acrobat
* Vertrautheit mit AEM/AEM Forms.

### Die folgenden Funktionen sind erforderlich, damit diese Funktion auf Ihrem System funktioniert

* Laden Sie die Pakete mit der [Felix Web Console](http://localhost:4502/system/console/bundles) herunter und stellen Sie sie bereit
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Laden Sie dieses Paket herunter und importieren Sie es in AEM](assets/acro-form-aem-form.zip). Dieses Paket enthält den Beispielarbeitsablauf und die HTML-Seite zum Erstellen von XSD aus acroform
* Öffnen Sie [configMgr](http://localhost:4502/system/console/configMgr)
   * Suchen Sie nach &quot;Apache Sling Service User Mapper Service&quot; und klicken Sie auf , um die Eigenschaften zu öffnen
   * Klicken Sie auf das Symbol `+` (Plus), um die folgende Dienstzuordnung hinzuzufügen:
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Klicken Sie auf &quot;Speichern&quot;
