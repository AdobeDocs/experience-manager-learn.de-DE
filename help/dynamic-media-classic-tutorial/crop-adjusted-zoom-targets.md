---
title: Zuschneiden, angepasste Bilder und Zoom-Ziele
description: Das Primärbild von Dynamic Media Classic unterstützt das Erstellen separater Zuschnittversionen für jedes Bild, um Details anzuzeigen oder für Farbfelder, ohne dass für jedes Bild separate Zuschnittversionen erstellt werden müssen. Erfahren Sie, wie Sie Bilder in Dynamic Media Classic zuschneiden und als neue Primärdatei oder virtuelles Bild speichern, virtuelle angepasste Bilder speichern und sie anstelle von Primär-Assets verwenden und wie Sie Zoom-Ziele für Bilder erstellen, um hervorgehobene Details anzuzeigen.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: a1d83c77-a9e4-4ed1-9b00-65fb002164c0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2653'
ht-degree: 97%

---

# Zuschneiden, angepasste Bilder und Zoom-Ziele {#crop-adjusted-zoom-targets}

Einer der Hauptvorteile von Primärbildern in Dynamic Media Classic besteht darin, dass Sie das Bild-Asset für viele Zwecke wiederverwenden können. Vorher mussten Sie separate, zugeschnittene Versionen jedes Bildes erstellen, um Details anzuzeigen, oder für Farbfelder. Bei der Verwendung von Dynamic Media Classic können Sie dieselben Aufgaben für Ihre einzelnen Primärbilder ausführen und diese zugeschnittenen Versionen entweder als neue physische Dateien oder als virtuelle Derivate speichern, die keinen Speicherplatz belegen.

Am Ende dieses Tutorial-Abschnitts wissen Sie, wie Sie folgende Vorgänge durchführen:

- Zuschneiden von Bildern in Dynamic Media Classic und Speichern als neue Primärdateien oder als virtuelle Bilder. [Weitere Informationen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html?lang=de)
- Speichern Sie virtuelle angepasste Bilder und verwenden Sie sie anstelle von Primär-Assets. [Weitere Informationen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/adjusting-image.html?lang=de)
- Erstellen Sie Zoom-Ziele für Ihre Bilder, um ihre Highlights anzuzeigen. [Weitere Informationen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html?lang=de)

## Zuschneiden

Dynamic Media Classic verfügt über mehrere Bildbearbeitungswerkzeuge, die in der Benutzeroberfläche bequem verfügbar sind, einschließlich des Zuschneidewerkzeugs. Es kann mehrere Gründe geben, warum Sie Ihr Primärbild in Dynamic Media Classic zuschneiden möchten. Zum Beispiel:

- Sie haben keinen Zugriff auf die Originaldatei. Sie möchten das Bild mit einem anderen Zuschneide- oder Seitenverhältnis anzeigen, aber die Originaldatei ist nicht auf Ihrem Computer vorhanden oder Sie arbeiten von zu Hause aus. In diesem Fall können Sie das Bild in Dynamic Media Classic suchen, zuschneiden und speichern oder es als neue Version speichern.
- Sie mochten überschüssigen Leerraum entfernen. Das Bild wurde mit zu viel Leerraum fotografiert, wodurch das Produkt klein aussieht. Ihre Miniaturansichten sollten die Arbeitsfläche so weit wie möglich füllen.
- Um angepasste Bilder zu erstellen, werden virtuelle Kopien von Bildern erstellt, die keinen Speicherplatz belegen. Einige Unternehmen verfügen über Geschäftsregeln, nach denen sie separate Kopien desselben Bildes aufbewahren müssen, jedoch mit einem anderen Namen. Oder vielleicht hätten Sie gern eine zugeschnittene und eine nicht zugeschnittene Version desselben Bildes.
- So erstellen Sie neue Bilder aus einem Quellbild. Sie können beispielsweise Farbfelder oder eine Detailansicht des Hauptbildes erstellen. Sie können dies in Adobe Photoshop tun und es separat hochladen oder das Zuschneidewerkzeug in Dynamic Media Classic verwenden.

>[!NOTE]
>
>Alle URLs in den folgenden Erläuterungen zum Zuschneiden dienen nur zu Veranschaulichungszwecken. Es handelt sich nicht um Livelinks.

### Verwenden des Zuschneidewerkzeugs

Sie können auf das Zuschneidewerkzeug in Dynamic Media Classic über die Detailseite für ein Asset zugreifen oder indem Sie auf die Schaltfläche **Bearbeiten** klicken. Sie können das Tool zum Zuschneiden auf zwei Arten verwenden:

