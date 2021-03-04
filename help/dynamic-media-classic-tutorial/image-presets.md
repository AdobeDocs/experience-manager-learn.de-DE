---
title: Bildvorgaben
description: Bildvorgaben in Dynamic Media Classic alle Einstellungen enthalten, die zum Erstellen eines Bildes in einer bestimmten Größe, einem bestimmten Format, einer bestimmten Qualität und einer bestimmten Scharfzeichnung erforderlich sind. Bildvorgaben sind eine wichtige Komponente der dynamischen Größe. Wenn Sie sich eine URL in Dynamic Media Classic ansehen, können Sie leicht erkennen, ob eine Bildvorgabe verwendet wird. Erfahren Sie mehr über Bildvorgaben, warum sie so nützlich sind und wie man sie erstellt.
sub-product: dynamic-media
feature: Dynamic Media Classic, Bildvorgaben
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: Geschäftspraktiker
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 1%

---


# Bildvorgaben {#image-presets}

Eine Bildvorgabe ist im Wesentlichen ein Rezept, das alle Einstellungen enthält, die zum Erstellen eines Bildes in einer bestimmten Größe, einem bestimmten Format, einer bestimmten Qualität und einem bestimmten Scharfzeichnen erforderlich sind. Bildvorgaben sind eine wichtige Komponente der dynamischen Größe.

Wenn Sie sich die URLs von nahezu jedem Dynamic Media Classic-Kunden ansehen, wird wahrscheinlich eine Bildvorgabe verwendet. Suchen Sie einfach nach $name$ am Ende der URL (wobei jedes Wort oder jedes Wort den Namen ersetzt).

Bildvorgaben verkürzen die URL, sodass Sie, anstatt mehrere Image-Server-Anweisungen pro Anforderung zu schreiben, eine einzige Bildvorgabe schreiben können. Diese beiden URLs erzeugen beispielsweise dasselbe JPEG-Bild mit 300 x 300 Pixeln und Scharfzeichnen, bei der zweiten URL wird jedoch eine Bildvorgabe verwendet:

![image](assets/image-presets/image-preset-2.png)

Der wahre Wert von Bildvorgaben besteht darin, dass jeder Administrator der Firma die Bildvorgabe aktualisieren kann und jedes Bild in diesem Format beeinflussen kann — — ohne Änderung von Webcode. Die Ergebnisse einer Änderung an einer Bildvorgabe werden angezeigt, nachdem der Cache für die URL gelöscht wurde.

>[!IMPORTANT]
>
>Beim Ändern der Bildgröße sollte das Seitenverhältnis, d. h. das Verhältnis der Bildbreite zur Bildhöhe, stets proportional gehalten werden, damit das Bild nicht verzerrt wird.

Eine Bildvorgabe hat auf beiden Seiten ihres Namens ein Dollarzeichen ($) und folgt dem Fragezeichen (?) Trennzeichen.

>[!TIP]
>
>Erstellen Sie eine Bildvorgabe pro individueller Bildgröße auf Ihrer Site. Wenn Sie z. B. ein Bild mit 350 x 350 für die Produktdetailseite, ein Bild mit 120 x 120 für die Durchsuchen-/Suchseiten und ein Bild mit 90 x 90 für ein Querverkauf-/Sonderelement benötigen, dann benötigen Sie drei Bildvorgaben, unabhängig davon, ob Sie über 500 oder 500.00000 verfügen.

- Erfahren Sie mehr über [Bildvorgaben](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Erfahren Sie, wie Sie eine Bildvorgabe [erstellen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset) erstellen.

## Bildvorgaben und Scharfzeichnen

Bei Bildvorgaben wird die Größe eines Bildes normalerweise angepasst. Bei jeder Größenänderung eines Bildes in der Originalgröße sollten Sie das Scharfzeichnen hinzufügen. Das liegt daran, dass bei der Größenanpassung viele Pixel zusammengeführt und in einen kleineren Bereich übergehen, sodass das Bild weich und verschwommen aussieht. Durch Scharfzeichnen wird der Kontrast von Kanten und Bereichen mit hohem Kontrast in einem Bild erhöht.

Wir gehen davon aus, dass die hochauflösenden Bilder, die Sie in Dynamic Media Classic hochladen, beim Anzeigen in voller Größe keine Scharfzeichnung benötigen — beim Einzoomen. Bei kleinerer Größe ist jedoch ein Scharfzeichnen in der Regel wünschenswert.

>[!TIP]
>
>Schärfen Sie immer, wenn Sie die Größe von Bildern ändern! Das bedeutet, dass Sie jeder Bildvorgabe (und Viewer-Vorgabe, die wir später besprechen werden) Scharfzeichnen hinzufügen müssen.
>
>Wenn Ihre Bilder nicht gut aussehen, ist es wahrscheinlich, dass sie Scharfzeichnen benötigen oder die Qualität war vielleicht schlecht zu beginnen.

Wie viel Scharfzeichnung hinzugefügt werden soll, ist völlig subjektiv. Manche Leute mögen weiche Bilder, andere mögen sie sehr scharf. Sie können ein Bild ganz einfach verbessern, indem Sie eine Kombination aus Scharfzeichnungs-Filtern auf einem Bild ausführen. Es ist jedoch auch einfach, ein Bild zu übertreiben und zu scharfzuzeichnen.

Die folgende Grafik zeigt drei Stufen des Scharfzeichnens. Von rechts nach links haben Sie kein Scharfzeichnen, nur den richtigen Betrag und zu viel.

![image](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic ermöglicht drei Arten des Scharfzeichnens: Einfaches Scharfzeichnen, Resamplingmodus und Unschärfemaske.

Weitere Informationen zu [Dynamic Media Classic Sharpening Options](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Zusätzliche Ressourcen

[Anleitung](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf) zu Bildvorgaben. Zu verwendende Einstellungen zur Optimierung der Bildqualität und der Ladegeschwindigkeit.

[Bild ist alles Teil 2: Es ist nie nur ein Weichzeichner — Qualität im Vergleich zur Geschwindigkeit](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Ein Blog-Beitrag, in dem die Verwendung von Bildvorgaben für die Bereitstellung hochwertiger, schnell ladender Bilder diskutiert wird.

[Bild ist alles Webinare](https://dynamicmediaseries2019.enterprise.adobeevents.com/). Links zu Aufzeichnungen aller drei Webinare der Serie _Bild ist alles_. [Webinar 2 ](https://seminars.adobeconnect.com/p6lqaotpjnd3) behandelt Bildvorgaben.
