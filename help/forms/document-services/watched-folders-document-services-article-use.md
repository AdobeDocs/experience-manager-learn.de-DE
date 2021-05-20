---
title: Verwenden von überwachten Ordnern in AEM Forms
seo-title: Verwenden von überwachten Ordnern in AEM Forms
description: Überwachte Ordner in AEM Forms konfigurieren und verwenden
seo-description: Überwachte Ordner in AEM Forms konfigurieren und verwenden
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: Ausgabe-Service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 21%

---


# Verwenden von überwachten Ordnern in AEM Forms{#using-watched-folders-in-aem-forms}

Ein Administrator kann einen Netzwerkordner konfigurieren, der als überwachter Ordner bezeichnet wird, sodass ein vorkonfigurierter Workflow-, Dienst- oder Skriptvorgang zur Verarbeitung der Datei aufgerufen wird, wenn ein Benutzer eine Datei (z. B. eine PDF-Datei) in diesem überwachten Ordner ablegt. Nachdem der Dienst den vorgesehenen Vorgang ausgeführt hat, wird die Ergebnisdatei in einem angegebenen Ausgabeordner gespeichert. Weitere Informationen zu Workflows, Diensten und Skripten.

Um mehr über das Erstellen eines überwachten Ordners zu erfahren, klicken Sie [hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Überwachte Ordner werden verwendet, um Dokumente im Batch-Modus zu generieren. Mithilfe des Mechanismus für überwachte Ordner können Sie interaktive Kommunikation für den Druckkanal generieren oder den Ausgabedienst verwenden, um Daten mit der Vorlage zusammenzuführen.

In diesem Artikel wird der Anwendungsfall der Zusammenführung von Daten mit einer Vorlage mithilfe des Ausgabediensts über den Mechanismus für überwachte Ordner behandelt.

Der OSGi-Dienst Output ist eine Komponente der AEM Document Services. Der Output-Dienst unterstützt verschiedene Ausgabeformate und Ausgabeentwurfsfunktionen von AEM Forms Designer. Mit dem Output-Dienst können Sie XFA-Vorlagen und XML-Daten in verschiedene Druckformate konvertieren.

Um mehr über den Output-Dienst zu erfahren, klicken Sie [bitte hier](https://helpx.adobe.com/aem-forms/6/output-service.html).
Gehen Sie wie folgt vor, um überwachte Ordner auf Ihrem System einzurichten:
* [Laden Sie den Inhalt der ZIP-Datei herunter und extrahieren Sie ihn.](assets/outputservicewatchedfolderkt.zip) Diese ZIP-Datei enthält das Paket zum Erstellen eines überwachten Ordners und Beispieldateien zum Testen des Ausgabediensts mithilfe des Mechanismus für überwachte Ordner
   * Windows System

      * Importieren Sie outputServiceWatchedfolder.zip mit Package Manager in AEM
      * Dadurch wird ein überwachter Ordner namens &quot;outputServiceWatchedfolder&quot;auf Ihrem C-Laufwerk erstellt.
   * Nicht-Windows-System
      * [Öffnen Sie die Konfigurationseinstellung des überwachten Ordners](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Legen Sie die Ordnerpfadeigenschaft des Outservice-Knotens fest, um auf einen geeigneten Speicherort zu verweisen
      * Speichern Sie Ihre Änderungen
      * Der oben genannte Speicherort ist Ihr überwachter Ordner.

Legen Sie die Ordner SamplePdfFile und SampleXdpFile im Eingabeordner des überwachten Ordners ab. Bei erfolgreicher Verarbeitung der Dateien werden die Ergebnisse im Ergebnisordner Ihres überwachten Ordners abgelegt.


>[!NOTE]
>
>Wenn das mit dem überwachten Ordner verknüpfte Skript mehr als eine Datei benötigt, müssen Sie einen Ordner erstellen, alle erforderlichen Dateien im Ordner ablegen und den Ordner im Eingabeordner Ihres überwachten Ordners ablegen.
>
>Wenn das mit dem überwachten Ordner verknüpfte Skript nur eine Eingabedatei benötigt, können Sie die Datei direkt im Eingabeordner Ihres überwachten Ordners ablegen

