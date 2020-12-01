---
title: Bestimmen der Ordnerstruktur und Dateibenennungskonvention
description: Die Dateibenennung ist möglicherweise die wichtigste Entscheidung, die Sie bei der Implementierung von Dynamic Media Classic treffen werden. Die Ordnerstruktur ist ebenfalls wichtig. Erfahren Sie, warum es so wichtig und möglich ist, Ansätze für Ihre Ordnerstruktur und Dateinamen zu verwenden.
sub-product: dynamic-media
feature: null
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1211'
ht-degree: 0%

---


# Bestimmen der Ordnerstruktur und Dateibenennungskonvention {#folder-structure-filenaming}

Bevor Sie mit dem Hochladen des gesamten Beginns beginnen, sollten Sie die Ordnerstruktur und insbesondere Ihre Dateibenennungsregeln beachten. Es wird Ihnen wahrscheinlich Zeit sparen und Aufgaben später wiederholen müssen. Es ist am besten, diese Entscheidungen über alle Gruppen hinweg zu koordinieren.

## Ordnerhierarchie und Dateibenennungsregel

Die Dateibenennung ist im Allgemeinen die wichtigste Entscheidung, die Sie bezüglich der Implementierung von Dynamic Media Classic treffen. Um zu verstehen, warum es wichtig ist, lassen Sie uns zunächst über Ihre Ordnerstruktur sprechen.

### Ordnerhierarchie

Die Ordnerhierarchie ist nur für Sie und Ihre Firma aus organisatorischen Gründen wichtig — Ihre URLs für dynamische Medien-Classic verweisen nur auf den Asset-Namen, nicht auf den Ordner oder Pfad. Unabhängig davon, wo Sie eine Datei hochladen, ist die URL identisch. Das unterscheidet sich ganz von der Art, wie die meisten Menschen ihre Bilder und Inhalte für das Web organisieren, aber mit Dynamic Media Classic macht es keinen Unterschied.

Ein weiterer wichtiger Aspekt ist die Anzahl der Assets oder Ordner, die in jedem Ordner gespeichert werden sollen. Wenn viele Assets in einem Ordner gespeichert sind, wird die Leistung bei der Anzeige von Assets in Dynamic Media Classic beeinträchtigt. Speichern Sie nicht Tausende von Assets in einem Ordner. Entwickeln Sie stattdessen eine hierarchische Struktur mit weniger als 500 Assets oder Ordnern in einer bestimmten Hierarchieabteilung. Dies ist keine strikte Anforderung, trägt jedoch dazu bei, dass akzeptable Reaktionszeiten beim Anzeigen oder Suchen von Assets beibehalten werden. Die Empfehlung besteht darin, Hierarchien zu erstellen, die breit und flach sind, anstatt schmal und tief.

Die einfachste Möglichkeit zum Erstellen von Ordnern besteht darin, die gesamte Ordnerstruktur per FTP hochzuladen und die Option **Unterordner einschließen** zu aktivieren. Diese Option bewirkt, dass Dynamic Media Classic die Ordnerstruktur auf der FTP-Site in Dynamic Media Classic neu erstellt.

Wir möchten, dass Sie Ihre Ordnerstruktur berücksichtigen, bevor Sie den Beginn haben, alle Ihre Dateien hochzuladen, da es wesentlich einfacher ist, Ihre Dateien und Ordner lokal auf Ihrem Computer zu organisieren und zu verwalten, als dies in Dynamic Media Classic der Fall ist. Sie können beispielsweise nur Dateien, jedoch nicht ganze Ordner, innerhalb von Dynamic Media Classic ziehen und ablegen.

### Ordnerstrategien

Berücksichtigen Sie bei Ihrer Ordnerstrategie, was für Ihr Unternehmen sinnvoll ist. Im Folgenden finden Sie einige allgemeine Szenarien für die Ordnernennung:

- Spiegeln Sie die Website oder die Produktaufschlüsselung. Wenn Sie beispielsweise Bekleidung verkaufen, haben Sie möglicherweise Ordner für Männer, Frauen und Zubehör sowie Unterordner für Hemden und Schuhe.
- SKU oder Produkt-ID-basierte Strategie. Bei Einzelhändlern mit Tausenden von Artikeln könnte es sinnvoll sein, SKU-Nummern oder Produkt-IDs als Ordnernamen zu verwenden.
- Markenstrategie. Hersteller mit mehreren Marken können beispielsweise ihre Markennamen als Ordner der obersten Ebene auswählen.

## Dateibenennungsregel

Wie Sie sich entscheiden, Ihre Dateien zu benennen, ist vielleicht die wichtigste frühe Entscheidung, die Sie in Bezug auf Dynamic Media Classic treffen werden. Dies liegt daran, dass alle Assets in Dynamic Media Classic eindeutige Namen haben müssen, unabhängig davon, wo sie im Konto gespeichert werden.

Alle URLs und Transaktionen in Dynamic Media Classic werden von einer Asset-ID gesteuert, der eindeutigen Kennung eines Assets in der Datenbank. Wenn Sie eine Datei hochladen, wird die Asset-ID erstellt, indem Sie den Dateinamen verwenden und die Erweiterung entfernen. Beispiel: _896649.jpg_ erhält Asset _ID 896649_.

