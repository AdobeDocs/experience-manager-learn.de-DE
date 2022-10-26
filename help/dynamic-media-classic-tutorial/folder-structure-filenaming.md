---
title: Bestimmen der Ordnerstruktur und der Dateinamenkonvention
description: Die Dateibenennung ist möglicherweise die wichtigste Entscheidung, die Sie bei der Implementierung von Dynamic Media Classic treffen werden. Die Ordnerstruktur ist ebenfalls wichtig. Erfahren Sie, warum es so wichtig und möglich ist, Ansätze für Ihre Ordnerstruktur und Dateinamen zu finden.
feature: Dynamic Media Classic
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 0%

---

# Bestimmen der Ordnerstruktur und der Dateinamenkonvention {#folder-structure-filenaming}

Bevor Sie mit dem Hochladen des gesamten Inhalts beginnen, sollten Sie die Ordnerstruktur, die Sie verwenden, und insbesondere Ihre Dateibenennungskonvention berücksichtigen. Dadurch sparen Sie wahrscheinlich Zeit und müssen Aufgaben später erneut ausführen. Am besten koordinieren Sie diese Entscheidungen über alle Gruppen hinweg.

## Ordnerhierarchie und Dateinamenkonvention

Die Dateibenennung ist im Allgemeinen die wichtigste Entscheidung, die Sie bezüglich der Implementierung von Dynamic Media Classic treffen. Um jedoch zu verstehen, warum es wichtig ist, sprechen wir zunächst über Ihre Ordnerstruktur.

### Ordnerhierarchie

Die Ordnerhierarchie ist nur für organisatorische Zwecke wichtig - Ihre Dynamic Media Classic-URLs verweisen nur auf den Asset-Namen, nicht auf den Ordner oder Pfad. Unabhängig davon, wo Sie eine Datei hochladen, ist die URL identisch. Dies unterscheidet sich stark von der Art und Weise, wie die meisten Benutzer ihre Bilder und Inhalte für das Web organisieren, aber mit Dynamic Media Classic macht es keinen Unterschied.

Eine weitere wichtige Überlegung ist die Anzahl der Assets oder Ordner, die in jedem Ordner gespeichert werden sollen. Wenn viele Assets in einem Ordner gespeichert sind, verschlechtert sich die Leistung bei der Anzeige von Assets in Dynamic Media Classic. Speichern Sie nicht Tausende von Assets in einem Ordner. Entwickeln Sie stattdessen eine Organisationshierarchie mit weniger als 500 Assets oder Ordnern in einem bestimmten Zweig Ihrer Hierarchie. Dies ist keine strenge Anforderung, aber es hilft, akzeptable Antwortzeiten beim Anzeigen oder Suchen von Assets beizubehalten. Es wird empfohlen, Hierarchien zu erstellen, die breit und flach sind, nicht schmal und tief.

Die einfachste Möglichkeit, Ordner zu erstellen, besteht darin, die gesamte Ordnerstruktur per FTP hochzuladen und die Option **Unterordner einschließen**. Diese Option bewirkt, dass Dynamic Media Classic die Ordnerstruktur auf der FTP-Site in Dynamic Media Classic neu erstellt.

Wir möchten, dass Sie Ihre Ordnerstruktur berücksichtigen, bevor Sie mit dem Hochladen aller Dateien beginnen, da es wesentlich einfacher ist, Ihre Dateien und Ordner lokal auf Ihrem Computer zu organisieren und zu verwalten, als es in Dynamic Media Classic der Fall ist. Sie können beispielsweise nur Dateien, nicht aber ganze Ordner in Dynamic Media Classic ziehen und ablegen.

### Ordnerstrategien

Beachten Sie bei Ihrer Ordnerstrategie, was für Ihr Unternehmen sinnvoll ist. Im Folgenden finden Sie einige gängige Szenarien für die Ordnerbenennung:

- Mirror-Website oder Produktunterteilung. Wenn Sie z. B. Kleidung verkaufen, können Sie Ordner für Männer, Frauen und Zubehör sowie Unterordner für Hemden und Schuhe haben.
- SKU oder Produkt-ID-basierte Strategie. Bei Einzelhändlern mit Tausenden von Artikeln kann es beispielsweise sinnvoll sein, SKU-Nummern oder Produkt-IDs als Ordnernamen zu verwenden.
- Markenstrategie. Hersteller mit mehreren Marken können beispielsweise ihre Markennamen als Ordner der obersten Ebene auswählen.

## Dateinamenkonvention

Wie Sie Ihre Dateien benennen, ist vielleicht die wichtigste frühe Entscheidung, die Sie in Bezug auf Dynamic Media Classic treffen werden. Dies liegt daran, dass alle Assets in Dynamic Media Classic eindeutige Namen haben müssen, unabhängig davon, wo sie im Konto gespeichert sind.

Alle URLs und Transaktionen in Dynamic Media Classic werden durch eine Asset-ID gesteuert, die die eindeutige Kennung eines Assets in der Datenbank darstellt. Beim Hochladen einer Datei wird die Asset-ID erstellt, indem der Dateiname verwendet und die Erweiterung entfernt wird. Beispiel: _896649.jpg_ get Asset _ID 896649_.

