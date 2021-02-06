---
title: Bild-, Muster-, Rotationsset- und gemischte Mediensets
description: Eine der nützlichsten und leistungsfähigsten Funktionen von Dynamic Media Classic ist die Unterstützung für das Erstellen von Rich-Media-Sets wie Bild, Muster, Rotationsset und gemischte Mediensets. Erfahren Sie, was jedes Rich-Media-Set ist und wie Sie die einzelnen Typen in Dynamic Media Classic erstellen. Erfahren Sie mehr über Stapelsatzvorgaben, mit denen die Erstellung von Rich-Media-Sets beim Hochladen automatisiert wird.
sub-product: dynamic-media
feature: sets
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1456'
ht-degree: 2%

---


# Bild-, Muster-, Rotationsset- und gemischte Mediensets {#media-sets}

Die Dynamic Media Classic-Set-Sammlungen, die über Einzelbilder hinausgehen, um die Größe und Größe dynamisch zu gestalten, bieten eine umfassendere Online-Erfahrung. In diesem Teil des Lernprogramms wird die Erstellung der folgenden Rich-Media-Sets in Dynamic Media Classic untersucht:

- Bildset
- Musterset
- Rotationsset
- Gemischtes Medienset

Außerdem wird erläutert, wie die Erstellung von Sets mithilfe von Stapelsatzvorgaben automatisiert wird.

## Alles, was Sie immer über Sets wissen möchten

Neben der grundlegenden dynamischen Größenanpassung und dem Zoomen sind Sätze wahrscheinlich das am häufigsten verwendete Dynamic Media Classic-Unterprodukt. Sätze sind im Wesentlichen &quot;virtuelle&quot;Assets, die keine tatsächlichen Bilder enthalten, aber aus einer Reihe von Beziehungen zu anderen Bildern und/oder Videos bestehen. Die Hauptattraktion von Sets ist, dass es sich um Mini-Anwendungen handelt, die &quot;aus dem Regal&quot;fertig sind. Damit meinen wir, dass jeder Set-Viewer seine eigene Logik und Oberfläche enthält, sodass Sie nur auf der Site aufrufen müssen. Darüber hinaus müssen Sie nur eine einzelne Asset-ID pro Set verfolgen, anstatt alle Elemente und Beziehungen selbst verwalten zu müssen.

Wenn Sie einen Satz erstellen, wird dieser Satz als separates Asset verwaltet, das zur Veröffentlichung markiert und veröffentlicht werden muss, bevor er über eine URL bereitgestellt werden kann. Alle zugehörigen Mitgliederelemente müssen ebenfalls veröffentlicht werden.

### Typen von Sets

Lernen wir die vier Typen von Sets kennen, die Sie in Dynamic Media Classic erstellen können: Bild-, Muster-, Rotationsset- und gemischte Mediensets.

## Bildset

Dies ist der am häufigsten verwendete Set-Typ. Normalerweise verwenden Sie es für alternative Ansichten desselben Elements. Es besteht aus mehreren Bildern, die Sie in den Viewer laden, indem Sie auf die zugehörige Miniaturansicht des Bildes klicken.

![image](assets/media-sets/image-set-1.jpg)

_Beispiel eines Bildsatzes_

Die URL für den obigen Bildsatz könnte wie folgt aussehen:

![image](assets/media-sets/image-set-url-1.png)