- Der standardmäßige Zuschneidemodus, in den Sie die Handles des Zuschneidefensters ziehen oder Werte in das Feld „Größe“ eingeben. Erfahren Sie, wie das [manuelle Zuschneiden](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop?lang=de) funktioniert.
- Abschneiden. Verwenden Sie dies, um zusätzlichen Leerraum um das Bild zu entfernen, indem Sie die Anzahl der Pixel berechnen, die nicht mit Ihrem Bild übereinstimmen. Erfahren Sie, wie [Zuschneiden durch Trimmen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html?lang=de#crop-to-remove-white-space-around-an-image) funktioniert.

### _Manuelles Zuschneiden_

Wenn Sie eine manuell zugeschnittene Version speichern, wird das Bild dauerhaft zugeschnitten. Dynamic Media Classic blendet die Pixel durch Hinzufügen eines internen URL-Modifikators zum Zuschneiden des Bildes aus. Nach dem Veröffentlichen ist klar zu sehen, dass das Bild zugeschnitten wurde. Sie können jedoch zu einem späteren Zeitpunkt zum Zuschneide-Editor zurückkehren und den Zuschnitt entfernen.

Sie können dann auswählen, ob es Sie es als neues Primärbild oder als zusätzliche Ansicht des Primärbildes speichern möchten. Ein neues Primärbild ist eine neue physische Datei (wie TIFF oder JPEG), die Speicherplatz belegt. Eine zusätzliche Ansicht ist ein virtuelles Bild, das keinen Server-Speicherplatz beansprucht. Es wird nicht empfohlen, die Option „Original ersetzen“ zu wählen, da dadurch das Primärbild überschrieben wird und der Zuschnitt dauerhaft wird. Zum Speichern als Primärbild oder zusätzliche Ansicht müssen Sie eine neue Asset-ID auswählen. Wie bei anderen Asset-IDs muss es sich auch hierbei um einen eindeutigen Namen in Dynamic Media Classic handeln.

### _Zuschneiden/Beschneiden_

Wenn Sie ein Bild mit zu viel Leerraum (zusätzlicher Arbeitsfläche) um das Hauptmotiv des Bildes herum hochladen, wird es im Web im Falle einer Größenänderung deutlich kleiner aussehen. Dies gilt insbesondere für Miniaturansichten, die höchstens 150 Pixel groß sind. Das Fotomotiv kann in dem zusätzlichen Platz, der um es herum vorhanden ist, verloren wirken.

Vergleichen Sie diese beiden Versionen desselben Bildes.

![Bild](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

Das Bild auf der rechten Seite ist wesentlich auffälliger, weil der zusätzliche Platz um das Produkt herum entfernt wurde. Das Zuschneiden mit dem Zuschneidewerkzeug kann jeweils nur für ein Bild erfolgen oder beim Hochladen als Batch-Prozess ausgeführt werden. Wir empfehlen die Ausführung als Batch-Prozess, wenn Sie möchten, dass alle Bilder konsistent bis an die Grenzen des Hauptmotivs zugeschnitten werden. Schneiden Sie bis zum Begrenzungsrahmen zu – dem Rechteck, das das Bild umgibt.

>[!NOTE]
>
>Beim Zuschneiden wird keine Transparenz um das Bild erzeugt. Dazu müssen Sie einen Beschneidungspfad in das Bild einbetten und die Hochladeoption **Maske aus Clip-Pfad erstellen** verwenden.
>
>Um ein Bild nach dem Zuschneiden wieder in seinen ursprünglichen Zustand zu versetzen, verwenden Sie die Option **Speichern**, zeigen Sie das Bild auf dem Zuschneideeditor-Bildschirm an und wählen Sie die Schaltfläche **Zurücksetzen** aus.

### _Zuschneiden beim Hochladen_

Wie bereits erwähnt, können Sie die Bilder auch beim Hochladen zuschneiden. Um beim Hochladen das Zuschneiden/Beschneiden anzuwenden, klicken Sie auf die Schaltfläche **Auftragsoptionen** und wählen Sie unter „Beschneidungsoptionen“ die Option **Zuschneiden** aus.

Dynamic Media Classic speichert diese Option für den nächsten Hochladevorgang. Möglicherweise möchten Sie, dass Bilder nicht immer, sondern nur für diesen Hochladevorgang zugeschnitten werden. Eine weitere Möglichkeit besteht darin, einen speziellen geplanten FTP-Upload-Auftrag festzulegen und die Beschneidungsoptionen dabei aunzuwenden. Auf diese Weise würden Sie den Auftrag nur ausführen, wenn Sie Ihre Bilder zuschneiden müssen.

>[!IMPORTANT]
>
>Wenn Sie einen Zuschnitt für Ihren Hochladevorgang festlegen, setzt Dynamic Media Classic ein Cookie, um diese Einstellung für das nächste Mal zu speichern. Als Best Practice wird empfohlen, vor dem nächsten Hochladen auf die Schaltfläche **Auf Unternehmenseinstellungen zurücksetzen** zu klicken, um alle vom letzten Upload verbliebenen Beschneidungsoptionen zu löschen. Andernfalls könnten die nächsten Bilder versehentlich zugeschnitten werden.

### Zuschneiden nach URL

Obwohl dies in Dynamic Media Classic nicht offensichtlich ist, können Sie auch nur über die URL zuschneiden (oder sogar eine Bildvorgabe zuschneiden lassen).

Bei jeder Verwendung des Zuschneidewerkzeugs werden im Feld unten URL-Werte angezeigt. Sie können diese Werte verwenden und sie als URL-Modifikator direkt auf ein Bild anwenden.

![Bild](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_Modifikatoren des Zuschneidebefehls unten im Zuschneideeditor_

![Bild](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Da die Größe pro Bild berechnet werden muss, wenn Sie durch Beschneiden zuschneiden, ist eine Automatisierung über die URL nicht möglich. Zuschneiden/Beschneiden kann nur beim Hochladen oder ansonsten durch Anwenden auf jeweils ein Bild ausgeführt werden.

### _Zuschneiden in der Bildvorgabe_

Bildvorgaben verfügen über ein Feld, in dem Sie zusätzliche Befehle zur Bildbereitstellung hinzufügen können. Um Ihrer Bildvorgabe denselben Zuschnitt wie oben hinzuzufügen, bearbeiten Sie die Vorgabe, fügen oder geben Sie die Werte in das Feld „URL-Modifikatoren“ ein und speichern und veröffentlichen Sie sie.

![Bild](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_Hinzufügen von Zuschneidebefehlen (oder eines beliebigen Befehls) zu den URL-Modifikatoren der Bildvorgabe_

Der Zuschnitt ist jetzt Teil dieser Bildvorgabe und wird bei jeder Verwendung automatisch angewendet. Diese Methode hängt natürlich von allen Bildern ab, die in diesem Umfang zugeschnitten werden müssen. Wenn nicht alle Bilder auf die gleiche Weise aufgenommen werden, funktioniert diese Methode nicht gut.

## Angepasste Bilder

Wenn Sie das Zuschneidewerkzeug verwenden, können Sie die Option **Als zusätzliche Ansicht des Primärbilds speichern** einsetzen. Beim Speichern entsteht so eine neue Art von Dynamic Media Classic-Asset – ein angepasstes Bild. Ein angepasstes Bild, auch als Bearbeitung bezeichnet, ist ein virtuelles Bild. Es handelt sich nicht um ein Bild, sondern um einen Datenbankverweis (wie ein Alias oder eine Verknüpfung) zum physischen Primärbild.

### Unterschieden zwischen echtem und angepasstem Bild`?`

Können Sie erkennen, welches das Primär- und welches das angepasste Bild ist?

![Bild](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

Wenn Sie nicht gerade in Dynamic Media Classic den Asset-Typ „Angepasstes Bild“ für SBR_MAIN2 sehen, sollte dies nicht möglich sein.

Ein angepasstes Bild belegt keinen Speicherplatz, da es nur als Zeileneintrag in der Datenbank vorhanden ist. Es ist auch dauerhaft mit dem ursprünglichen Asset verknüpft. Wenn das Original gelöscht wird, wird auch das angepasste Bild gelöscht. Es kann aus einem ganzen, nicht zugeschnittenen Bild oder nur einem Teil eines Bildes (einem Zuschnitt) bestehen.

![image](assets/crop-adjusted-zoom-targets/adjusted-image.png)

In der Regel erstellen Sie angepasste Bilder mit dem Zuschneide-Tool. Sie können jedoch auch mit den anderen Bildeditoren erstellt werden: mit den Tools „Anpassen“ und „Scharfzeichnen“.

Für angepasste Bilder ist eine eindeutige Asset-ID erforderlich. Nach der Veröffentlichung (Sie müssen sie wie jedes andere Asset veröffentlichen) agieren sie wie jedes andere Bild und werden anhand ihrer Asset-ID über eine URL aufgerufen. Auf der Detailseite können Sie angepasste Bilder, die mit einem übergeordneten Bild verknüpft sind, auf der Registerkarte **Erstellt und Derivate** anzeigen.

![image](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_Angepasste Ansichten für Musterbild ASIAN_BR_MAIN_

## Zoom-Ziele

Zoom-Ziele finden Sie auch im Menü **Bearbeiten** und auf der Seite **Details** eines Bildes. Sie ermöglichen es Ihnen, &quot;Hotspots&quot;festzulegen, um bestimmte Merchandising-Funktionen eines Zoombilds hervorzuheben. Anstatt separate Bilder durch Zuschneiden eines großen Musterbilds zu erstellen, kann der Zoom-Viewer die Details über dem Bild zusammen mit einer kurzen Beschriftung bereitstellen, die Sie erstellen.

![image](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Da Zoom-Ziele im Wesentlichen eine Merchandising-Funktion sind und Kenntnisse über die Verkaufsargumente für ein Produkt erfordern, werden sie normalerweise von einer Person im Merchandising- oder Produkt-Team eines Unternehmens erstellt.

Der Prozess ist sehr einfach – klicken Sie auf die Funktion, geben Sie ihr einen beschreibenden Namen und speichern Sie sie. Ziele können von einem Bild in ein anderes kopiert werden, wenn sie ähnlich sind. Der Prozess erfolgt jedoch manuell. In Dynamic Media Classic gibt es keine Möglichkeit, die Erstellung von Zoom-Zielen zu automatisieren, da jedes Bild unterschiedlich ist und verschiedene Funktionen aufweist.

Ein weiterer Faktor bei der Entscheidung, ob Sie Zoom-Ziele verwenden möchten, ist Ihre Viewer-Auswahl. Nicht alle Viewer-Typen können Zoom-Ziele anzeigen (z. B. unterstützt der Fly-out-Viewer sie nicht).

Erfahren Sie mehr zum [Erstellen von Zoom-Zielen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html?lang=de#creating-and-editing-zoom-targets).

![image](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Verwenden des Zoom-Ziel-Tools

Im Folgenden finden Sie den Workflow zum Erstellen von Zielen in Dynamic Media Classic.

1. Navigieren Sie zu Ihrem Bild, klicken Sie auf die Schaltfläche **Bearbeiten** und wählen Sie **Zoom-Ziele**.
2. Der Zoom-Ziel-Editor wird geladen. Sie sehen Ihr Bild in der Mitte, einige Schaltflächen oben und ein leeres Zielfeld auf der rechten Seite. Unten links sehen Sie eine ausgewählte Viewer-Vorgabe. Der Standard ist „Zoom1-Guided“.
3. Bewegen Sie den roten Kasten mit der Maus und klicken Sie, um ein neues Ziel zu erstellen.

   - Der rote Kasten ist der Zielbereich. Wenn Benutzende auf dieses Ziel klicken, zoomen sie in den Bereich innerhalb des Felds.
   - Die Zielgröße wird durch die Ansichtsgröße innerhalb der Viewer-Vorgabe bestimmt. Dadurch wird die Größe des Haupt-Zoom-Bilds bestimmt. Siehe _Einstellen der Anzeigegröße_ unten.

4. Das soeben erstellte Ziel wird blau angezeigt und rechts sehen Sie eine Miniaturansicht des Ziels sowie den Standardnamen „target-0“.
5. Um Ihr Ziel umzubenennen, klicken Sie auf die Miniaturansicht, geben Sie einen neuen **Namen** ein und klicken Sie auf **Eingabe** oder **Registerkarte** – wenn Sie einfach wegklicken, wird Ihr Name nicht gespeichert.
6. Während das Ziel ausgewählt ist, ist das Feld mit grünen gestrichelten Linien umrandet, wobei Sie die Größe ändern und es verschieben können. Ziehen Sie die Ecken, um die Größe zu ändern, oder ziehen Sie das Zielfeld, um es zu verschieben.

   - Dadurch wird das Bild im benutzerdefinierten Standard-Zoom-Viewer geladen. Stellen Sie sicher, dass die Viewer-Vorgabe Zoom-Ziele unterstützt – im Allgemeinen wurden alle Standardvorgaben mit dem Wort „-Guided“ für die Verwendung mit Zoom-Zielen entwickelt. Um die Ziele zu verwenden, bewegen Sie den Mauszeiger über die Miniaturansicht des Ziels (oder das Hotspot-Symbol) und zeigen Sie so die Bezeichnung an; klicken Sie darauf, damit der Viewer auf diese Funktion zoomt.
   - Wie alle anderen Arbeiten, die Sie in Dynamic Media Classic durchführen, müssen Sie sie veröffentlichen, damit Ihre Zoom-Ziele im Internet verfügbar sind. Wenn Sie bereits einen Viewer verwenden, der Ziele unterstützt, werden diese sofort angezeigt (sobald der Cache gelöscht wurde). Wenn Sie jedoch keinen für Zoom-Ziele aktivierten Viewer verwenden, bleiben diese ausgeblendet.

     ![image](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Wenn Sie ein Ziel entfernen müssen, wählen Sie es aus, indem Sie auf die Miniaturansicht klicken, und drücken Sie die Schaltfläche **Ziel löschen** oder drücken Sie die Entf-Taste auf der Tastatur.
8. Klicken Sie weiter, um weiterhin neue Ziele hinzuzufügen und ggf. umzubenennen und/oder die Größe nach dem Hinzufügen zu ändern.
9. Wenn Sie fertig sind, klicken Sie auf die Schaltfläche **Speichern** und dann auf **Vorschau**.

### Festlegen der Anzeigegröße in der Zoom-Viewer-Vorgabe

Sprechen wir nun darüber, woher die Größe der Zoom-Ziele stammt. Innerhalb der Viewer-Vorgabe für Ihren Zoom-Viewer befindet sich eine Einstellung namens „Anzeigegröße“. Die Anzeigegröße entspricht der Größe des Zoom-Bilds im Viewer. Sie unterscheidet sich von der Staging-Größe, d. h. der Gesamtgröße Ihres Viewers, einschließlich der Benutzeroberflächen-Komponenten und Grafiken.

Wenn Sie ein neues Ziel erstellen, werden dessen Größe und Seitenverhältnis von der Ansichtsgröße abgeleitet. Wenn Ihre Anzeigegröße beispielsweise 200 x 200 beträgt, können Sie nur quadratische Ziele mit einem maximalen Zoom-Bereich von 200 Pixeln erstellen. Ihre Ziele können größer als 200 Pixel sein, jedoch immer quadratisch. Das bedeutet aber auch, dass das Bild in Ihrem Zoom-Viewer nur 200 Pixel hat – die Größe des Zoom-Ziels hat einen direkten Bezug zur Größe Ihres Viewers. Sie entscheiden also zunächst über Ihr Viewer-Design, bevor Sie Ziele festlegen.

Standardmäßig ist die Anzeigegröße jedoch leer (auf 0 x 0 eingestellt), da die Größe des Hauptansichtsbilds dynamisch ist und automatisch entsprechend der Größe der Anzeigefläche abgeleitet wird. Das Problem besteht darin, dass das Zoom-Ziel-Tool nicht weiß, welche Größe die Ziele haben sollen, wenn Sie in Ihrer Vorgabe keine explizite Anzeigegröße festlegen.

Wenn Sie das Zoom-Ziel-Tool laden, wird die Anzeigegröße neben dem Namen der Vorgabe angezeigt. Vergleichen Sie die Anzeigegröße zwischen der integrierten Vorgabe „Zoom1-Guided“ und der benutzerdefinierten Vorgabe „ZT_AUTHORING“.

![Bild](assets/crop-adjusted-zoom-targets/view-size.jpg)

Sie können sehen, dass die integrierte Vorgabe eine Größe von 900 x 550 hat, was bedeutet, dass das Ziel nie kleiner werden kann als dieses eher große Format. Das ist wahrscheinlich zu groß. Wenn Sie ein 2000-Pixel-Bild haben, können Sie nur eine Funktion aufrufen, die mindestens 900 Pixel breit ist. Der Benutzer kann manuell weiter zoomen, aber Sie können sie nicht näher heranführen. Wenn Sie die Anzeigegröße auf 350 x 350 festlegen, können Ziele ziemlich nah herangezoomt oder stärker vergrößert werden. Wenn Sie jedoch ein größeres Zoombild in Ihrem Viewer wünschen, müssen Sie eine neue Vorgabe erstellen, da Ihre mit 350 Pixel gesperrt ist.

### Erstellen oder Bearbeiten einer Viewer-Vorgabe, die Zoom-Ziele unterstützt

Um die Anzeigegröße festzulegen, erstellen oder bearbeiten Sie eine Viewer-Vorgabe, die Zoom-Ziele unterstützt.

1. Wechseln Sie in der Viewer-Vorgabe zur Option **Zoom-Einstellungen**.
2. Legen Sie eine Breite und Höhe fest.
3. Speichern Sie die Vorgabe und schließen Sie das Fenster. Wenn Sie diese Vorgabe auf Ihrer Live-Site verwenden möchten, müssen Sie sie später auch veröffentlichen.
4. Wählen Sie im Zoom-Ziel-Tool unten links die bearbeitete Vorgabe aus. Sie sehen sofort die neue Ansichtsgröße, die in Ihren Zielen widerspiegelt wird.
