---
title: Verwenden der Tabellenkomponente in einem Druckkanaldokument von AEM Forms
description: Das folgende Video führt Sie durch die Schritte, die zur Verwendung der Tabellenkomponente in der interaktiven Kommunikation für Druckkanaldokumente erforderlich sind.
feature: Interactive Communication
doc-type: technical video
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 277
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 100%

---

# Verwenden der Tabellenkomponente in einem Druckkanaldokument von AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

Das folgende Video führt Sie durch die Schritte, die zur Verwendung der Tabellenkomponente in der interaktiven Kommunikation für Druckkanaldokumente erforderlich sind.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

Tabellen werden verwendet, um Daten tabellarisch anzuzeigen. Die Zeilen der Tabelle müssen entsprechend den von der Datenquelle zurückgegebenen Daten vergrößert oder verkleinert werden. Um eine Tabelle im Druckkanaldokument zu verwenden, müssen wir eine Layout-Datei (XDP-Datei) mit AEM Forms Designer erstellen. In dieser Layout-Datei fügen wir die Tabelle mit der erforderlichen Anzahl von Spalten hinzu. Stellen Sie sicher, dass der Objekttyp für das Spaltenfeld je nach Ihren Anforderungen entweder „Textfeld“ oder „Numerisches Feld“ lautet. Achten Sie bei den Feldern der einzelnen Spalten darauf, dass die Datenbindung auf „Name verwenden“ eingestellt ist.

>[!NOTE]
>
>Um die Tabelle dynamisch zu gestalten, müssen Sie die Zeile als sich wiederholend markieren.

**Probieren Sie es auf Ihrem eigenen Server aus:**

* [Laden Sie die Asset-Datei auf Ihre Festplatte herunter und entpacken Sie diese.](assets/usingtablesinprintchannel.zip)

* Importieren Sie die beiden ZIP-Dateien mit Package Manager in AEM.

* In den mit diesem Artikel verbundenen Assets ist Folgendes enthalten:

   * Layout-Fragment

   * Formulardatenmodell

   * Dokument zur interaktiven Kommunikation
   * sampleretirementaccountdata.json

* Öffnen Sie das Dokument zur interaktiven Kommunikation im [Bearbeitungsmodus](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Fügen Sie das Layout-Fragment „TableDemo“ zum Abschnitt mit den Beiträgen hinzu.
* Binden Sie die Tabellenzellen an die entsprechenden Formulardatenmodell-Elemente, wie im Video gezeigt

* Zeigen Sie das Dokument zur interaktiven Kommunikation mit der Ihnen bereitgestellten beispielhaften JSON-Datendatei in einer Vorschau an.
