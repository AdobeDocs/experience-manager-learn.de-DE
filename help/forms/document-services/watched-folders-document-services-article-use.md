---
title: Verwenden von überwachten Ordnern in AEM Forms
description: Überwachte Ordner in AEM Forms konfigurieren und verwenden
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 25%

---

# Verwenden von überwachten Ordnern in AEM Forms{#using-watched-folders-in-aem-forms}

Ein Administrator kann einen Netzwerkordner konfigurieren, der als überwachter Ordner bezeichnet wird, sodass ein vorkonfigurierter Workflow-, Dienst- oder Skriptvorgang zur Verarbeitung der Datei aufgerufen wird, wenn ein Benutzer eine Datei (z. B. eine PDF-Datei) in diesem überwachten Ordner ablegt. Nachdem der Dienst den vorgesehenen Vorgang ausgeführt hat, wird die Ergebnisdatei in einem angegebenen Ausgabeordner gespeichert. Weitere Informationen zu Workflows, Diensten und Skripten.

Weitere Informationen zum Erstellen eines überwachten Ordners finden Sie unter [Hier klicken](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Überwachte Ordner werden verwendet, um Dokumente im Batch-Modus zu generieren. Mithilfe des Mechanismus für überwachte Ordner können Sie interaktive Kommunikation für den Druckkanal generieren oder den Ausgabedienst verwenden, um Daten mit der Vorlage zusammenzuführen.

In diesem Artikel wird der Anwendungsfall der Zusammenführung von Daten mit einer Vorlage mithilfe des Ausgabediensts über den Mechanismus für überwachte Ordner behandelt.

Der OSGi-Dienst Output ist eine Komponente der AEM Document Services. Der Ausgabedienst unterstützt verschiedene Ausgabeformate und Ausgabedesignfunktionen von AEM Forms Designer. Mit dem Output-Dienst können Sie XFA-Vorlagen und XML-Daten in verschiedene Druckformate konvertieren.

Weitere Informationen zum Output-Dienst finden Sie unter [Bitte klicken Sie hier](https://helpx.adobe.com/aem-forms/6/output-service.html).
Gehen Sie wie folgt vor, um überwachte Ordner auf Ihrem System einzurichten:
* [Herunterladen und Extrahieren des Inhalts der ZIP-Datei](assets/outputservicewatchedfolderkt.zip).Diese ZIP-Datei enthält ein Paket zum Erstellen eines überwachten Ordners und Beispieldateien zum Testen des Ausgabediensts mithilfe des Mechanismus für überwachte Ordner.
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
