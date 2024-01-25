---
title: Verwenden der Batch-API zum Generieren von Dokumenten zur interaktiven Kommunikation
description: Beispiel-Assets zum Generieren von Druckkanaldokumenten mithilfe der Batch-API
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 90
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 100%

---

# Batch-API

Sie können die Batch-API verwenden, um mehrere interaktive Kommunikationen aus einer Vorlage zu erstellen. Die Vorlage ist eine interaktive Kommunikation ohne Daten. Die Batch-API kombiniert Daten mit einer Vorlage, um eine interaktive Kommunikation zu erzeugen. Die API ist bei der Massenproduktion interaktiver Kommunikationen nützlich. Zum Beispiel Telefonrechnungen, Kreditkartenauszüge für mehrere Kunden.

[Weitere Informationen zur Batch-Generierungs-API](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html?lang=de)

Dieser Artikel enthält Beispiel-Assets zum Generieren von Dokumenten zur interaktiven Kommunikation mithilfe der Batch-API.

## Batch-Generierung mithilfe überwachter Ordner

* Importieren Sie die [Vorlage zur interaktiven Kommunikation](assets/Beneficiaries-confirmation.zip) in Ihren AEM Forms-Server.
* Importieren Sie die [Konfiguration für überwachte Ordner](assets/batch-generation-api.zip). Dadurch wird ein Ordner mit dem Namen `batchAPI` auf dem Laufwerk „C:“ erstellt.

**Wenn Sie AEM Forms unter einem Nicht-Windows-Betriebssystem ausführen, führen Sie die drei folgenden Schritte aus:**

1. [Öffnen Sie den überwachten Ordner.](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Wählen Sie „BatchAPIWatchedFolder“ aus und klicken Sie auf „Bearbeiten“.
3. Ändern Sie den Pfad entsprechend Ihrem Betriebssystem.

![path](assets/watched-folder-batch-api-basic.PNG)

* Laden Sie die [ZIP-Datei](assets/jsonfile.zip) herunter und extrahieren Sie deren Inhalt. Die ZIP-Datei enthält den Ordner `jsonfile` mit der Datei `beneficiaries.json`. Diese Datei verfügt über die Daten, um drei Dokumente zu generieren.

* Legen Sie den Ordner `jsonfile` im Eingabeordner Ihres überwachten Ordners ab.
* Nachdem der Ordner der Verarbeitung zugeführt wurde, überprüfen Sie den Ergebnisordner des überwachten Ordners. Es sollten drei PDF-Dateien generiert worden sein.

## Batch-Generierung mithilfe von REST-Anfragen

Sie können die [Batch-API](https://helpx.adobe.com/de/experience-manager/6-5/forms/javadocs/index.html) durch REST-Anfragen aufrufen. Sie können REST-Endpunkte für andere Anwendungen offenlegen, um die API zum Generieren von Dokumenten aufzurufen.
Die bereitgestellten Beispiel-Assets machen den REST-Endpunkt zum Generieren von Dokumenten zur interaktiven Kommunikation verfügbar. Das Servlet akzeptiert die folgenden Parameter:

* fileName: Speicherort der Datendatei im Dateisystem
* templatePath: IC-Vorlagenpfad
* saveLocation: Speicherort zum Speichern der generierten Dokumente im Dateisystem
* channelType: „Print“, „Web“ oder beides
* recordId: JSON-Pfad zum Element, um den Namen für die interaktive Kommunikation festzulegen

Der folgende Screenshot zeigt die Parameter und deren Werte.
![Beispielanfrage](assets/generate-ic-batch-servlet.PNG)

## Bereitstellen von Beispiel-Assets auf Ihrem Server

* Importieren Sie [ICTemplate](assets/ICTemplate.zip) mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).
* Importieren Sie den [benutzerdefinierten Übermittlungs-Handler](assets/BatchAPICustomSubmit.zip) mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).
* Importieren Sie das [adaptive Formular](assets/BatchGenerationAPIAF.zip) mithilfe der Benutzeroberfläche [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Implementieren und starten Sie das [benutzerdefinierte OSGi-Bundle](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) mithilfe der [Felix-Web-Konsole](http://localhost:4502/system/console/bundles).
* [Lösen Sie die Batch-Generierung aus, indem Sie das Formular übermitteln.](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
