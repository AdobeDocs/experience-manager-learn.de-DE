---
title: Verwenden der Tabellenkomponente in einem Druckkanaldokument von AEM Forms
seo-title: Using Table Component in AEM Forms Print Channel Document
description: Das folgende Video führt Sie durch die Schritte, die zur Verwendung der Tabellenkomponente in der interaktiven Kommunikation für Druckkanaldokumente erforderlich sind.
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '267'
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
