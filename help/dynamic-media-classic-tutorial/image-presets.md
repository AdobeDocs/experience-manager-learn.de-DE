---
title: Bildvorgaben
description: Bildvorgaben in Dynamic Media Classic enthalten alle Einstellungen, die zum Erstellen eines Bildes in einer bestimmten Größe, einem bestimmten Format, einer bestimmten Qualität und einer bestimmten Scharfzeichnung erforderlich sind. Bildvorgaben sind eine Schlüsselkomponente der dynamischen Größenanpassung. Wenn Sie sich eine URL in Dynamic Media Classic ansehen, können Sie leicht erkennen, ob eine Bildvorgabe verwendet wird. Erfahren Sie mehr über Bildvorgaben und darüber, warum sie so nützlich sind und wie man sie erstellt.
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 157
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '648'
ht-degree: 100%

---

# Bildvorgaben {#image-presets}

Eine Bildvorgabe ist im Wesentlichen ein Rezept, das alle Einstellungen enthält, die zum Erstellen eines Bildes in einer bestimmten Größe, einem bestimmten Format, einer bestimmten Qualität und einer bestimmten Scharfzeichnung erforderlich sind. Bildvorgaben sind eine Schlüsselkomponente der dynamischen Größenanpassung.

Wenn Sie sich die URLs von nahezu jeder Kundin bzw. jedem Kunden von Dynamic Media Classic ansehen, werden Sie wahrscheinlich eine Bildvorgabe in Verwendung sehen. Suchen Sie einfach nach „$name$“ am Ende der URL (wobei „name“ durch ein beliebiges Wort oder Wörter ersetzt werden kann).

Bildvorgaben verkürzen die URL. Anstatt also mehrere Bildbereitstellungs-Anweisungen pro Anfrage zu schreiben, können Sie eine einzelne Bildvorgabe schreiben. Diese beiden URLs generieren beispielsweise dasselbe 300 x 300 JPEG-Bild mit Scharfzeichnung, die zweite verwendet jedoch eine Bildvorgabe:

![Bild](assets/image-presets/image-preset-2.png)

Der wahre Wert von Bildvorgaben besteht darin, dass Unternehmensadmins nur die Definition dieser Bildvorgabe aktualisieren müssen und alle Bilder, die dieses Format verwenden, davon betroffen sind – ohne den Webcode zu ändern. Sie sehen die Ergebnisse jeder Änderung an einer Bildvorgabe, nachdem der Cache für die URL gelöscht wurde.

>[!IMPORTANT]
>
>Beim Ändern der Bildgröße sollte das Seitenverhältnis (d. h. das Verhältnis der Bildbreite zur Bildhöhe) immer proportional gehalten werden, damit das Bild nicht verzerrt wird.

Eine Bildvorgabe weist auf beiden Seiten ihres Namens ein Dollarzeichen ($) auf und folgt dem Fragezeichen (?) Trennzeichen.

>[!TIP]
>
>Erstellen Sie je eine Bildvorgabe pro eindeutiger Bildgröße auf Ihrer Site. Wenn Sie z. B. ein Bild mit den Maßen 350 x 350 für die Produktdetailseite, ein Bild mit den Maßen 120 x 120 für die Seiten zum Durchblättern/Suchen und ein Bild mit den Maßen 90 x 90 für einen Crosssell-Artikel benötigen, dann brauchen Sie drei Bildvorgaben, unabhängig davon, ob Sie 500 oder 500.000 Bilder haben.

- Weitere Informationen zu [Bildvorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html?lang=de).
- Erfahren Sie, wie Sie eine [Bildvorgabe erstellen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html?lang=de#creating-an-image-preset).

## Bildvorgaben und Scharfzeichnen

Mit Bildvorgaben wird in der Regel die Größe eines Bildes geändert, und jedes Mal, wenn Sie die Größe eines Bildes gegenüber der Originalgröße ändern, sollten Sie eine Scharfzeichnung hinzufügen. Das liegt daran, dass die Größenanpassung dazu führt, dass viele Pixel zusammengeführt und in einen kleineren Bereich verschmolzen werden, sodass das Bild weich und verschwommen aussieht. Durch Scharfzeichnen wird der Kontrast von Kanten und kontrastreichen Bereichen in einem Bild erhöht.

Wir gehen davon aus, dass die hochauflösenden Bilder, die Sie in Dynamic Media Classic hochladen, bei der Anzeige in voller Größe keine Scharfzeichnung benötigen, wenn hineingezoomt wird. Bei jeder kleineren Größe ist jedoch eine gewisse Scharfzeichnung in der Regel wünschenswert.

>[!TIP]
>
>Sie sollten immer schärfen, wenn die Größe von Bildern geändert wird! Das bedeutet, dass Sie jede Bildvorgabe (und Viewer-Vorgabe, die wir später besprechen werden) mit einer Scharfzeichnung versehen müssen.
>
>Wenn Ihre Bilder nicht gut aussehen, liegt das mit hoher Wahrscheinlichkeit daran, dass sie entweder scharfgezeichnet werden müssen oder die Qualität schon zu Beginn schlecht war.

Wie viel Scharfzeichnung hinzugefügt werden soll, ist völlig subjektiv. Manche Menschen mögen weichere Bilder, während andere sie sehr scharfgezeichnet mögen. Es ist einfach, ein Bild zu verbessern, indem man eine Kombination von Scharfzeichnungsfiltern auf ein Bild anwendet. Allerdings kann man es auch leicht übertreiben und ein Bild zu stark schärfen.

Die folgende Grafik zeigt drei Stufen der Scharfzeichnung. Von rechts nach links: keine Schärfung, genau das richtige Maß, und zu viel.

![Bild](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic bietet drei Arten von Scharfzeichnung: einfache Scharfzeichnung, Resample-Modus und Unschärfemaske.

Weitere Informationen über [Dynamic Media Classic-Scharfzeichnungsoptionen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html?lang=de#sharpening_an_image).

## Zusätzliche Ressourcen

[Handbuch zu Bildvorgaben](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Einstellungen, die zur Optimierung der Bildqualität und Ladegeschwindigkeit verwendet werden.

[Image Is Everything Part 2: It&#39;s Never Just a Blur — Quality Versus Speed](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). In diesem Blog-Beitrag wird die Verwendung von Bildvorgaben für die Bereitstellung hochwertiger, schnell ladender Bilder besprochen.
