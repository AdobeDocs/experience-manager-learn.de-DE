---
title: Verwenden der Tabellenkomponente des AEM Forms-Druckkanaldokuments
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
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# Verwenden der Tabellenkomponente des AEM Forms-Druckkanaldokuments {#using-table-component-in-aem-forms-print-channel-document}

Das folgende Video führt Sie durch die Schritte, die zur Verwendung der Tabellenkomponente in der interaktiven Kommunikation für Druckkanaldokumente erforderlich sind.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Tabellen werden verwendet, um Daten tabellarisch anzuzeigen. Die Zeilen in der Tabelle müssen abhängig von den von der Datenquelle zurückgegebenen Daten vergrößert oder verkleinert werden. Um eine Tabelle im Druckkanaldokument zu verwenden, müssen wir eine Layout-Datei (XDP-Datei) mit AEM Forms Designer erstellen. In dieser Layout-Datei fügen wir die Tabelle mit der erforderlichen Anzahl von Spalten hinzu. Stellen Sie sicher, dass der Objekttyp für das Spaltenfeld je nach Ihren Anforderungen entweder TextField oder Numeric Field ist. Bei Feldern in den einzelnen Spalten stellen Sie sicher, dass die Datenbindung auf &quot;Name verwenden&quot;eingestellt ist.

>[!NOTE]
>
>Um die Tabelle dynamisch zu gestalten, müssen Sie sicherstellen, dass Sie die Zeile als wiederholend markiert haben.

**Probieren Sie es auf Ihrem eigenen Server aus**

* [Herunterladen und Entpacken der Asset-Datei auf Ihre Festplatte](assets/usingtablesinprintchannel.zip)

* Importieren Sie die beiden ZIP-Dateien mit Package Manager in AEM

* Folgende Elemente sind in den mit diesem Artikel verknüpften Assets enthalten:

   * Layout-Fragment

   * Formulardatenmodell

   * Interaktive Kommunikationsdokumente
   * sampleretirementaccountdata.json

* Öffnen Sie das Dokument zur interaktiven Kommunikation in [Bearbeitungsmodus](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Fügen Sie das Layout-Fragment TableDemo zum Beitragsabschnitt hinzu.
* Binden Sie die Tabellenzellen an die entsprechenden Formulardatenmodellelemente, wie im Video gezeigt

* Anzeigen der Vorschau des interaktiven Kommunikationsdokuments mit der JSON-Musterdatendatei, die Ihnen bereitgestellt wird
