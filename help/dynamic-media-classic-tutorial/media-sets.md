---
title: Bild-, Farb-, Rotations- und Sets für gemischte Medien
description: Eine der nützlichsten und leistungsfähigsten Funktionen von Dynamic Media Classic ist die Unterstützung für das Erstellen von Rich-Media-Sets wie Bild-, Farb-, Farb-, Rotations- und gemischten Mediensets. Erfahren Sie, was jedes Rich-Media-Set ist und wie Sie die einzelnen Typen in Dynamic Media Classic erstellen. Erfahren Sie mehr über Stapelsatzvorgaben, die die Erstellung von Rich-Media-Sets beim Hochladen automatisieren.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 2%

---

# Bild-, Farb-, Rotations- und Sets für gemischte Medien {#media-sets}

Dynamic Media Classic-Sammlungen, die über einzelne Bilder hinausgehen, um die dynamische Größe und den Zoom zu optimieren, ermöglichen ein umfangreicheres Online-Erlebnis. In diesem Abschnitt des Tutorials wird untersucht, wie die folgenden Rich-Media-Sets in Dynamic Media Classic erstellt werden:

- Bildset
- Musterset
- Rotationsset
- Gemischtes Medienset

Außerdem wird erläutert, wie mithilfe von Stapelsatzvorgaben die Erstellung von Sets über einen Upload automatisiert wird.

## Alles, was Sie immer über Sets wissen möchten

Neben der grundlegenden dynamischen Skalierung und dem Zoom sind Sets wahrscheinlich das am häufigsten verwendete Dynamic Media Classic-Unterprodukt. Sets sind im Wesentlichen &quot;virtuelle&quot;Assets, die keine tatsächlichen Bilder enthalten, aber aus einer Reihe von Beziehungen zu anderen Bildern und/oder Videos bestehen. Die Hauptattraktion von Sets besteht darin, dass es sich um Minianwendungen handelt, die &quot;aus dem Regal&quot;bereit sind. Damit meinen wir, dass jeder Set-Viewer seine eigene Logik und Oberfläche enthält, sodass Sie nur auf der Site aufrufen müssen. Darüber hinaus müssen Sie nur eine einzelne Asset-ID pro Set verfolgen, anstatt alle zugehörigen Assets und Beziehungen selbst verwalten zu müssen.

Wenn Sie einen Satz erstellen, wird dieser Satz als separates Asset verwaltet, das zur Veröffentlichung markiert und veröffentlicht werden muss, bevor er über eine URL bereitgestellt werden kann. Alle zugehörigen Mitglieder-Assets müssen ebenfalls veröffentlicht werden.

### Typen von Sets

Im Folgenden werden die vier Arten von Sets vorgestellt, die Sie in Dynamic Media Classic erstellen können: Bild-, Farb-, Rotations- und Sets für gemischte Medien.

## Bildset

Dies ist der am häufigsten verwendete Set-Typ. Normalerweise verwenden Sie sie für alternative Ansichten desselben Elements. Es besteht aus mehreren Bildern, die Sie in den Viewer laden, indem Sie auf die zugehörige Miniaturansicht des Bildes klicken.

![image](assets/media-sets/image-set-1.jpg)

_Beispiel eines Bildsets_

Die URL für das obige Bildset könnte wie folgt aussehen:

![image](assets/media-sets/image-set-url-1.png)

