---
title: Bild-, Farbfeld-, Rotations- und gemischte Medien-Sets
description: Eine der nützlichsten und leistungsfähigsten Funktionen von Dynamic Media Classic ist die Unterstützung für das Erstellen von Rich Media Sets wie Bild-, Farbfeld-, Rotations- und gemischte Medien-Sets. Erfahren Sie, was jedes Rich Media Set ist und wie Sie die einzelnen Typen in Dynamic Media Classic erstellen. Erfahren Sie auch mehr über Stapelsatzvorgaben, die die Erstellung von Rich Media Sets beim Hochladen automatisieren.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: ht
source-wordcount: '1433'
ht-degree: 100%

---

# Bild-, Farbfeld-, Rotations- und gemischte Medien-Sets {#media-sets}

Dynamic Media Classic-Sets bieten mehr, als Größe und Zoom für einzelne Bilder anzupassen, und sorgen so für ein noch besseres Online-Erlebnis. In diesem Abschnitt des Tutorials wird untersucht, wie die folgenden Rich Media Sets in Dynamic Media Classic erstellt werden:

- Bild-Set
- Farbfeld-Set
- Rotations-Set
- Gemischtes Medien-Set

Außerdem wird erläutert, wie mithilfe von Stapelsatzvorgaben die Erstellung von Sets über einen Upload automatisiert wird.

## Alles, was Sie schon immer über Sets wissen wollten

Neben der grundlegenden dynamischen Größen- und Zoom-Anpassung sind Sets wahrscheinlich das Dynamic Media Classic-Unterprodukt, das am häufigsten verwendet wird. Sets sind im Grunde „virtuelle“ Assets, die keine tatsächlichen Bilder enthalten, aber aus einer Reihe von Beziehungen zu anderen Bildern und/oder Videos bestehen. Was Sets vor allem attraktiv macht, ist die Tatsche, dass es sich um sofort einsetzbare Minianwendungen handelt. Damit ist gemeint, dass jeder Setviewer über eine eigene Logik und Oberfläche verfügt, sodass Sie sie nur noch auf der Site aufrufen müssen. Darüber hinaus müssen Sie nur eine einzelne Asset-ID pro Set nachverfolgen, anstatt alle zugehörigen Assets und Beziehungen selbst verwalten zu müssen.

Wenn Sie ein Set erstellen, wird dieses Set als separates Asset verwaltet, das zur Veröffentlichung markiert und veröffentlicht werden muss, bevor es über eine URL bereitgestellt werden kann. Alle zugehörigen Assets müssen ebenfalls veröffentlicht werden.

### Set-Typen

Im Folgenden werden die vier Typen von Sets vorgestellt, die Sie in Dynamic Media Classic erstellen können: Bild-, Farbfeld-, Rotations- und gemischte Medien-Sets.

## Bild-Set

Dies ist der am häufigsten verwendete Set-Typ. Normalerweise wird dieses Set für alternative Ansichten desselben Artikels oder Elements eingesetzt. Es besteht aus mehreren Bildern, die Sie in den Viewer laden, indem Sie auf die zugehörige Miniaturansicht des Bildes klicken.

![Bild](assets/media-sets/image-set-1.jpg)

_Beispiel eines Bild-Sets_

Die URL für das obige Bild-Set könnte wie folgt aussehen:

![Bild](assets/media-sets/image-set-url-1.png)

