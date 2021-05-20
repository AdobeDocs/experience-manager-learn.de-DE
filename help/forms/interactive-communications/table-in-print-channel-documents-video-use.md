---
title: Verwenden der Tabellenkomponente des AEM Forms-Druckkanaldokuments
seo-title: Verwenden der Tabellenkomponente des AEM Forms-Druckkanaldokuments
description: Das folgende Video führt Sie durch die Schritte, die zur Verwendung der Tabellenkomponente in der interaktiven Kommunikation für Druckkanaldokumente erforderlich sind.
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 1%

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

* Öffnen Sie das Dokument zur interaktiven Kommunikation im Bearbeitungsmodus [a1/>.](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)

* Fügen Sie das Layout-Fragment TableDemo zum Beitragsabschnitt hinzu.
* Binden Sie die Tabellenzellen an die entsprechenden Formulardatenmodellelemente, wie im Video gezeigt

* Anzeigen der Vorschau des interaktiven Kommunikationsdokuments mit der JSON-Musterdatendatei, die Ihnen bereitgestellt wird

