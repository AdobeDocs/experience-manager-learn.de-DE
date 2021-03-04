---
title: Verwenden der Stapel-API zum Generieren von Dokumenten für interaktive Kommunikation
description: Beispiel-Assets zum Generieren von Dokumenten für den Kanal mit der Batch-API
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 7%

---


# Stapel-API

Sie können die Stapel-API verwenden, um mehrere interaktive Mitteilungen aus einer Vorlage zu erstellen. Die Vorlage ist eine interaktive Kommunikation ohne Daten. Die Stapel-API kombiniert Daten mit einer Vorlage, um eine interaktive Kommunikation zu erzeugen. Die API ist bei der Massenproduktion interaktiver Kommunikation nützlich. Zum Beispiel Telefonrechnungen, Kreditkartenauszüge für mehrere Kunden.

[Weitere Informationen zur API zur Stapelgenerierung](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Dieser Artikel enthält Beispiel-Assets zum Generieren von interaktiven Kommunikations-Dokumenten mit der Stapel-API.

## Stapelgenerierung mit überwachtem Ordner

* Importieren Sie die Vorlage [Interaktive Kommunikation](assets/Beneficiaries-confirmation.zip) in Ihren AEM Forms-Server.
* Importieren Sie die Konfiguration des überwachten Ordners [a1/>. ](assets/batch-generation-api.zip) Dadurch wird ein Ordner mit dem Namen `batchAPI` im Laufwerk C erstellt.

**Wenn Sie AEM Forms unter einem Nicht-Windows-Betriebssystem ausführen, führen Sie die folgenden 3 Schritte aus:**

1. [Überwachten Ordner öffnen](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Wählen Sie BatchAPIWatchedFolder und klicken Sie auf Bearbeiten.
3. Ändern Sie den Pfad entsprechend Ihrem Betriebssystem.

![path](assets/watched-folder-batch-api-basic.PNG)

* Laden Sie den Inhalt der [ZIP-Datei](assets/jsonfile.zip) herunter und extrahieren Sie ihn. Die ZIP-Datei enthält den Ordner `jsonfile`, der die Datei `beneficiaries.json` enthält. Diese Datei enthält die Daten, um 3 Dokumente zu generieren.

* Legen Sie den Ordner `jsonfile` in den Eingabeordner Ihres überwachten Ordners ab.
* Nachdem der Ordner zur Verarbeitung aufgenommen wurde, überprüfen Sie den Ergebnisordner Ihres überwachten Ordners. Es sollten 3 generierte PDF-Dateien angezeigt werden.

## Stapelgenerierung mit REST-Anforderungen

Sie können die [Stapel-API](https://helpx.adobe.com/de/experience-manager/6-5/forms/javadocs/index.html) über REST-Anforderungen aufrufen. Sie können REST-Endpunkte für andere Anwendungen bereitstellen, um die API zum Generieren von Dokumenten aufzurufen.
Die bereitgestellten Beispiel-Assets stellen den REST-Endpunkt zum Generieren von Dokumenten der interaktiven Kommunikation dar. Das Servlet akzeptiert die folgenden Parameter:

* fileName - Speicherort der Datendatei im Dateisystem.
* templatePath - IC-Vorlagenpfad
* saveLocation - Speicherort zum Speichern der generierten Dokumente im Dateisystem
* channelType - Print, Web oder beides
* recordId - JSON-Pfad zu einem Element zum Festlegen des Namens einer interaktiven Kommunikation

Der folgende Screenshot zeigt die Parameter und ihre Werte
![Beispielanforderung](assets/generate-ic-batch-servlet.PNG)

## Beispielelemente auf dem Server bereitstellen

* [ICTemplate](assets/ICTemplate.zip) mithilfe von [Package Manager](http://localhost:4502/crx/packmgr/index.jsp) importieren
* [Benutzerspez. Sendehandler](assets/BatchAPICustomSubmit.zip) mit [Paketmanager](http://localhost:4502/crx/packmgr/index.jsp) importieren
* [Adaptives Formular](assets/BatchGenerationAPIAF.zip) mithilfe der [Forms- und Dokument-Schnittstelle](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) importieren
* [Benutzerdefiniertes OSGI-Bundle](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) mithilfe von [Felix-Webkonsole ](http://localhost:4502/system/console/bundles) bereitstellen und Beginn
* [Stapelgenerierung von Triggern durch Senden des Formulars](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
