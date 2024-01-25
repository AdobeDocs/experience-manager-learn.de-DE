---
title: Bestimmen Ihrer Ordnerstruktur und Dateinamenskonvention
description: Das Benennen der Dateien ist möglicherweise die wichtigste Entscheidung, die Sie bei der Implementierung von Dynamic Media Classic treffen werden. Die Ordnerstruktur ist ebenfalls wichtig. Erfahren Sie, warum dies so wichtig ist und welche Ansätze Sie für Ihre Ordnerstruktur und Dateinamen wählen können.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
duration: 275
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 100%

---

# Bestimmen Ihrer Ordnerstruktur und Dateinamenskonvention {#folder-structure-filenaming}

Bevor Sie mit dem Hochladen all Ihrer Inhalte beginnen, sollten Sie sich Gedanken über die von Ihnen verwendete Ordnerstruktur und insbesondere über Ihre Dateinamenskonvention machen. Dies erspart Ihnen sehr wahrscheinlich Zeit und den Umstand, Aufgaben später wiederholen zu müssen. Es ist am besten, diese Entscheidungen über alle Gruppen hinweg zu koordinieren.

## Ordnerhierarchie und Dateinamenskonvention

Das Benennen der Dateien ist im Allgemeinen die wichtigste Entscheidung, die Sie bezüglich der Implementierung von Dynamic Media Classic treffen. Um jedoch zu verstehen, warum es so wichtig ist, sprechen wir zunächst über Ihre Ordnerstruktur.

### Ordnerhierarchie

Die Ordnerhierarchie ist für Sie und Ihr Unternehmen nur aus organisatorischen Gründen wichtig – Ihre Dynamic Media Classic-URLs verweisen schließlich nur auf den Asset-Namen, nicht auf den Ordner oder Pfad. Unabhängig davon, wo Sie eine Datei hochladen, ist die URL identisch. Dies unterscheidet sich stark von der Art und Weise, wie die meisten Menschen ihre Bilder und Inhalte für das Web organisieren, aber mit Dynamic Media Classic macht dies keinen Unterschied.

Eine weitere wichtige Überlegung ist die Anzahl der Assets oder Ordner, die in jedem Ordner gespeichert werden sollen. Wenn viele Assets in einem Ordner gespeichert sind, verschlechtert sich die Leistung bei der Anzeige von Assets in Dynamic Media Classic. Speichern Sie nicht Tausende von Assets in einem Ordner. Entwickeln Sie stattdessen eine Organisationshierarchie mit weniger als etwa 500 Assets oder Ordnern innerhalb eines bestimmten Zweigs Ihrer Hierarchie. Dies ist keine strenge Anforderung, aber es hilft, akzeptable Antwortzeiten beim Anzeigen oder Suchen von Assets zu gewährleisten. Es wird empfohlen, eher breite und flache als schmale und tiefe Hierarchien zu erstellen.

Die einfachste Möglichkeit, Ordner zu erstellen, besteht darin, die gesamte Ordnerstruktur per FTP hochzuladen und die Option **Unterordner einschließen** zu aktivieren. Diese Option bewirkt, dass Dynamic Media Classic die Ordnerstruktur auf der FTP-Site in Dynamic Media Classic neu erstellt.

Wir möchten, dass Sie Ihre Ordnerstruktur berücksichtigen, bevor Sie mit dem Hochladen aller Dateien beginnen, da es wesentlich einfacher ist, Ihre Dateien und Ordner lokal auf Ihrem Computer zu organisieren und zu verwalten, als es innerhalb von Dynamic Media Classic der Fall ist. So können Sie beispielsweise innerhalb von Dynamic Media Classic nur Dateien per Drag-and-Drop verschieben, nicht aber ganze Ordner.

### Ordnerstrategien

Beachten Sie bei Ihrer Ordnerstrategie, was für Ihre Organisation sinnvoll ist. Im Folgenden finden Sie einige gängige Szenarien für die Benennung von Ordnern:

- Mirror-Website oder Produktunterteilung. Wenn Sie z. B. Kleidung verkaufen, könnten Sie Ordner für Männer, Frauen und Accessoires sowie Unterordner für Hemden und Schuhe anlegen.
- SKU- oder Produkt-ID-basierte Strategie. Bei Einzelhändlern mit Tausenden von Artikeln kann es beispielsweise sinnvoll sein, SKU-Nummern oder Produkt-IDs als Ordnernamen zu verwenden.
- Markenstrategie. Hersteller mit mehreren Marken können beispielsweise ihre Markennamen als Ordner der obersten Ebene auswählen.

## Dateinamenskonvention

Wie Sie Ihre Dateien benennen, ist vielleicht die wichtigste der frühen Entscheidungen, die Sie in Bezug auf Dynamic Media Classic treffen werden. Dies liegt daran, dass alle Assets in Dynamic Media Classic eindeutige Namen haben müssen, unabhängig davon, wo sie im Konto gespeichert sind.

Alle URLs und Transaktionen in Dynamic Media Classic werden durch eine Asset-ID gesteuert, die die eindeutige Kennung eines Assets in der Datenbank darstellt. Beim Hochladen einer Datei wird die Asset-ID erstellt, indem der Dateiname genommen und die Erweiterung entfernt wird. Beispielsweise erhält die Datei _896649.jpg_ die Asset-ID _896649_.