- Erfahren Sie mehr über Bildsets mit dem [Schnellstart zu Bildsets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- Erfahren Sie, wie Sie [Erstellen eines Bildsets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set).

### Musterset

Dieser Set-Typ wird normalerweise verwendet, um farbige Ansichten desselben Elements anzuzeigen. Es besteht aus Paaren von Bildern und Farbmustern.

Der Hauptunterschied zwischen einem Muster und einem Bildset besteht darin, dass Mustersets ein anderes Bild als klickbares Farbfeld verwenden, während Bildsets eine Miniatur-Miniaturansicht verwenden, auf die geklickt werden kann, und eine Miniaturansicht-Version des Originalbilds.

Mustersets färben keine Bilder (eine häufige falsche Vorstellung). Die Bilder werden einfach ausgetauscht, genau wie in einem Bildset. Die Mini-Musterbilder hätten mit Photoshop erstellt werden können, jede Farbe hätte einzeln fotografiert werden können oder das Zuschnitt-Tool in Dynamic Media Classic hätte verwendet werden können, um ein Farb-Bildmuster zu erstellen.

![image](assets/media-sets/image-set-2.jpg)

_Beispiel eines Mustersets_

Die URL für das obige Musterset könnte wie folgt aussehen:

![image](assets/media-sets/image-set_url.png)

- Erfahren Sie mehr über Mustersets mit dem [Schnellstart für Mustersets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- Erfahren Sie, wie Sie [Erstellen eines Mustersets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set).

### Rotationsset

Dieser Satz wird normalerweise verwendet, um eine 360-Grad-Ansicht eines Elements anzuzeigen. Wie Mustersets verwenden Rotationssets keine 3D-Magie - die eigentliche Arbeit besteht darin, viele Fotos eines Bildes von allen Seiten zu erstellen. Mit dem Viewer können Sie einfach wie eine Stopp-Motion-Animation zwischen den Bildern wechseln.

Rotationssets können sich entweder in eine Richtung entlang einer einzelnen Achse drehen oder alternativ als 2D-Rotationsset erstellt werden, das auf mehreren Achsen gedreht wird. Beispielsweise kann ein Auto gedreht werden, während sich alle Räder auf dem Boden befinden, und dann auch auf den Hinterrädern nach oben gedreht werden. Für ein ordnungsgemäß eingerichtetes 2D-Rotationsset sollte die Anzahl der Bilder pro Zeile für jede Achse gleich sein. Anders ausgedrückt: Wenn Sie sich auf zwei Achsen drehen, benötigen Sie doppelt so viele Bilder wie eine Drehung um einen Winkel.

![image](assets/media-sets/image-set-3.png)

_Beispiel eines Rotationssets_

Die URL für das obige Rotationsset könnte wie folgt aussehen:

![image](assets/media-sets/spin-set.png)

- Weitere Informationen zu Rotationssets finden Sie unter [Schnellstart für Rotationssets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html).
- Erfahren Sie, wie Sie [Erstellen eines Rotationssets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## Gemischtes Medienset

Dies ist ein Kombinationssatz. Dadurch können Sie alle vorherigen Sets sowie Videos in einem einzigen Viewer kombinieren. In diesem Workflow erstellen Sie zunächst einen der Komponentensätze und stellen sie dann in einem gemischten Medienset zusammen.

![image](assets/media-sets/image-set-4.png)

_Beispiel eines gemischten Mediensets_

Die URL für das obige gemischte Medienset könnte wie folgt aussehen:

![image](assets/media-sets/image-set-url-1.png)

- Erfahren Sie mehr über gemischte Mediensets mit der [Schnellstart für gemischte Mediensets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- Erfahren Sie, wie Sie [Erstellen eines gemischten Mediensets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set).

Um ein Bild für den Zoom, ein Set oder ein Video auf Ihrer Website anzuzeigen, nennen Sie es in einem Dynamic Media Classic-&quot;Viewer&quot;. Dynamic Media Classic umfasst Viewer für Rich-Media-Assets wie Mustersets, Rotationssets, Videos und viele andere.

Weitere Informationen [Viewer für AEM Assets und Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=de).

## Stapelsatz-Vorgaben

Bisher wurde besprochen, wie Sets mithilfe der Dynamic Media Classic Build-Funktion manuell erstellt werden. Es ist jedoch möglich, die Erstellung von Bild- und Rotationssets mithilfe einer Stapelsatzvorgabe zu automatisieren, sofern Sie über eine standardisierte Benennungskonvention verfügen.

Jede Vorgabe ist ein eindeutig benannter, in sich abgeschlossener Satz von Anweisungen, die definieren, wie das Set mit Bildern erstellt wird, die den definierten Benennungskonventionen entsprechen. In der Vorgabe definieren Sie zunächst Benennungskonventionen für die Assets, die Sie in einem Satz gruppieren möchten. Anschließend kann eine Stapelsatzvorgabe erstellt werden, um auf diese Bilder zu verweisen.

Auch wenn es möglich ist, die Vorgabe selbst zu erstellen (sie finden Sie unter **Einstellungen > Anwendungseinstellungen > Stapelsatzvorgaben** ), sollten Sie als Best Practice Ihr Beratungsteam oder den technischen Support für Sie einrichten lassen. Dies ist der Grund:

- Stapelsatzvorgaben können komplex einzurichten sein. Sie basieren auf regulären Ausdrücken. Wenn Sie kein Entwickler sind, ist diese Syntax möglicherweise unbekannt oder verwirrend.
- Nach der Erstellung sind sie standardmäßig aktiviert. Die Funktion &quot;Rückgängig&quot;ist nicht verfügbar. Wenn Sie Tausende von Bildern hochladen und Ihre Vorgabe falsch konfiguriert ist, kann es vorkommen, dass Sie Hunderte oder Tausende von fehlerhaften Sets haben, die Sie manuell finden und löschen müssen.

Zuvor wurde eine einfache Benennungskonvention vorgeschlagen, die sehr einfach in eine Stapelsatzvorgabe integriert werden kann. Da die Vorgaben jedoch sehr flexibel sind, können sie komplexe Benennungsstrategien handhaben. Kurz gesagt, die Bilder, die zu einem Satz gehören, sollten mit einem gemeinsamen Namen verknüpft werden - oft handelt es sich dabei um die SKU-Nummer oder die Produkt-ID. In Dynamic Media Classic legen Sie entweder eine Standardbenennungsregel fest, nach der alle Bilder für eine Vorgabe verwendet werden sollen, oder Sie können mehrere Vorgaben mit jeweils unterschiedlichen Benennungsregeln erstellen.

Stapelsatzvorgaben werden nur beim Hochladen angewendet; sie können nach dem Hochladen der Bilder nicht mehr ausgeführt werden. Daher ist es wichtig, Ihre Namenskonvention zu planen und eine Vorgabe zu erstellen, bevor Sie mit dem Laden aller Bilder beginnen.

Nachdem die Vorgaben erstellt wurden, kann der Unternehmensadministrator auswählen, ob sie aktiv oder inaktiv sind. Aktiv bedeutet, dass sie auf der Upload-Seite unter **Auftragsoptionen**, während inaktive Vorgaben ausgeblendet bleiben.

Erfahren Sie, wie Sie [Erstellen einer Stapelsatzvorgabe](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### Verwenden von Stapelsatzvorgaben beim Hochladen

So verwenden Sie Stapelsatzvorgaben beim Hochladen nach deren Erstellung:

1. Klicken **Hochladen** und wählen Sie entweder **Vom Desktop** oder **Über FTP**.
2. Klicken **Auftragsoptionen**.
3. Öffnen Sie die **Stapelsatzvorgaben** und aktivieren oder deaktivieren Sie die Vorgabe, um sie beim Hochladen zu verwenden.
4. Nachdem der Upload abgeschlossen ist, suchen Sie in Ihrem Ordner nach den fertigen Sets.

Weitere Informationen [Stapelsatzvorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