- Weitere Informationen über Bild-Sets finden Sie unter [Schnellstart für Bild-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html?lang=de).
- Weitere Informationen finden Sie unter [Erstellen eines Bild-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html?lang=de#creating-an-image-set).

### Farbfeld-Set

Dieser Set-Typ wird normalerweise verwendet, um farbige Ansichten desselben Artikels oder Elements anzuzeigen. Dieses Set besteht aus Paaren von Bildern und Farbfeldern (oder Farbmustern).

Der Hauptunterschied zwischen einem Farbfeld- und einem Bild-Set besteht darin, dass Farbfeld-Sets ein anderes Bild als klickbares Farbfeld verwenden, Bild-Sets hingegen eine klickbare Miniaturansicht oder Miniaturversion des Originalbildes.

Farbfeld-Sets färben keine Bilder (eine häufige Fehlannahme). Die Bilder werden einfach ausgetauscht, genau wie in einem Bild-Set. Die Mini-Farbfeldbilder können beispielsweise mit Photoshop, durch Fotografieren jeder einzelnen Farbe oder durch Generieren eines Farbfelds aus einem der farbigen Bilder mithilfe des Freistellungswerkzeugs in Dynamic Media Classic erstellt werden.

![Bild](assets/media-sets/image-set-2.jpg)

_Beispiel eines Farbfeld-Sets_

Die URL für das obige Farbfeld-Set könnte wie folgt aussehen:

![Bild](assets/media-sets/image-set_url.png)

- Erfahren Sie mehr über Farbfeld-Sets mit dem [Schnellstart für Farbfeld-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html?lang=de).
- Erlernen Sie das [Erstellen eines Farbfeld-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html?lang=de#creating-a-swatch-set).

### Rotations-Set

Dieses Set wird normalerweise verwendet, um eine 360-Grad-Ansicht eines Elements anzuzeigen. Wie Farbfeld-Sets verwenden Rotations-Sets keine 3D-Magie – die eigentliche Arbeit besteht darin, viele Fotos eines Bildes von allen Seiten zu erstellen. Mit dem Viewer können Sie einfach wie eine Stopp-Bewegungs-Animation zwischen den Bildern wechseln.

Rotations-Sets können sich entweder in eine Richtung entlang einer einzelnen Achse drehen oder alternativ als 2D-Rotations-Set erstellt werden, das auf mehreren Achsen gedreht wird. Beispielsweise kann ein Auto gedreht werden, während sich alle Räder auf dem Boden befinden, und auch auf den Hinterrädern stehend nach oben gedreht werden. Für ein ordnungsgemäß eingerichtetes 2D-Rotations-Set sollte die Anzahl der Bilder pro Zeile für jede Achse gleich sein. Anders ausgedrückt: Wenn Sie etwas um zwei Achsen drehen, benötigen Sie doppelt so viele Bilder wie für eine Drehung um einen einzelnen Winkel.

![Bild](assets/media-sets/image-set-3.png)

_Beispiel eines Rotations-Sets_

Die URL für das obige Rotations-Set könnte wie folgt aussehen:

![Bild](assets/media-sets/spin-set.png)

- Weitere Informationen zu Rotations-Sets finden Sie unter [Schnellstart für Rotations-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html?lang=de).
- Erlernen Sie das [Erstellen eines Rotations-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html?lang=de#creating-a-spin-set).

## Gemischtes Medien-Set

Dies ist ein Kombinations-Set. Dadurch können Sie alle vorherigen Sets sowie Videos in einem einzigen Viewer hinzufügen und kombinieren. In diesem Workflow erstellen Sie zunächst einen der Komponenten-Sets und stellen diese dann in einem gemischten Medien-Set zusammen.

![Bild](assets/media-sets/image-set-4.png)

_Beispiel eines gemischten Medien-Sets_

Die URL für das obige gemischte Medien-Set könnte wie folgt aussehen:

![Bild](assets/media-sets/image-set-url-1.png)

- Erfahren Sie mehr über gemischte Medien-Sets mit dem [Schnellstart für gemischte Medien-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html?lang=de).

- Erlernen Sie das [Erstellen eines gemischten Medien-Sets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html?lang=de#creating-a-mixed-media-set).

Um ein Bild für den Zoom, ein Set oder ein Video auf Ihrer Website anzuzeigen, rufen Sie es in einem Dynamic Media Classic-„Viewer“ auf. Dynamic Media Classic umfasst Viewer für Rich-Media-Assets wie Farbfeld-Sets, Rotations-Sets, Videos und viele andere.

Hier finden Sie weitere Informationen zu [Viewern für AEM Assets und Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=de).

## Stapelsatzvorgaben

Bisher wurde besprochen, wie Sets mithilfe der Dynamic Media Classic-Build-Funktion manuell erstellt werden. Es ist jedoch möglich, die Erstellung von Bild- und Rotations-Sets mithilfe einer Stapelsatzvorgabe zu automatisieren, sofern Sie über eine standardisierte Benennungskonvention verfügen.

Jede Vorgabe ist ein eindeutig benannter, in sich abgeschlossener Satz von Anweisungen, die definieren, wie das Bild-Set mithilfe von Bildern erstellt wird, die den definierten Namenskonventionen entsprechen. In der Vorgabe definieren Sie zunächst Benennungskonventionen für die Assets, die Sie in einem Set gruppieren möchten. Anschließend kann eine Stapelsatzvorgabe erstellt werden, um auf diese Bilder zu verweisen.

Auch wenn es möglich ist, die Vorgabe selbst zu erstellen (sie finden Sie unter **Einstellungen > Anwendungseinstellungen > Stapelsatzvorgaben**), sollten Sie als Best Practice Ihr Beratungs-Team oder den technischen Support sie für Sie einrichten lassen. Dies ist der Grund:

- Stapelsatzvorgaben können komplex einzurichten sein. Sie basieren auf regulären Ausdrücken. Wenn Sie nicht gerade Entwicklerin oder Entwickler sind, ist diese Syntax möglicherweise unbekannt oder verwirrend.
- Nach der Erstellung sind sie standardmäßig aktiviert. Die Funktion „Rückgängig machen“ ist nicht verfügbar. Wenn Sie Tausende von Bildern hochladen und Ihre Vorgabe falsch konfiguriert ist, kann es vorkommen, dass Sie Hunderte oder Tausende fehlerhafte Sets haben, die Sie manuell finden und löschen müssen.

Zuvor wurde eine einfache Benennungskonvention vorgeschlagen, die sehr einfach in eine Stapelsatzvorgabe integriert werden kann. Da die Vorgaben jedoch sehr flexibel sind, können sie komplexe Benennungsstrategien handhaben. Kurz gesagt, die Bilder, die zu einem Set gehören, sollten mit einem gemeinsamen Namen verknüpft werden – oft handelt es sich dabei um die SKU-Nummer oder die Produkt-ID. In Dynamic Media Classic legen Sie entweder eine Standardbenennungsregel fest, die für alle Bilder als Vorgabe verwendet werden soll, oder Sie können mehrere Vorgaben mit jeweils unterschiedlichen Benennungsregeln erstellen.

Stapelsatzvorgaben werden nur beim Hochladen angewendet. Sie können nicht mehr ausgeführt werden, nachdem die Bilder hochgeladen wurden. Daher ist es wichtig, Ihre Namenskonvention zu planen und eine Vorgabe zu erstellen, bevor Sie mit dem Laden aller Bilder beginnen.

Nachdem die Vorgaben erstellt wurden, können die Unternehmensadmins auswählen, ob sie aktiv oder inaktiv sind. Aktiv bedeutet, dass sie auf der Upload-Seite unter **Auftragsoptionen** erscheinen, während inaktive Vorgaben ausgeblendet bleiben.

Erlernen Sie das [Erstellen einer Stapelsatzvorgabe](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=de#creating-a-batch-set-preset).

### Verwenden von Stapelsatzvorgaben beim Hochladen

So verwenden Sie Stapelsatzvorgaben beim Hochladen nach deren Erstellung:

1. Klicken Sie auf **Hochladen** und wählen Sie entweder **Über den Desktop** oder **Per FTP**.
2. Klicken Sie auf **Auftragsoptionen**.
3. Öffnen Sie die Option **Stapelsatzvorgaben** und aktivieren oder deaktivieren Sie die Vorgabe, um sie beim Hochladen zu verwenden.
4. Nachdem der Upload abgeschlossen ist, suchen Sie in Ihrem Ordner nach den fertigen Sets.

Hier finden Sie weitere Informationen zu [Stapelsatzvorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=de#batch-set-presets).
