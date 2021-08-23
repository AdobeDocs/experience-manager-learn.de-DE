---
title: Verwenden der Batch-API zum Generieren interaktiver Kommunikationsdokumente
description: Beispiel-Assets zum Generieren von Druckkanaldokumenten mit der Batch-API
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 7%

---


# Batch-API

Sie können die Batch-API verwenden, um mehrere interaktive Kommunikationen aus einer Vorlage zu erstellen. Die Vorlage ist eine interaktive Kommunikation ohne Daten. Die Batch-API kombiniert Daten mit einer Vorlage, um eine interaktive Kommunikation zu erzeugen. Die API ist bei der Massenproduktion interaktiver Kommunikation nützlich. Zum Beispiel Telefonrechnungen, Kreditkartenauszüge für mehrere Kunden.

[Weitere Informationen zur Batch-Generierungs-API](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Dieser Artikel enthält Beispiel-Assets zum Generieren interaktiver Kommunikationsdokumente mithilfe der Batch-API.

## Stapelgenerierung mithilfe eines überwachten Ordners

* Importieren Sie die Vorlage [Interaktive Kommunikation](assets/Beneficiaries-confirmation.zip) in Ihren AEM Forms-Server.
* Importieren Sie die [Konfiguration des überwachten Ordners](assets/batch-generation-api.zip). Dadurch wird ein Ordner mit dem Namen `batchAPI` in Ihrem C-Laufwerk erstellt.

**Wenn Sie AEM Forms unter Nicht-Windows-Betriebssystemen ausführen, führen Sie die folgenden 3 Schritte aus:**

1. [Überwachten Ordner öffnen](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Wählen Sie BatchAPIWatchedFolder und klicken Sie auf Bearbeiten.
3. Ändern Sie den Pfad entsprechend Ihrem Betriebssystem.

![path](assets/watched-folder-batch-api-basic.PNG)

* Laden Sie den Inhalt von [zip file](assets/jsonfile.zip) herunter und extrahieren Sie ihn. Die ZIP-Datei enthält den Ordner `jsonfile` , der die Datei `beneficiaries.json` enthält. Diese Datei verfügt über die Daten, um 3 Dokumente zu generieren.

* Legen Sie den Ordner `jsonfile` im Eingabeordner Ihres überwachten Ordners ab.
* Nachdem der Ordner zur Verarbeitung aufgenommen wurde, überprüfen Sie den Ergebnisordner des überwachten Ordners. Es sollten 3 generierte PDF-Dateien angezeigt werden.

## Stapelgenerierung mit REST-Anforderungen

Sie können die [Batch-API](https://helpx.adobe.com/de/experience-manager/6-5/forms/javadocs/index.html) über REST-Anfragen aufrufen. Sie können REST-Endpunkte für andere Anwendungen verfügbar machen, um die API zum Generieren von Dokumenten aufzurufen.
Die bereitgestellten Beispiel-Assets stellen den REST-Endpunkt zum Generieren von Dokumenten zur interaktiven Kommunikation bereit. Das Servlet akzeptiert die folgenden Parameter:

* fileName - Speicherort der Datendatei im Dateisystem.
* templatePath - IC-Vorlagenpfad
* saveLocation - Speicherort zum Speichern der generierten Dokumente im Dateisystem
* channelType - Print,Web oder beides
* recordId - JSON-Pfad zum Element zum Festlegen des Namens einer interaktiven Kommunikation

Der folgende Screenshot zeigt die Parameter und deren Werte
![Beispielanfrage](assets/generate-ic-batch-servlet.PNG)

## Bereitstellen von Beispiel-Assets auf Ihrem Server

* Importieren Sie [ICTemplate](assets/ICTemplate.zip) mithilfe von [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Importieren Sie [Benutzerdefinierter Submit-Handler](assets/BatchAPICustomSubmit.zip) mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Importieren Sie [Adaptives Formular](assets/BatchGenerationAPIAF.zip) mithilfe der [Forms- und Document-Benutzeroberfläche](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Benutzerdefiniertes OSGI-Bundle](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) mithilfe von [Felix Web Console](http://localhost:4502/system/console/bundles) bereitstellen und starten
* [Trigger-Stapelgenerierung durch Senden des Formulars](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