Regeln für Asset-IDs:

- In Dynamic Media Classic dürfen zwei Assets unabhängig vom Ordner, in dem sich die Assets befinden, nicht denselben Namen haben.
- Bei Namen muss die Groß-/Kleinschreibung beachtet werden. Beispielsweise würden &quot;Chair.jpg&quot;, &quot;chair.jpg&quot;und &quot;CHAIR.jpg&quot;drei verschiedene Asset-IDs erstellen.
- Als bewährtes Verfahren sollten Asset-IDs keine Leerzeichen oder Symbole enthalten. Die Verwendung von Leerzeichen und Symbolen erschwert die Implementierung, da Sie diese Zeichen URL-kodieren müssen. Beispiel: Ein Leerzeichen &quot; wird &quot;%20&quot;.

Ihre Benennungskonvention ist im Wesentlichen die Art und Weise, wie Sie mit Dynamic Media Classic integrieren. Sie integrieren Ihre Back-Office-Systeme normalerweise nicht in Dynamic Media Classic, da es sich um ein geschlossenes System handelt. Es ist ein passiver Partner, der auf Anweisungen in Form von URLs wartet.

Die meisten Benutzer basieren bei ihren Benennungsregeln auf ihrer internen SKU oder Produkt-IDs, sodass beim Aufruf einer Webseite mit Informationen zu dieser SKU automatisch nach einem Bild mit einem ähnlichen Namen gesucht werden kann. Wenn keine Verbindung zwischen dem Dateinamen und der SKU oder ID besteht, muss Ihr Backoffice-System die Dateinamen manuell verfolgen und eine Person muss diese Verbindungen unterhalten — — Kurz gesagt, eine Menge Arbeit für die IT- und Inhaltsteams.

### Dateibenennungsstrategien

Ihre Benennungsstrategie sollte flexibel sein, damit Sie nach dem Start keine Umbenennung mehr vornehmen müssen. Hier sind einige typische Benennungsstrategien:

**Keine alternativen Bilder.** In diesem Szenario haben Sie nur ein Bild pro Produkt und keine alternativen oder farbigen Ansichten. Sie würden jedes Bild streng nach seiner eindeutigen SKU oder Produkt-ID benennen. Beim Laden der Seite ruft die Seitenvorlage die Asset-ID mit derselben SKU-Nummer auf.

| SKU/PID | Dateiname | Asset-ID |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU 123 | SKU123.png | SKU 123 |

Dies ist ein sehr einfaches System, und gut, wenn Sie bescheidene Bedürfnisse haben. Es ist jedoch nicht sehr flexibel. Nur weil Sie heute keine alternativen Bilder haben, heißt das nicht, dass Sie diese Bilder morgen nicht haben werden. Das nächste Szenario Angebot mehr Flexibilität.

**Verwenden Sie das Bild, alternative Ansichten, farbige Versionen, Muster.** Diese Strategie ermöglicht alternative Ansichten und/oder farbige Ansichten, sofern vorhanden. Anstatt das Bild nur nach der SKU zu benennen, fügen Sie einen Modifikator wie &quot;_1&quot;und &quot;_2&quot;für alternative Ansichten sowie einen Farbcode von &quot;_RED&quot;oder &quot;_BLU&quot;für farbige Ansichten hinzu. Wenn Sie sowohl Farbbilder als auch alternative Ansichten für dasselbe Produkt haben, würden Sie vielleicht &quot;_RED_1&quot;und &quot;_RED_2&quot;für die erste und zweite rote Ansicht hinzufügen. Farbfelder werden mit der SKU, dem Farbcode und der Erweiterung &quot;_SW&quot;benannt.

| SKU/PID | Kategorie | Dateiname | Asset-ID |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Alt-Ansichten | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Farbige Ansichten | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Muster | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Bildsatz oder Musterset |  | AA123 oder AA123_SET | — |

Beim Umgang mit festgelegten Sammlungen, z. B. Bildsätzen und Mustersets, muss das Set selbst auch einen eindeutigen Namen haben. In diesem Fall könnte das Set die Basis-SKU als Name oder die SKU mit der Erweiterung &quot;_SET&quot;erhalten.

### Namenskonvention und Automatisierung

Ein letztes Wort zur Bedeutung der Benennungskonvention. Wenn Sie Sätze verwenden möchten (z. B. Bildsätze oder Mustersets), können Sie die Erstellung mithilfe einer vorhersagbaren Benennungskonvention automatisieren. Jede skriptgesteuerte Methode, wie z. B. eine Stapelsatzvorgabe, über die Sie in einem separaten Abschnitt dieses Lernprogramms Bescheid wissen, kann von einer Benennungsregel abgeschnitten werden.

Die alternative Methode besteht darin, Ihre Sets manuell zu erstellen. Beim manuellen Erstellen von Bildsätzen für 200 Bilder ist es vielleicht nicht besonders wichtig, wenn Sie mehr als 100.000 Bilder haben. Dies ist der Zeitpunkt, an dem die Automatisierung der Set-Erstellung entscheidend wird.
