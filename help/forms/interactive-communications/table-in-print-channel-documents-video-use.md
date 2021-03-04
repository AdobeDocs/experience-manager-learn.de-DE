---
title: Verwenden der Tabellenkomponente im AEM Forms Print Kanal Dokument
seo-title: Verwenden der Tabellenkomponente im AEM Forms Print Kanal Dokument
description: Im folgenden Video werden die Schritte erläutert, die erforderlich sind, um die Tabellenkomponente in der interaktiven Kommunikation für Dokumente von Kanälen zu verwenden.
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---


# Verwenden der Tabellenkomponente im AEM Forms Print Kanal-Dokument {#using-table-component-in-aem-forms-print-channel-document}

Im folgenden Video werden die Schritte erläutert, die erforderlich sind, um die Tabellenkomponente in der interaktiven Kommunikation für Dokumente von Kanälen zu verwenden.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Tabellen werden verwendet, um Daten tabellarisch anzuzeigen. Die Zeilen in der Tabelle müssen je nach den von der Datenquelle zurückgegebenen Daten wachsen oder schrumpfen. Um eine Tabelle im Print Kanal-Dokument zu verwenden, müssen wir eine Layoutdatei (xdp-Datei) mit AEM Forms Designer erstellen. In dieser Layoutdatei fügen wir die Tabelle mit der erforderlichen Spaltenanzahl hinzu. Stellen Sie sicher, dass der Objekttyp für das Spaltenfeld je nach Bedarf entweder TextField oder Numeric Field ist. Für jede Spalte stellen die Felder sicher, dass die Datenbindung auf &quot;Name verwenden&quot;eingestellt ist.

>[!NOTE]
>
>Um die Tabelle dynamisch zu gestalten, stellen Sie sicher, dass Sie die Zeile als wiederholend markiert haben.

**Testen Sie es auf Ihrem eigenen Server**

* [Laden Sie die Asset-Datei auf Ihre Festplatte herunter und entpacken Sie sie.](assets/usingtablesinprintchannel.zip)

* Importieren Sie die beiden ZIP-Dateien mit dem Package Manager in AEM

* Folgende Elemente sind in den mit diesem Artikel verknüpften Elementen enthalten:

   * Layout-Fragment

   * Formulardatenmodell

   * Interaktives Kommunikations-Dokument
   * sampleretirementaccountdata.json

* Öffnen Sie das Dokument &quot;Interaktive Kommunikation&quot;im Bearbeitungsmodus [a1/>.](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)

* hinzufügen Sie das Layout-Fragment &quot;TableDemo&quot;in den Beitragsabschnitt.
* Binden Sie die Tabellenzellen an die entsprechenden Formulardatenmodellelemente, wie im Video gezeigt.

* Vorschau Interactive Communication Dokument mit der JSON-Musterdatendatei