Regeln für Asset-IDs:

- In Dynamic Media Classic dürfen zwei Assets nicht denselben Namen haben, unabhängig davon, in welchem Ordner sich die Assets befinden.
- Bei Namen wird zwischen Groß- und Kleinschreibung unterschieden. Beispielsweise werden für „Stuhl.jpg“, „stuhl.jpg“ und „STUHL.jpg“ drei verschiedene Asset-IDs erstellt.
- Asset-IDs sollten, so die Best Practice, keine Leerzeichen oder Symbole enthalten. Leerzeichen und Symbole erschweren die Implementierung, da Sie diese Zeichen URL-kodieren müssen. Ein Leerzeichen wird z. B. zu „%20“.

Ihre Namenskonvention entspricht im Wesentlichen der Art und Weise, wie die Integration in Dynamic Media Classic durchgeführt wird. Normalerweise integrieren Sie Ihre Backoffice-Systeme nicht in Dynamic Media Classic, da es sich um ein geschlossenes System handelt. Es ist ein passiver Partner, der auf Anweisungen in Form von URLs wartet.

Die meisten Benutzenden nutzen die interne SKU oder Produkt-ID als Basis für ihre Namenskonvention, sodass beim Aufruf einer Web-Seite mit Informationen zu dieser SKU die Seite automatisch nach einem Bild mit einem ähnlichen Namen suchen kann. Wenn es keine Verbindung zwischen dem Dateinamen und der SKU oder ID gibt, muss Ihr Backoffice-System jeden Dateinamen manuell verfolgen und eine Person muss die entsprechenden Zuordnungen verwalten. Kurz gesagt, das bedeutet jede Menge Arbeit für die IT- und Inhalts-Teams.

### Dateibenennungsstrategien

Ihre Benennungsstrategie sollte für zukünftige Erweiterungen flexibel sein, damit Sie nach dem Launch nichts mehr umbenennen müssen. Im Folgenden finden Sie einige typische Benennungsstrategien:

**Keine alternativen Bilder.** In diesem Szenario gibt es nur ein Bild pro Produkt und keine alternativen oder farbigen Ansichten. Jedes Bild wird streng nach seiner eindeutigen SKU oder Produkt-ID benannt. Beim Laden der Seite ruft die Seitenvorlage die Asset-ID mit derselben SKU-Nummer auf.

| SKU/PID | Dateiname | Asset-ID |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Dies ist ein sehr einfaches System, das bei geringen Anforderungen gut funktioniert. Es ist aber nicht sonderlich flexibel. Auch wenn heute keine alternativen Bilder vorhanden sind, kann das morgen schon ganz anders aussehen. Das nächste Szenario bietet mehr Flexibilität.

**Das Bild, alternative Ansichten, farbige Versionen und Farbfelder verwenden.** Diese Strategie lässt alternative und/oder farbige Ansichten zu, sofern vorhanden. Anstatt das Bild nur nach der SKU zu benennen, fügen Sie einen Modifikator wie „_1“ und „_2“ für alternative Ansichten und einen Farb-Code wie „_ROT“ oder „_BLAU“ für farbige Ansichten hinzu. Wenn Sie sowohl farbige Bilder als auch alternative Ansichten für ein und dasselbe Produkt haben, können Sie etwa „_ROT_1“ und „_ROT_2“ für die erste und zweite Ansicht in Rot hinzufügen. Farbfelder werden mit der SKU, dem Farb-Code und der Erweiterung „_SW“ (für „Swatch“, Farbfeld) benannt.

| SKU/PID | Kategorie | Dateiname | Asset-ID |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Alternative Ansichten | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|         | Farbige Ansichten | AA123_BLAU.tif AA123_ROT.tif AA123_BRAUN.tif | AA123_BLAU AA123_RED AA123_BRAUN |
|         | Farbfelder | AA123_BLAU_SW.tif | AA123_BLAU_SW |
|         | Bild- oder Farbfeld-Set |                                             | AA123 oder AA123_SET | -- |

Bei der Arbeit mit Set-Sammlungen, z. B. Bild- und Farbfeld-Sets, muss das Set selbst auch einen eindeutigen Namen haben. In diesem Fall könnte das Set die Basis-SKU oder die SKU mit der Erweiterung „_SET“ als Namen erhalten.

### Namenskonvention und Automatisierung

Ein letztes Wort zur Bedeutung von Namenskonventionen. Wenn Sie Sets verwenden möchten (z. B. Bild- oder Farbfeld-Sets), können Sie diese mit einer vorhersagbaren Namenskonvention automatisch erstellen lassen. Jede skriptgesteuerte Methode, wie z. B. eine Batch-Set-Vorgabe, über die Sie in einem separaten Abschnitt dieses Tutorials mehr erfahren werden, kann über eine Namenskonvention gestartet werden.

Die alternative Methode besteht darin, Sets manuell zu erstellen. Die manuelle Set-Erstellung für 200 Bilder ist vielleicht keine große Aufgabe, aber bei mehr als 100.000 Bildern? Dies ist der Punkt, an dem die Automatisierung der Set-Erstellung eine entscheidende Rolle spielt.
