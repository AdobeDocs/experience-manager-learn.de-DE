---
title: Bildvorgaben
description: Bildvorgaben in Dynamic Media Classic alle Einstellungen enthalten, die zum Erstellen eines Bildes in einer bestimmten Größe, einem bestimmten Format, einer bestimmten Qualität und einer bestimmten Scharfzeichnung erforderlich sind. Bildvorgaben sind eine Schlüsselkomponente der dynamischen Größe. Wenn Sie sich eine URL in Dynamic Media Classic ansehen, können Sie leicht erkennen, ob eine Bildvorgabe verwendet wird. Erfahren Sie mehr über Bildvorgaben, warum sie so nützlich sind und wie man sie erstellt.
sub-product: dynamic-media
feature: Dynamic Media Classic, Bildvorgaben
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '706'
ht-degree: 1%

---


# Bildvorgaben {#image-presets}

Eine Bildvorgabe ist im Wesentlichen ein Rezept, das alle Einstellungen enthält, die zum Erstellen eines Bildes in einer bestimmten Größe, einem bestimmten Format, einer bestimmten Qualität und einer bestimmten Scharfzeichnung erforderlich sind. Bildvorgaben sind eine Schlüsselkomponente der dynamischen Größe.

Wenn Sie sich die URLs von nahezu jedem Dynamic Media Classic-Kunden ansehen, wird wahrscheinlich eine verwendete Bildvorgabe angezeigt. Suchen Sie einfach nach $name$ am Ende der URL (wobei jedes Wort oder jedes Wort den Namen ersetzt).

Bildvorgaben verkürzen die URL. Anstatt also mehrere Image Serving-Anweisungen pro Anfrage zu schreiben, können Sie eine einzelne Bildvorgabe schreiben. Diese beiden URLs generieren beispielsweise dasselbe 300 x 300 JPEG-Bild mit Scharfzeichnung, die zweite verwendet jedoch eine Bildvorgabe:

![Bild](assets/image-presets/image-preset-2.png)

Der wahre Wert von Bildvorgaben besteht darin, dass jeder Unternehmensadministrator die Definition dieser Bildvorgabe aktualisieren und jedes Bild mit diesem Format beeinflussen kann, ohne dass ein Webcode geändert wird. Sie sehen die Ergebnisse jeder Änderung an einer Bildvorgabe, nachdem der Cache für die URL gelöscht wurde.

>[!IMPORTANT]
>
>Beim Ändern der Bildgröße sollte das Seitenverhältnis (das Verhältnis der Bildbreite zur Bildhöhe) immer proportional gehalten werden, damit das Bild nicht verzerrt wird.

Eine Bildvorgabe weist auf beiden Seiten ihres Namens ein Dollarzeichen ($) auf und folgt dem Fragezeichen (?) Trennzeichen.

>[!TIP]
>
>Erstellen Sie eine Bildvorgabe pro eindeutiger Bildgröße auf Ihrer Site. Wenn Sie z. B. ein Bild mit dem Format 350 x 350 für die Produktdetailseite, ein Bild mit dem Format 120 x 120 für die Durchsuchen-/Suchseiten und ein Bild mit dem Format 90 x 90 für ein Querverkauf-/Funktionselement benötigen, benötigen Sie drei Bildvorgaben, unabhängig davon, ob Sie 500 oder 500.0000000.

- Erfahren Sie mehr über [Bildvorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Erfahren Sie, wie Sie [eine Bildvorgabe erstellen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Bildvorgaben und Scharfzeichnen

Bildvorgaben ändern normalerweise die Größe eines Bildes. Jedes Mal, wenn Sie die Größe eines Bildes von seiner ursprünglichen Größe aus ändern, sollten Sie eine Scharfzeichnung hinzufügen. Das liegt daran, dass die Größenanpassung dazu führt, dass viele Pixel zusammengeführt und in einen kleineren Bereich verschmolzen werden, sodass das Bild weich und verschwommen aussieht. Durch Scharfzeichnen wird der Kontrast von Kanten und kontrastreichen Bereichen in einem Bild erhöht.

Wir gehen davon aus, dass die hochauflösenden Bilder, die Sie in Dynamic Media Classic hochladen, bei der Anzeige in voller Größe keine Scharfzeichnung benötigen - wenn sie vergrößert werden. Bei jeder kleineren Größe ist jedoch eine gewisse Scharfzeichnung in der Regel wünschenswert.

>[!TIP]
>
>Immer schärfen, wenn die Größe von Bildern geändert wird! Das bedeutet, dass Sie jede Bildvorgabe (und Viewer-Vorgabe, die wir später besprechen werden) mit einer Scharfzeichnung versehen müssen.
>
>Wenn Ihre Bilder nicht gut aussehen, liegt die Wahrscheinlichkeit daran, dass sie scharfgezeichnet werden müssen, oder die Qualität war anfangs schlecht.

Wie viel Scharfzeichnung hinzugefügt werden soll, ist völlig subjektiv. Manche Menschen mögen weichere Bilder, während andere sie sehr scharf mögen. Es ist einfach, ein Bild durch Ausführen einer Kombination von Scharfzeichnungsfiltern auf einem Bild zu verbessern. Es ist jedoch auch einfach, ein Bild zu übertreiben und zu stark zu schärfen.

Die folgende Grafik zeigt drei Stufen der Scharfzeichnung. Von rechts nach links haben Sie keine Scharfzeichnung, nur den richtigen Betrag und zu viel.

![Bild](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic ermöglicht drei Arten der Scharfzeichnung: Einfaches Scharfzeichnen, Neuberechnen und Unschärfemaske.

Erfahren Sie mehr über [Dynamic Media Classic-Scharfzeichnungsoptionen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Zusätzliche Ressourcen

[Anleitung zur Bildvorgabe](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Einstellungen, die zur Optimierung der Bildqualität und Ladegeschwindigkeit verwendet werden.

[Bild ist alles Teil 2: Es ist nie nur ein Weichzeichner - Qualität gegen Geschwindigkeit](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). In einem Blog-Beitrag wird die Verwendung von Bildvorgaben für die Bereitstellung hochwertiger, schnell ladender Bilder besprochen.

[Bild ist alles Webinare](https://dynamicmediaseries2019.enterprise.adobeevents.com/). Links zu Aufzeichnungen aller drei Webinare in der Serie _Bild ist alles_. [Webinar 2](https://seminars.adobeconnect.com/p6lqaotpjnd3) behandelt Bildvorgaben.