- Erfahren Sie mehr über Bildsätze mit dem Beginn [Schnellansicht zu Bildsätzen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- Erfahren Sie, wie Sie einen Bildsatz [erstellen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set) erstellen.

### Musterset

Dieser Set-Typ wird normalerweise verwendet, um farbige Ansichten desselben Elements anzuzeigen. Es besteht aus Paaren von Bildern und Farbfeldern.

Der Hauptunterschied zwischen einem Musterset und einem Bildsatz besteht darin, dass Mustersets ein anderes Bild als klickbares Farbfeld verwenden, während Bildsätze eine Miniaturansicht und eine Miniaturansicht des Originalbilds verwenden.

Mustersets färben keine Bilder (eine häufig auftretende falsche Vorstellung). Die Bilder werden einfach ausgetauscht, genau wie in einem Bildsatz. Die Mini-Musterbilder hätten mit Photoshop erstellt werden können, jede Farbe hätte einzeln fotografiert werden können, oder das Beschneidungswerkzeug in Dynamic Media Classic hätte verwendet werden können, um ein Farbfeld aus einem der farbigen Bilder zu erstellen.

![image](assets/media-sets/image-set-2.jpg)

_Beispiel eines Mustersets_

Die URL für das obige Musterset könnte wie folgt aussehen:

![image](assets/media-sets/image-set_url.png)

- Erfahren Sie mehr über Mustersets mit dem Beginn [Schnellansicht zu Mustersets](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- Erfahren Sie, wie Sie ein Musterset [erstellen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set) erstellen.

### Rotationsset

Dieser Satz wird normalerweise verwendet, um eine 360-Grad-Ansicht eines Elements anzuzeigen. Wie Mustersets verwenden Rotationssets keine 3D-Magie — Die eigentliche Arbeit ist, viele Fotos von einem Bild von allen Seiten zu erstellen. Mit dem Viewer können Sie einfach wie eine Stopp-Motion-Animation zwischen den Bildern wechseln.

Rotationssets können entweder in eine Richtung entlang einer einzelnen Achse gedreht oder alternativ als 2D-Rotationsset erstellt werden — auf mehreren Achsen drehen. Ein Auto kann beispielsweise gedreht werden, während alle Räder auf dem Boden liegen, und dann auch auf den Hinterrädern &quot;umgedreht&quot;und gedreht werden. Für ein korrekt eingerichtetes 2D-Rotationsset sollte die Anzahl der Bilder pro Zeile für jede Achse gleich sein. Mit anderen Worten, wenn Sie sich auf zwei Achsen drehen, benötigen Sie doppelt so viele Bilder wie ein Winkel.

![image](assets/media-sets/image-set-3.png)

_Beispiel eines Rotationssets_

Die URL für das oben stehende Rotationsset könnte wie folgt aussehen:

![image](assets/media-sets/spin-set.png)

- Weitere Informationen zu Rotationssets mit dem Beginn [Schnellansicht zu Rotationssets](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html).
- Erfahren Sie, wie Sie ein Rotationsset [erstellen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## Gemischtes Medienset

Dies ist ein Kombinationssatz. Damit können Sie alle vorherigen Sätze kombinieren und Videos zu einem einzigen Viewer hinzufügen. In diesem Arbeitsablauf erstellen Sie zunächst einen der Komponentensätze und stellen diese dann zu einem gemischten Medienset zusammen.

![image](assets/media-sets/image-set-4.png)

_Beispiel eines gemischten Mediensets_

Die URL für das oben stehende gemischte Medienset könnte wie folgt aussehen:

![image](assets/media-sets/image-set-url-1.png)

- Weitere Informationen zu gemischten Mediensets mit dem Beginn [Schnellansicht zu gemischten Mediensets](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- Erfahren Sie, wie Sie ein gemischtes Medienset [erstellen.](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)

Um ein Bild für Zoom, Set oder Video auf Ihrer Website anzuzeigen, rufen Sie es in einem Dynamic Media Classic-&quot;Viewer&quot;auf. Dynamic Media Classic umfasst Viewer für Rich-Media-Assets wie Mustersets, Rotationssets, Videos und viele andere.

Erfahren Sie mehr über [Viewer für AEM Assets und Dynamic Media Classic](https://docs.adobe.com/content/help/de-DE/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html).

## Stapelsatzvorgaben

Bisher haben wir bereits darüber diskutiert, wie Sätze manuell mithilfe der Dynamic Media Classic Build-Funktion erstellt werden können. Die Erstellung von Bildsätzen und Rotationssets kann jedoch mithilfe einer Stapelsatzvorgabe automatisiert werden, sofern eine standardisierte Benennungskonvention vorliegt.

Jede Vorgabe ist ein eindeutig benannter, in sich abgeschlossener Satz von Anweisungen, die definieren, wie der Satz mithilfe von Bildern, die den definierten Benennungsregeln entsprechen, erstellt wird. In der Vorgabe definieren Sie zuerst Benennungsregeln für die Assets, die Sie in einem Satz gruppieren möchten. Anschließend kann eine Stapelsatzvorgabe erstellt werden, um auf diese Bilder zu verweisen.

Es ist zwar möglich, die Vorgabe selbst zu erstellen (sie finden Sie unter **Einstellungen > Anwendungseinstellungen > Stapelsatzvorgaben**), aber als Best Practice sollten Sie das Beratungsteam oder den technischen Support für Sie einrichten. Deshalb:

- Stapelsatzvorgaben können komplex eingerichtet werden — Sie werden von regulären Ausdrücken betrieben, und wenn Sie kein Entwickler sind, kann diese Syntax unbekannt oder verwirrend sein.
- Nach der Erstellung sind sie standardmäßig aktiviert. Es gibt keine Rückgängig-Funktion. Wenn Sie Beginn haben, Tausende von Bildern hochzuladen und Ihre Vorgabe falsch konfiguriert ist, können Sie am Ende Hunderte oder Tausende von beschädigten Sets haben, die Sie manuell finden und löschen müssen.

Eine einfache Benennungsregel wurde früher vorgeschlagen, die sehr einfach in eine Stapelsatzvorgabe integriert werden kann. Da die Vorgaben jedoch sehr flexibel sind, können sie komplexe Benennungsstrategien handhaben. Kurz gesagt, die Bilder, die zu einem Satz gehören, sollten durch einen gemeinsamen Namen zusammengeführt werden — häufig die SKU-Nummer oder Produkt-ID. In Dynamic Media Classic haben Sie entweder eine Standardbenennungsregel für alle Bilder festgelegt, die für eine Vorgabe verwendet werden sollen, oder Sie können mehrere Vorgaben mit jeweils unterschiedlichen Benennungsregeln erstellen.

Stapelsatzvorgaben werden nur beim Hochladen angewendet. sie können nach dem Hochladen der Bilder nicht mehr ausgeführt werden. Daher ist es wichtig, dass Sie Ihre Benennungsregeln planen und eine Vorgabe erstellen, bevor Sie alle Beginn hochladen.

Nachdem die Vorgaben erstellt wurden, kann der Administrator der Firma wählen, ob sie aktiv oder inaktiv sind. Aktiv bedeutet, dass sie auf der Upload-Seite unter **Auftragsoptionen** angezeigt werden, während inaktive Vorgaben ausgeblendet bleiben.

Erfahren Sie, wie Sie eine Stapelsatzvorgabe [erstellen.](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)

### Stapelsatzvorgaben beim Hochladen verwenden

So verwenden Sie Stapelsatzvorgaben beim Hochladen, nachdem sie erstellt wurden:

1. Klicken Sie auf **Hochladen** und wählen Sie entweder **Von Desktop** oder **Über FTP**.
2. Klicken Sie auf **Auftragsoptionen**.
3. Öffnen Sie die Option **Stapelsatzvorgaben** und aktivieren oder deaktivieren Sie die Vorgabe, um sie beim Hochladen zu verwenden.
4. Suchen Sie nach Abschluss des Uploads in Ihrem Ordner nach den fertigen Sets.

Weitere Informationen zu [Stapelsatzvorgaben](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