Regeln für Asset-IDs:

- In Dynamic Media Classic dürfen nicht zwei Assets denselben Namen haben, unabhängig davon, in welchem Ordner sich die Assets befinden.
- Bei Namen wird zwischen Groß- und Kleinschreibung unterschieden. Beispielsweise würden Chair.jpg, chair.jpg und CHAIR.jpg drei verschiedene Asset-IDs erstellen.
- Asset-IDs sollten keine Leerzeichen oder Symbole enthalten. Die Verwendung von Leerzeichen und Symbolen erschwert die Implementierung, da Sie diese Zeichen URL-kodieren müssen. Beispielsweise wird ein Leerzeichen &quot;&quot;zu &quot;%20&quot;.

Ihre Namenskonvention ist im Wesentlichen die Art und Weise, wie Sie in Dynamic Media Classic integrieren. Normalerweise integrieren Sie Ihre Back-Office-Systeme nicht in Dynamic Media Classic, da es sich um ein geschlossenes System handelt. Es ist ein passiver Partner, der auf Anweisungen in Form von URLs wartet.

Die meisten Benutzer basieren ihre Benennungskonvention auf ihrer internen SKU oder Produkt-IDs, sodass die Seite automatisch nach einem Bild mit einem ähnlichen Namen suchen kann, wenn eine Webseite mit Informationen zu dieser SKU aufgerufen wird. Wenn es keine Verbindung zwischen dem Dateinamen und der SKU oder ID gibt, muss Ihr Backoffice-System jeden Dateinamen manuell verfolgen und eine Person muss diese Verknüpfungen verwalten - kurz gesagt, eine Menge Arbeit für die IT- und Inhaltsteams.

### Dateibenennungsstrategien

Ihre Benennungsstrategie sollte für eine zukünftige Erweiterung flexibel sein, damit Sie nach dem Start keine Umbenennung mehr vornehmen müssen. Im Folgenden finden Sie einige typische Benennungsstrategien:

**Keine alternativen Bilder.** In diesem Szenario haben Sie nur ein Bild pro Produkt und keine alternativen oder farbigen Ansichten. Sie würden jedes Bild streng nach seiner eindeutigen SKU oder Produkt-ID benennen. Beim Laden der Seite ruft die Seitenvorlage die Asset-ID mit derselben SKU-Nummer auf.

| SKU/PID | Dateiname | Asset-ID |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU 123 | SKU123.png | SKU 123 |

Dies ist ein sehr einfaches System, und gut, wenn Sie bescheidene Bedürfnisse haben. Es ist jedoch nicht sehr flexibel. Nur weil Sie heute keine alternativen Bilder haben, heißt das nicht, dass Sie diese Bilder morgen nicht haben werden. Das nächste Szenario bietet mehr Flexibilität.

**Mithilfe des Bilds können Sie alternative Ansichten, farbige Versionen und Muster verwenden.** Diese Strategie ermöglicht alternative Ansichten und/oder farbige Ansichten, falls vorhanden. Anstatt das Bild nur nach der SKU zu benennen, fügen Sie einen Modifikator wie &quot;_1&quot;und &quot;_2&quot;für alternative Ansichten hinzu und einen Farbcode von &quot;_RED&quot;oder &quot;_BLU&quot;für farbige Ansichten. Wenn Sie sowohl farbige Bilder als auch alternative Ansichten für dasselbe Produkt haben, würden Sie vielleicht &quot;_RED_1&quot;und &quot;_RED_2&quot;für die erste und zweite rote Ansicht hinzufügen. Muster werden mit der SKU, dem Farbcode und der Erweiterung &quot;_SW&quot;benannt.

| SKU/PID | Kategorie | Dateiname | Asset-ID |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Alt-Ansichten | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Farbige Ansichten | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Muster | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Bildset oder Musterset |  | AA123 oder AA123_SET | — |

Bei der Arbeit mit Set-Sammlungen, z. B. Bildsets und Mustersets, muss das Set selbst auch einen eindeutigen Namen haben. In diesem Fall könnte der Satz die Basis-SKU als Namen oder die SKU mit der Erweiterung &quot;_SET&quot;erhalten.

### Benennungskonvention und Automatisierung

Ein letztes Wort zur Bedeutung der Namenskonvention. Wenn Sie Sets verwenden möchten (z. B. Bildsets oder Mustersets), können Sie mit einer vorhersehbaren Namenskonvention deren Erstellung automatisieren. Jede skriptfähige Methode, wie z. B. eine Stapelsatzvorgabe, über die Sie in einem separaten Abschnitt dieses Tutorials erfahren, kann von einer Benennungskonvention abgelöst werden.

Die alternative Methode besteht darin, die Sets manuell zu erstellen. Beim manuellen Erstellen von Bildsets für 200 Bilder ist es vielleicht nicht besonders wichtig, sich vorzustellen, dass Sie mehr als 100.000 Bilder haben. Dies ist der Punkt, an dem die Automatisierung der Set-Erstellung entscheidend wird.
