---
title: Verwenden überwachter Ordner in AEM Forms
description: Konfigurieren und Verwenden überwachter Ordner in AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
duration: 107
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 100%

---

# Verwenden überwachter Ordner in AEM Forms{#using-watched-folders-in-aem-forms}

Ein Administrator kann einen Netzwerkordner konfigurieren, der als überwachter Ordner bezeichnet wird, sodass ein vorkonfigurierter Workflow-, Dienst- oder Skriptvorgang zur Verarbeitung der Datei aufgerufen wird, wenn ein Benutzer eine Datei (z. B. eine PDF-Datei) in diesem überwachten Ordner ablegt. Nachdem der Dienst den vorgesehenen Vorgang ausgeführt hat, wird die Ergebnisdatei in einem angegebenen Ausgabeordner gespeichert. Weitere Informationen zu Workflow, Dienst und Skript finden Sie unter den entsprechenden Themen.

Weitere Informationen zum Erstellen eines überwachten Ordners finden Sie [hier](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html).

Überwachte Ordner werden verwendet, um Dokumente im Batch-Modus zu generieren. Mithilfe des Mechanismus für überwachte Ordner können Sie Dokumente zur interaktiven Kommunikation für den Druckkanal generieren oder den Output-Dienst nutzen, um Daten mit der Vorlage zusammenzuführen.

Dieser Artikel widmet sich der Zusammenführung von Daten mit einer Vorlage mithilfe des Output-Dienstes über den Mechanismus für überwachte Ordner.

Der Output-Dienst ist als OSGi-Dienst Teil der AEM-Dokumentendienste. Der Ausgabedienst unterstützt verschiedene Ausgabeformate und Ausgabedesignfunktionen von AEM Forms Designer. Mit dem Output-Dienst können Sie XFA-Vorlagen und XML-Daten in verschiedene Druckformate konvertieren.

Weitere Informationen zum Output-Dienst finden Sie [hier](https://helpx.adobe.com/de/aem-forms/6/output-service.html).
Gehen Sie wie folgt vor, um überwachte Ordner auf Ihrem System einzurichten:
* [Laden Sie die ZIP-Datei herunter und extrahieren Sie ihren Inhalt](assets/outputservicewatchedfolderkt.zip). Diese ZIP-Datei enthält ein Paket zum Erstellen eines überwachten Ordners und Beispieldateien zum Testen des Output-Dienstes mithilfe des Mechanismus für überwachte Ordner.
   * Windows-System

      * Importieren Sie die Datei „outputServiceWatchedfolder.zip“ mit Package Manager in AEM.
      * Dadurch wird ein überwachter Ordner namens “outputServiceWatchedfolder“ auf Ihrem Laufwerk „C“ erstellt.
   * Nicht-Windows-System
      * [Öffnen Sie die Konfigurationseinstellung des überwachten Ordners.](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Legen Sie die Ordnerpfad-Eigenschaft des Knotens für den Output-Dienst so fest, dass auf einen geeigneten Speicherort verwiesen wird.
      * Speichern Sie Ihre Änderungen
      * Der oben genannte Speicherort ist Ihr überwachter Ordner.

Legen Sie die Ordner „SamplePdfFile“ und „SampleXdpFile“ im Eingabeordner des überwachten Ordners ab. Bei erfolgreicher Verarbeitung der Dateien werden die Ergebnisse im Ergebnisordner Ihres überwachten Ordners abgelegt.


>[!NOTE]
>
>Wenn das mit dem überwachten Ordner verknüpfte Skript mehr als eine Datei benötigt, müssen Sie einen Ordner erstellen, alle erforderlichen Dateien in dem Ordner ablegen und den Ordner im Eingabeordner Ihres überwachten Ordners ablegen.
>
>Wenn das mit dem überwachten Ordner verknüpfte Skript nur eine einzige Eingabedatei benötigt, können Sie die Datei direkt im Eingabeordner Ihres überwachten Ordners ablegen.
