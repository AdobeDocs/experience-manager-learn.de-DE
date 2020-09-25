---
title: Verwenden von überwachten Ordnern in AEM Forms
seo-title: Verwenden von überwachten Ordnern in AEM Forms
description: Überwachte Ordner in AEM Forms konfigurieren und verwenden
seo-description: Überwachte Ordner in AEM Forms konfigurieren und verwenden
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 20%

---


# Verwenden von überwachten Ordnern in AEM Forms{#using-watched-folders-in-aem-forms}

Ein Administrator kann einen Netzwerkordner konfigurieren, der als überwachter Ordner bezeichnet wird, sodass ein vorkonfigurierter Workflow-, Dienst- oder Skriptvorgang zur Verarbeitung der Datei aufgerufen wird, wenn ein Benutzer eine Datei (z. B. eine PDF-Datei) in diesem überwachten Ordner ablegt. Nachdem der Dienst den vorgesehenen Vorgang ausgeführt hat, wird die Ergebnisdatei in einem angegebenen Ausgabeordner gespeichert. Weitere Informationen zu Workflow, Dienst und Skript.

Weitere Informationen zum Erstellen eines überwachten Ordners erhalten Sie [hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Überwachte Ordner werden zum Generieren von Dokumenten im Stapelmodus verwendet. Mit dem Watched Folder-Mechanismus können Sie interaktive Nachrichten für den Print-Kanal generieren oder mithilfe des Output-Dienstes Daten mit der Vorlage zusammenführen.

Dieser Artikel behandelt den Anwendungsfall des Zusammenführens von Daten mit einer Vorlage mithilfe des Ausgabediensts über den Mechanismus für überwachte Ordner.

Der OSGi-Dienst Output ist eine Komponente der AEM Document Services. Der Output-Dienst unterstützt verschiedene Ausgabeformate und Ausgabeentwurfsfunktionen von AEM Forms Designer. Mit dem Output-Dienst können Sie XFA-Vorlagen und XML-Daten in verschiedene Druckformate konvertieren.

Um mehr über den Output-Dienst zu erfahren, klicken [Sie bitte hier](https://helpx.adobe.com/aem-forms/6/output-service.html).
Gehen Sie wie folgt vor, um überwachten Ordner auf Ihrem System einzurichten:
* [Herunterladen und Extrahieren des Inhalts der ZIP-Datei](assets/outputservicewatchedfolderkt.zip).Diese ZIP-Datei enthält das Paket zum Erstellen des überwachten Ordners und Beispieldateien zum Testen des Ausgabediensts mithilfe des Mechanismus für überwachte Ordner
   * Windows-System

      * Importieren Sie die Datei &quot;outputserviceWatchedfolder.zip&quot;mit dem Package Manager in AEM
      * Dadurch wird ein überwachter Ordner mit dem Namen &quot;outputserviceWatchedfolder&quot;auf Ihrem Laufwerk erstellt.
   * Nicht-Windows-System
      * [Öffnen Sie die Konfigurationseinstellung des überwachten Ordners](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Legen Sie die Ordnerpfadeigenschaft des outservice-Knotens so fest, dass sie auf einen geeigneten Speicherort verweist
      * Speichern Sie Ihre Änderungen
      * Der oben erwähnte Speicherort ist Ihr überwachter Ordner.

Legen Sie die Ordner &quot;SamplePdfFile&quot;und &quot;SampleXdpFile&quot;im Ordner &quot;input&quot;des überwachten Ordners ab. Bei erfolgreicher Verarbeitung der Dateien werden die Ergebnisse im Ergebnisordner Ihres überwachten Ordners abgelegt.


>[!NOTE]
>
>Wenn das mit dem überwachten Ordner verknüpfte Skript mehr als eine Datei benötigt, müssen Sie einen Ordner erstellen, alle erforderlichen Dateien im Ordner ablegen und den Ordner im Eingabeordner des überwachten Ordners ablegen.
>
>Wenn das mit dem überwachten Ordner verknüpfte Skript nur eine Eingabedatei benötigt, können Sie die Datei direkt im Eingabeordner des überwachten Ordners ablegen

