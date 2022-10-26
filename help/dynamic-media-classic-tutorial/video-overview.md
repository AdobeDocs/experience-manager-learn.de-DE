---
title: Videoüberblick
description: Dynamic Media Classic bietet eine automatische Konvertierung von Videos beim Hochladen, Video-Streaming auf Desktop- und Mobilgeräte sowie adaptive Videosets, die für die Wiedergabe auf der Grundlage von Gerät und Bandbreite optimiert sind. Erfahren Sie mehr über Videos in Dynamic Media Classic und erhalten Sie eine Einführung in Videokonzepte und Terminologie. Machen Sie sich anschließend mit dem Erfahren, wie Sie Videos hochladen und kodieren, Videovorgaben zum Hochladen auswählen, eine Videovorgabe hinzufügen oder bearbeiten, Videos in einem Video-Viewer in der Vorschau anzeigen, Videos auf Web- und mobilen Sites bereitstellen, Untertitel und Kapitelmarken zu Videos hinzufügen und Video-Viewer für Desktop- und Mobilbenutzer veröffentlichen.
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: dfbf316f-3922-4bc7-b3f3-2a5bbdeb7063
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '6118'
ht-degree: 1%

---

# Videoüberblick {#video-overview}

Dynamic Media Classic bietet eine automatische Konvertierung von Videos beim Hochladen, Video-Streaming auf Desktop- und Mobilgeräte sowie adaptive Videosets, die für die Wiedergabe auf der Grundlage von Gerät und Bandbreite optimiert sind. Eines der wichtigsten Dinge an Videos ist, dass der Workflow einfach ist - er wurde so entwickelt, dass jeder ihn verwenden kann, auch wenn er nicht mit der Videotechnologie vertraut ist.

Am Ende dieses Abschnitts des Tutorials erfahren Sie, wie Sie:

- Hochladen und Kodieren (Transkodieren) von Videos in unterschiedlichen Größen und Formaten
- Wählen Sie aus den verfügbaren Videovorgaben für das Hochladen
- Hinzufügen oder Bearbeiten von Videokodierungsvorgaben
- Vorschau von Videos in einem Video-Viewer
- Bereitstellen von Videos auf Web- und mobilen Sites
- Hinzufügen von Untertiteln und Kapitelmarken zu Videos
- Anpassen und Veröffentlichen von Video-Viewern für Desktop- und Mobilbenutzer

>[!NOTE]
>
>Alle URLs in diesem Kapitel dienen nur Veranschaulichungszwecken. es sich nicht um Live-Links handelt.

## Überblick über Dynamic Media Classic Video

Lassen Sie uns zunächst ein besseres Verständnis der Videomöglichkeiten mit Dynamic Media Classic erhalten.

### Funktionen und Funktionen

Die Dynamic Media Classic-Videoplattform bietet alle Teile der Videolösung - das Hochladen, Konvertieren und Verwalten von Videos. die Möglichkeit, einem Video Untertitel und Kapitelmarken hinzuzufügen; und die Möglichkeit, Vorgaben für eine einfache Wiedergabe zu verwenden.

Dadurch ist es einfach, hochwertige adaptive Videos für das Streaming auf mehreren Bildschirmen zu veröffentlichen, einschließlich Desktop-, iOS-, Android™-, BlackBerry®- und Windows-Mobilgeräten. Es umfasst Versionen desselben Videos, die mit unterschiedlichen Bitraten und Formaten kodiert wurden, wie 400 kBit/s, 800 kBit/s und 1000 kBit/s. Der Desktop-Computer oder das Mobilgerät erkennt die verfügbare Bandbreite.

Darüber hinaus wird die Videoqualität automatisch umgeschaltet, wenn sich die Netzwerkbedingungen auf dem Desktop oder Mobilgerät ändern. Wenn ein Kunde auf einem Desktop in den Vollbildmodus wechselt, antwortet das adaptive Videoset mit einer besseren Auflösung, wodurch sich das Anzeigeerlebnis des Kunden verbessert. Mit adaptiven Videosets erhalten Sie die bestmögliche Wiedergabe für Kunden, die Dynamic Media Classic-Videos auf mehreren Bildschirmen und Geräten wiedergeben.

### Videomanagement

Das Arbeiten mit Videos kann komplexer sein als das Arbeiten mit noch digitalen Bildern. Mit Videos beschäftigen Sie sich mit zahlreichen Formaten und Standards und der Unsicherheit, ob Ihre Audience in der Lage ist, Ihre Clips wiederzugeben. Dynamic Media Classic erleichtert die Arbeit mit Videos und bietet viele leistungsstarke Tools &quot;im Hintergrund&quot;, beseitigt aber die Komplexität der Arbeit mit ihnen.

Dynamic Media Classic erkennt und kann mit vielen verschiedenen verfügbaren Quellformaten arbeiten. Das Lesen des Videos ist jedoch nur ein Teil der Arbeit - Sie müssen das Video auch in ein Web-freundliches Format konvertieren. Dynamic Media Classic übernimmt diese Aufgabe, indem es Ihnen ermöglicht, Videos in H.264-Videos zu konvertieren.

Das Konvertieren des Videos selbst kann mit den vielen professionellen und begeisterten Tools kompliziert werden. Dynamic Media Classic hält es einfach, indem es einfache Vorgaben anbietet, die für unterschiedliche Qualitätseinstellungen optimiert sind. Wenn Sie jedoch etwas Benutzerspezifischeres wünschen, können Sie auch eigene Vorgaben erstellen.

Wenn Sie viele Videos haben, werden Sie die Möglichkeit schätzen, alle Ihre Assets zusammen mit Ihren Bildern und anderen Medien in Dynamic Media Classic zu verwalten. Sie können Ihre Assets, einschließlich Video-Assets, organisieren, katalogisieren und durchsuchen, indem XMP Metadaten unterstützt werden.

### Videowiedergabe

Ähnlich wie das Problem, Videos zu konvertieren, um sie Web-freundlich und barrierefrei zu machen, besteht auch das Problem bei der Implementierung und Bereitstellung von Videos auf Ihrer Site. Die Entscheidung, ob Sie einen Spieler kaufen oder einen eigenen erstellen, ihn für verschiedene Geräte und Bildschirme kompatibel machen und dann Ihre Spieler pflegen, kann eine Vollzeitbeschäftigung sein.

Auch hier besteht der Ansatz von Dynamic Media Classic darin, Ihnen zu ermöglichen, die Vorgabe und den Viewer auszuwählen, die Ihren Anforderungen entsprechen. Sie haben viele verschiedene Viewer-Optionen und eine Bibliothek mit zahlreichen Vorgaben zur Verfügung.

Sie können Videos einfach an Web- und Mobilgeräte senden, da Dynamic Media Classic HTML5-Videos unterstützt. So können Sie Benutzer, die verschiedene Browser ausführen, sowie Benutzer der Android™- und iOS-Plattform als Ziel auswählen. Streaming-Videos ermöglichen eine reibungslose Wiedergabe von längeren oder hochauflösenden Inhalten, während progressive HTML5-Videos über Vorgaben verfügen, die für kleine Bildschirme optimiert sind.

Viewer-Vorgaben für Videos können je nach Viewer-Typ teilweise konfiguriert werden.

Wie alle Viewer erfolgt die Integration über eine einzelne Dynamic Media Classic-URL pro Viewer oder Video.

>[!NOTE]
>
>Verwenden Sie als Best Practice Dynamic Media Classic HTML5 Video-Viewer. Die in HTML5 Video-Viewern verwendeten Vorgaben sind robuste Video-Player. Durch die Kombination der Möglichkeit, die Wiedergabekomponenten mit HTML 5 und CSS zu entwerfen, eingebettete Wiedergabe zu ermöglichen und adaptives und progressives Streaming zu verwenden, je nach Browserfunktion, erweitern Sie die Reichweite Ihrer Rich-Media-Inhalte auf Desktop-, Tablet- und Mobilbenutzer und sorgen für ein optimiertes Videoerlebnis.

Ein letzter Hinweis zum Dynamic Media Classic-Video, das für einige Kunden gelten kann: Es ist nicht möglich, dass für alle Unternehmen die automatische Konvertierung, das Streaming oder Videovorgaben für ihr Konto aktiviert sind. Wenn Sie aus irgendeinem Grund nicht auf die URLs für Streaming-Videos zugreifen können, kann dies der Grund sein. Sie können progressiv heruntergeladene Videos hochladen und veröffentlichen und Zugriff auf alle Video-Viewer haben. Um jedoch die gesamten Dynamic Media Classic-Videokompetenzen nutzen zu können, wenden Sie sich an Ihren Kundenbetreuer oder Vertriebsmitarbeiter, um diese Funktionen zu aktivieren.

Weitere Informationen [Video in Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/quick-start-video.html).

## Video 101

### Grundlegende Videokonzepte und Terminologie

Bevor wir anfangen, besprechen wir einige Begriffe, mit denen Sie vertraut sein sollten, um mit Videos zu arbeiten. Diese Konzepte sind nicht spezifisch für Dynamic Media Classic, und wenn Sie Videos für eine professionelle Website verwalten, sollten Sie gut daran arbeiten, weitere Informationen zu diesem Thema zu erhalten. Wir empfehlen einige Ressourcen am Ende dieses Abschnitts.

- **Kodierung/Transkodierung.** Bei der Kodierung wird die Videokomprimierung angewendet, um unkomprimierte Rohdaten in ein Format zu konvertieren, das die Arbeit erleichtert. Die Transkodierung bezieht sich auf die Konvertierung von einer Kodierungsmethode in eine andere.

   - Übergeordnete Videodateien, die mit der Videobearbeitungssoftware erstellt wurden, sind häufig zu groß und nicht im richtigen Format für die Bereitstellung an Online-Zielorte. Sie werden normalerweise für die schnelle Wiedergabe auf dem Desktop und für die Bearbeitung kodiert, nicht aber für die Bereitstellung über das Internet.
   - Um digitale Videos in das richtige Format und die richtigen Spezifikationen für die Wiedergabe auf verschiedenen Bildschirmen zu konvertieren, werden die Videodateien in eine kleinere, effiziente Dateigröße transkodiert, die für die Bereitstellung im Internet und auf Mobilgeräten optimal ist.

- **Videokomprimierung.** Die Verringerung der Datenmenge, die zur Darstellung digitaler Videobilder verwendet wird, ist eine Kombination aus räumlicher Bildkomprimierung und zeitlicher Bewegungskompensation.

   - Die meisten Komprimierungstechniken sind verlustbehaftet, d. h. sie geben Daten aus, um eine kleinere Größe zu erreichen.
   - Beispielsweise ist DV-Video komprimiert relativ wenig und ermöglicht Ihnen, das Quellmaterial einfach zu bearbeiten, aber es ist viel zu groß, um es über das Web zu verwenden oder sogar auf eine DVD zu setzen.

- **Dateiformate.** Das Format ist ein Container, der einer ZIP-Datei ähnelt und bestimmt, wie Dateien in der Videodatei organisiert sind, normalerweise jedoch nicht, wie sie kodiert sind.

   - Zu den gebräuchlichen Dateiformaten für Quellvideos gehören unter anderem Windows Media (WMV), QuickTime (MOV), Microsoft® AVI und MPEG. Von Dynamic Media Classic veröffentlichte Formate sind MP4.
   - Eine Videodatei enthält in der Regel mehrere Spuren - einen Video-Track (ohne Audio) und eine oder mehrere Audiospuren (ohne Video) - die miteinander verknüpft und synchronisiert sind.
   - Das Videodateiformat bestimmt, wie diese verschiedenen Datenspuren und Metadaten organisiert werden.

- **Codec.** Ein Video-Codec beschreibt den Algorithmus, mit dem ein Video mithilfe der Komprimierung kodiert wird. Audio wird auch über einen Audio-Codec kodiert.

   - Codecs minimieren die Menge an Informationen, die zum Abspielen von Videos erforderlich sind. Statt Informationen zu jedem einzelnen Frame werden nur Informationen zu den Unterschieden zwischen einem Frame und dem nächsten gespeichert.
   - Da sich die meisten Videos von einem Frame zum nächsten kaum ändern, ermöglichen Codecs hohe Komprimierungsraten, was zu kleineren Dateigrößen führt.
   - Ein Videoplayer dekodiert das Video entsprechend seinem Codec und zeigt dann eine Reihe von Bildern oder Frames auf dem Bildschirm an.
   - Zu den gängigen Video-Codecs gehören H.264, On2 VP6 und H.263.

![image](assets/video-overview/bird-video.png)

- **Auflösung.** Höhe und Breite des Videos in Pixel.

   - Die Größe des Quellvideos wird durch die Kamera und die Ausgabe aus der Bearbeitungssoftware bestimmt. Eine HD-Kamera erzeugt ein Video mit einer hohen Auflösung von 1920 x 1080. Um jedoch eine reibungslose Wiedergabe im Web zu gewährleisten, sollten Sie die Kamera auf eine kleinere Auflösung wie 1280 x 720, 640 x 480 oder weniger herunterladen (anpassen).
   - Die Auflösung wirkt sich direkt auf die Dateigröße und die Bandbreite aus, die für die Wiedergabe des Videos erforderlich ist.

- **Seitenverhältnis anzeigen.** Verhältnis der Breite eines Videos zur Höhe eines Videos. Wenn das Seitenverhältnis des Videos nicht mit dem Verhältnis des Players übereinstimmt, wird möglicherweise &quot;schwarze Balken&quot;oder ein leerer Bereich angezeigt. Zur Anzeige von Videos werden zwei gängige Seitenverhältnisse verwendet:

   - 4:3 (1,33:1). Wird für fast alle standardmäßigen TV-Broadcast-Inhalte verwendet.
   - 16:9 (1,78:1). Wird für fast alle Breitbild-, High-Definition-TV-Inhalte (HDTV) und Filme verwendet.

- **Bitrate/Datenrate.** Die Datenmenge, die kodiert wird, um eine Sekunde Videowiedergabe auszumachen (in Kilobit pro Sekunde).

   - Im Allgemeinen gilt: Je niedriger die Bitrate, desto wünschenswerter ist sie für das Web, da sie schneller heruntergeladen werden kann. Dies kann aber auch bedeuten, dass die Qualität aufgrund von Komprimierungsverlusten gering ist.
   - Ein guter Codec sollte eine niedrige Bitrate bei guter Qualität ausgleichen.

- **Framerate (Frames pro Sekunde oder FPS).** Die Anzahl der Frames oder Standbilder für jede Sekunde des Videos. Normalerweise wird Nordamerikanisches Fernsehen (NTSC) mit 29,97 FPS ausgestrahlt. Europäisches und asiatisches Fernsehen (PAL) wird in 25 FPS ausgestrahlt. und Film (analog und digital) sind in der Regel 24 (23.976) FPS.

   - Um die Dinge verwirrender zu machen, gibt es auch progressive und Zwischenzeilenrahmen. Jeder progressive Rahmen enthält einen gesamten Bildrahmen, während Zwischenzeilenrahmen jede zweite Pixelzeile in einem Bildrahmen enthalten. Die Frames werden dann schnell wiedergegeben und scheinen miteinander zu verschmelzen. Film verwendet eine progressive Scan-Methode, während digitale Videos normalerweise interlaced sind.
   - Im Allgemeinen spielt es keine Rolle, ob Ihr Quellmaterial zwischenlackiert ist oder nicht - Dynamic Media Classic behält die Scanmethode im konvertierten Video bei.
   - Streaming/Progressiver Versand Beim Video-Streaming handelt es sich um das Senden von Medien in einem kontinuierlichen Stream, der beim Eintreffen wiedergegeben werden kann, während das progressiv heruntergeladene Video wie jede andere Datei von einem Server heruntergeladen und lokal in Ihrem Browser zwischengespeichert wird.

Hoffentlich hilft Ihnen dieser Leitfaden, die verschiedenen Optionen bei der Verwendung von Dynamic Media Classic-Videos zu verstehen.

## Video-Workflow

Beim Arbeiten mit Videos in Dynamic Media Classic folgen Sie einem grundlegenden Arbeitsablauf, der dem Arbeiten mit Bildern ähnelt.

![image](assets/video-overview/video-overview-2.png)

1. Laden Sie zunächst die Videodateien in Dynamic Media Classic hoch. Öffnen Sie dazu die **Tools-Menü** unten im Bedienfeld &quot;Dynamic Media Classic-Erweiterung&quot;und wählen Sie **Hochladen in Dynamic Media Classic > Dateien in den Ordnernamen** oder **In Dynamic Media Classic hochladen > Ordner in Ordnernamen**. &quot;Ordnername&quot;ist der Ordner, den Sie derzeit mit der Erweiterung durchsuchen. Videodateien können groß sein. Daher wird empfohlen, FTP zum Hochladen großer Dateien zu verwenden. Wählen Sie im Rahmen des Uploads eine oder mehrere Videovorgaben für die Videokodierung aus. Videos können beim Hochladen in MP4-Videos transkodiert werden. Weitere Informationen zur Verwendung und Erstellung von Kodierungsvorgaben finden Sie unten im Thema &quot;Videovorgaben&quot;. Informationen zu [Hochladen und Kodieren von Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. Wählen Sie eine Video-Viewer-Vorgabe aus, wählen Sie sie aus und ändern Sie sie, und zeigen Sie eine Vorschau des Videos an. Sie würden entweder eine vordefinierte Viewer-Vorgabe auswählen oder Ihre eigene anpassen. Wenn Sie Benutzer mit Mobilgeräten ansprechen, müssen Sie hier nichts tun, da mobile Plattformen keinen Viewer oder keine Vorgabe benötigen. Weitere Informationen [Vorschau von Videos in einem Video-Viewer](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) und [Hinzufügen oder Bearbeiten von Video-Viewer-Vorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. Führen Sie eine Videoveröffentlichung aus, rufen Sie die URL ab und integrieren Sie sie. Der Hauptunterschied zwischen diesem Schritt für den Video-Workflow und dem Bild-Workflow besteht darin, dass Sie eine spezielle Videoveröffentlichung anstelle der standardmäßigen Image Serving-Veröffentlichung (oder vielleicht und ) ausführen. Die Video-Viewer-Integration auf dem Desktop funktioniert genau wie die Bild-Viewer-Integration. Bei Mobilgeräten ist sie jedoch noch einfacher. Sie benötigen lediglich die URL zum Video selbst.

### Über Transkodierung

Die Transkodierung wurde früher als der Konvertierungsprozess von einer Kodierungsmethode zu einer anderen definiert. Bei Dynamic Media Classic wird das Quellvideo aus dem aktuellen Format in das MP4-Format konvertiert. Dies ist erforderlich, bevor Ihr Video im Desktop-Browser oder auf einem Mobilgerät angezeigt wird.

Dynamic Media Classic kann die Transkodierung für Sie durchführen - ein enormer Vorteil. Sie können das Video selbst transkodieren und die Dateien hochladen, die bereits in MP4 konvertiert wurden. Dies kann jedoch ein komplexer Prozess sein, der eine hoch entwickelte Software erfordert. Wenn du nicht weißt, was du tust, wirst du normalerweise beim ersten Versuch keine guten Ergebnisse erzielen.

Dynamic Media Classic konvertiert nicht nur die Dateien für Sie, sondern erleichtert auch die Bereitstellung benutzerfreundlicher Vorgaben. Sie müssen wirklich nicht viel über die technische Seite dieses Prozesses wissen - alles was Sie wissen sollten ist ungefähr die endgültige(n) Größe(en), die Sie aus dem System holen möchten, und ein Gefühl der Bandbreite, die Ihre Endbenutzer haben.

Die vordefinierten Vorgaben sind zwar praktisch und decken die meisten Anforderungen ab, aber manchmal möchten Sie etwas benutzerspezifischeres. In diesem Fall können Sie eine eigene Kodierungsvorgabe erstellen. In Dynamic Media Classic wird eine Kodierungsvorgabe als Videovorgabe bezeichnet. Dies wird weiter unten in diesem Kapitel erläutert.

### Über Streaming

Eine weitere wichtige Funktion ist das Video-Streaming, eine Standardfunktion der Dynamic Media Classic-Videoplattform. Streaming-Medien werden während der Bereitstellung ständig von einem Endbenutzer empfangen und ihm präsentiert. Dies ist aus mehreren Gründen von Bedeutung und wünschenswert.

Streaming erfordert in der Regel weniger Bandbreite als progressiver Download, da nur der Teil des Videos bereitgestellt wird, der angesehen wird. Der Dynamic Media Classic-Video-Streaming-Server und die Viewer verwenden die automatische Bandbreitenerkennung, um den bestmöglichen Stream für die Internetverbindung eines Benutzers bereitzustellen.

Beim Streaming beginnt die Videowiedergabe früher als mit anderen Methoden. Außerdem werden Netzwerkressourcen effizienter genutzt, da nur die angezeigten Teile des Videos an den Client gesendet werden.

Die andere Versandmethode ist der progressive Download. Verglichen mit Streaming-Videos bietet nur ein konsistenter Vorteil beim progressiven Download - Sie benötigen keinen Streaming-Server, um das Video bereitzustellen. Und hier kommt natürlich Dynamic Media Classic zum Einsatz - Dynamic Media Classic hat einen Streaming-Server, der in die Plattform integriert ist, sodass Sie nicht die Mühe oder zusätzlichen Kosten für die Wartung dieser dedizierten Hardware benötigen.

Progressives Download-Video kann von jedem normalen Webserver bereitgestellt werden. Dies kann praktisch und potenziell kostengünstig sein. Beachten Sie jedoch, dass progressive Downloads nur über eingeschränkte Such- und Navigationsfunktionen verfügen und Benutzer auf Ihre Inhalte zugreifen und diese wiederverwenden können. In einigen Situationen, wie z. B. der Wiedergabe hinter strikten Netzwerk-Firewalls, kann die Streaming-Bereitstellung blockiert werden. In diesen Fällen kann die Rücknahme auf eine progressive Bereitstellung wünschenswert sein.

Progressiver Download ist eine gute Wahl für Hobbyisten oder Websites mit geringen Traffic-Anforderungen. wenn es ihnen nichts ausmacht, ob ihr Inhalt auf dem Computer eines Benutzers zwischengespeichert wird; wenn sie nur kürzere Videos (unter 10 Minuten) bereitstellen müssen; oder wenn Besucher aus irgendeinem Grund kein Streaming-Video erhalten können.

Sie müssen Ihr Video streamen, wenn Sie erweiterte Funktionen und die Steuerung der Videobereitstellung benötigen und/oder wenn Sie Videos für größere Zielgruppen anzeigen müssen (z. B. mehrere 100 gleichzeitige Betrachter), die Nutzung verfolgen und Berichte erstellen oder Statistiken anzeigen möchten oder wenn Sie Ihren Betrachtern das beste interaktive Wiedergabeerlebnis bieten möchten.

Wenn Sie sich schließlich Gedanken darüber machen, Ihre Medien für Fragen des geistigen Eigentums oder der Rechteverwaltung zu schützen, bietet Streaming eine sicherere Videobereitstellung, da die Medien beim Streaming nicht im Cache des Kunden gespeichert werden.

## Videovorgaben

Beim Hochladen eines Videos wählen Sie eine oder mehrere Vorgaben aus, die die Einstellungen zum Konvertieren des Übergeordneten Videos in ein Web-freundliches Format durch Kodierung enthalten. Videovorgaben sind in zwei Varianten verfügbar: &quot;Adaptive Videovorgaben&quot;und &quot;Einzelne Kodierungsvorgaben&quot;.

Siehe [Verfügbare Videovorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

Adaptive Videovorgaben sind standardmäßig aktiviert, d. h. sie sind für die Kodierung verfügbar. Wenn Sie eine einzelne Kodierungsvorgabe verwenden möchten, muss Ihr Administrator sie aktivieren, damit sie in der Liste der Videovorgaben angezeigt wird.

Erfahren Sie, wie Sie [Aktivieren oder Deaktivieren von Videovorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

Sie können eine von vielen vordefinierten Vorgaben auswählen, die mit Dynamic Media Classic geliefert werden, oder Sie können eigene Vorgaben erstellen. Standardmäßig werden jedoch keine Vorgaben für den Upload ausgewählt. Mit anderen Worten: **Wenn Sie beim Hochladen keine Videovorgabe auswählen, wird Ihr Video nicht konvertiert und kann möglicherweise nicht veröffentlicht werden.**. Sie können das Video jedoch selbst offline konvertieren und einfach hochladen und veröffentlichen. Videovorgaben sind nur erforderlich, wenn Dynamic Media Classic die Konvertierung für Sie durchführen soll.

Beim Hochladen wählen Sie eine Videovorgabe aus, indem Sie **Videooptionen** im Bereich &quot;Auftragsoptionen&quot;. Anschließend wählen Sie aus, ob Sie die Kodierung für Computer, Mobilgeräte oder Tablets vornehmen möchten.

- Der Computer ist für den Desktop geeignet. Hier finden Sie in der Regel größere Vorgaben (z. B. HD), die mehr Bandbreite verbrauchen.
- Mobiltelefon und Tablet erstellen MP4-Videos für Geräte wie iPhones und Android™-Smartphones. Der einzige Unterschied zwischen Mobile und Tablet besteht darin, dass die Tablet-Vorgaben in der Regel eine höhere Bandbreite aufweisen, da sie auf der WLAN-Nutzung basieren. Mobile Vorgaben sind für eine langsamere 3G-Nutzung optimiert.

### Fragen, die Sie sich vor Auswahl einer Vorgabe stellen müssen

Bei der Auswahl einer Vorgabe sollten Sie Ihre Zielgruppe und Ihr Quellmaterial kennen. Was wissen Sie über Ihren Kunden? Wie sehen sie sich das Video an - mit einem Computermonitor oder einem Mobilgerät?

Welche Auflösung hat Ihr Video? Wenn Sie eine Vorgabe auswählen, die größer als das Original ist, erhalten Sie möglicherweise ein unscharfes/pixeliertes Video. Es ist in Ordnung, wenn Ihr Video größer als die Vorgabe ist, aber wählen Sie keine Vorgabe, die größer als Ihr Quellvideo ist.

Welches Seitenverhältnis hat es? Wenn Sie um das konvertierte Video schwarze Balken sehen, wählen Sie das falsche Seitenverhältnis aus. Dynamic Media Classic kann diese Einstellungen nicht automatisch erkennen, da die Datei zunächst vor dem Hochladen überprüft werden muss.

### Aufschlüsselung der Videooptionen

Anhand von Videovorgaben wird bestimmt, wie das Video kodiert wird, indem diese Einstellungen festgelegt werden. Wenn Sie mit diesen Begriffen nicht vertraut sind, lesen Sie bitte das Thema Grundlegende Videokonzepte und Terminologie weiter oben.

![image](assets/video-overview/video-overview-3.jpg)

- **Seitenverhältnis.** Standard 4:3 oder Widescreen16:9.
- **Größe.** Dies entspricht der Anzeigeauflösung und wird in Pixel gemessen. Dies hängt mit dem Seitenverhältnis zusammen. Bei einem Verhältnis von 16:9 beträgt ein Video 432 x 240 Pixel, bei 4:3 320 x 240 Pixel.
- **FPS.** Die standardmäßigen Bildraten betragen 30 Frames pro Sekunde, 25 Frames pro Sekunde oder 24 Frames pro Sekunde (fps), je nach Videostandard - NTSC, PAL oder Film. Diese Einstellung spielt keine Rolle, da Dynamic Media Classic immer dieselbe Framerate wie das Quellvideo verwendet.
- **Format.** Das ist MP4.
- **Bandbreite.** Dies ist die gewünschte Verbindungsgeschwindigkeit Ihres Zielbenutzers. Haben sie eine schnelle oder eine langsame Internetverbindung? Verwenden sie normalerweise Desktop-Computer oder Mobilgeräte? Dies hängt auch mit der Auflösung (Größe) zusammen, da je größer das Video ist, desto mehr Bandbreite benötigt wird.

### Bestimmen der Datenrate oder &quot;Bitrate&quot;für Ihr Video

Die Berechnung der Bitrate für Ihr Video ist einer der am wenigsten verständlichen Faktoren für die Bereitstellung von Videos im Internet, aber möglicherweise der wichtigste, da sie sich direkt auf das Benutzererlebnis auswirkt. Wenn Sie Ihre Bitrate zu hoch einstellen, werden Sie eine hohe Videoqualität, aber eine schlechte Leistung haben. Benutzer mit langsameren Internetverbindungen müssen warten, bis das Video bei der Wiedergabe andauernd angehalten wird. Wenn Sie es jedoch zu niedrig einstellen, leidet die Qualität. Innerhalb der Videovorgabe schlägt Dynamic Media Classic je nach Zielbandbreite einen Datenbereich vor. Das ist ein guter Anfang.

Wenn Sie es jedoch selbst herausfinden möchten, benötigen Sie einen Bitratenrechner. Dies ist ein Tool, das häufig von Videoexperten und Enthusiasten verwendet wird, um zu schätzen, wie viele Daten in einen bestimmten Stream oder ein bestimmtes Medienelement passen (z. B. eine DVD).

## Erstellen einer benutzerdefinierten Videovorgabe

Manchmal benötigen Sie eine spezielle Videovorgabe, die nicht mit den Einstellungen der nativen Kodierungs-Videovorgaben übereinstimmt. Dies kann vorkommen, wenn Sie ein benutzerdefiniertes Video einer bestimmten Größe haben, z. B. ein Video, das mit 3D-Animationssoftware erstellt wurde, oder ein Video, das von der Originalgröße abgeschnitten wurde. Vielleicht möchten Sie mit verschiedenen Bandbreiteneinstellungen experimentieren, um Videos mit höherer oder geringerer Qualität bereitzustellen. Erstellen Sie in jedem Fall eine benutzerdefinierte Videovorgabe für die einzelne Kodierung.

### Videovorgaben-Workflow

1. Videovorgaben befinden sich unter **Einrichtung > Anwendungseinstellungen > Videovorgaben**. Hier finden Sie eine Liste aller für Ihr Unternehmen verfügbaren Kodierungsvorgaben.

   - Jedes Streaming-Video-Konto verfügt über Dutzende von Vorgaben, und wenn Sie eigene benutzerdefinierte Vorgaben erstellen, sehen Sie sie auch hier.
   - Über das Dropdown-Menü können Sie nach Typ filtern. Die Vorgaben sind in Computer, Mobil und Tablet unterteilt.
      ![image](assets/video-overview/video-overview-4.jpg)

2. In der Spalte Aktiv können Sie auswählen, ob alle Vorgaben beim Hochladen oder nur die von Ihnen ausgewählten angezeigt werden sollen. Wenn Sie sich in den USA befinden, möchten Sie möglicherweise die europäischen PAL-Vorgaben deaktivieren und in Großbritannien/EMEA die NTSC-Vorgaben deaktivieren.
3. Klicken Sie auf **Hinzufügen** -Schaltfläche, um eine benutzerdefinierte Vorgabe zu erstellen. Dadurch wird der Bereich Videovorgabe hinzufügen geöffnet. Der Prozess hier ähnelt dem Erstellen einer Bildvorgabe.
4. Geben Sie ihr zunächst einen **Vorgabenname** in der Liste der Vorgaben angezeigt. Im obigen Beispiel ist diese Vorgabe für Video-Tutorials zur Bildschirmerfassung vorgesehen.
5. Die **Beschreibung** ist optional, gibt Ihren Benutzern jedoch eine QuickInfo, in der der Zweck dieser Vorgabe beschrieben wird.
6. Die **Dateisuffix kodieren** am Ende des Namens des Videos angehängt, das Sie hier erstellen. Beachten Sie, dass Sie ein Übergeordnetes Video sowie dieses kodierte Video haben, das ein Abbild des Übergeordneten darstellt, und dass keine zwei Assets in Dynamic Media Classic dieselbe Asset-ID haben können.
7. **Wiedergabegerät** ist der Bereich, in dem Sie das gewünschte Videodateiformat auswählen (Computer, Mobil oder Tablet). Beachten Sie, dass Mobile und Tablet dasselbe MP4-Format erzeugen. Dynamic Media Classic muss nur wissen, in welcher Kategorie die Vorgabe platziert werden soll. Der theoretische Unterschied besteht jedoch darin, dass Tablet-Vorgaben in der Regel für eine schnellere Internetverbindung sorgen, da alle WLAN unterstützen.
8. **Target-Datenrate** ist etwas, das Sie für sich selbst herausfinden müssen, aber Sie können einen vorgeschlagenen Bereich auf dem Bild unten sehen. Alternativ können Sie den Schieberegler auf die ungefähre Zielbandbreite ziehen. Verwenden Sie einen Bitratenrechner, um eine genauere Zahl zu erhalten. Es gibt ein wenig Versuch und Fehler.

   ![image](assets/video-overview/video-overview-5.jpg)

9. Quelldatei festlegen **Seitenverhältnis**. Diese Einstellung ist direkt an die unten stehende Größe gebunden. Wenn Sie _Benutzerdefiniert_ müssen Sie die Breite und Höhe manuell eingeben.
10. Wenn Sie ein Seitenverhältnis auswählen, legen Sie einen Wert für **Auflösungsgröße** und Dynamic Media Classic füllt den anderen Wert automatisch aus. Geben Sie jedoch für ein benutzerdefiniertes Seitenverhältnis beide Werte ein. Ihre Größe sollte Ihrer Datenrate entsprechen. Wenn Sie eine niedrige Datenrate und eine große Datenmenge festlegen, erwarten Sie eine schlechte Qualität.
11. Klicken **Speichern** , um die Vorgabe zu speichern. Im Gegensatz zu anderen Vorgaben müssen Sie zu diesem Zeitpunkt keine Veröffentlichung vornehmen, da die Vorgaben nur zum Hochladen von Dateien verwendet werden. Später müssen Sie die kodierten Videos veröffentlichen, aber die Vorgaben dienen nur der internen Dynamic Media Classic-Verwendung.
12. Um zu überprüfen, ob sich die Videovorgabe auf der Upload-Liste befindet, gehen Sie zu **Hochladen**. Auswählen **Auftragsoptionen** und erweitern **Videooptionen**. Ihre Vorgabe wird in der Kategorie für das Wiedergabegerät aufgelistet, das Sie ausgewählt haben (Computer, Mobil oder Tablet).

Weitere Informationen [Hinzufügen oder Bearbeiten von Videovorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## Hinzufügen von Untertiteln zu Videos

Manchmal kann es nützlich sein, Ihrem Video Untertitel hinzuzufügen, z. B. wenn Sie das Video für Betrachter in mehreren Sprachen bereitstellen müssen, aber nicht das Audio in einer anderen Sprache abspielen oder das Video erneut in separaten Sprachen aufzeichnen möchten. Darüber hinaus bietet das Hinzufügen von Untertiteln für Personen mit eingeschränktem Hörvermögen und die Verwendung von Untertiteln eine größere Barrierefreiheit. Dynamic Media Classic erleichtert das Hinzufügen von Untertiteln zu Videos.

Erfahren Sie, wie Sie [Hinzufügen von Untertiteln zu Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-captions-video.html).

## Hinzufügen von Kapitelmarken zu Ihrem Video

Bei langen Videos schätzen Ihre Betrachter wahrscheinlich die Möglichkeiten und den Komfort, die Ihnen durch die Navigation Ihres Videos mit Kapitelmarken geboten werden. Dynamic Media Classic ermöglicht das einfache Hinzufügen von Kapitelmarken zu Videos.

Erfahren Sie, wie Sie [Hinzufügen von Kapitelmarken zu Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## Themen zur Videoimplementierung

### URL veröffentlichen und kopieren

Der letzte Schritt im Dynamic Media Classic-Workflow besteht darin, Ihre Videoinhalte zu veröffentlichen. Das Video verfügt jedoch über einen eigenen Veröffentlichungsauftrag, den so genannten &quot;Video Server Publish&quot;, der unter &quot;Erweitert&quot;zu finden ist.

![image](assets/video-overview/video-overview-6.jpg)

Erfahren Sie, wie Sie [Veröffentlichen des Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

Nachdem Sie eine Videoveröffentlichung ausgeführt haben, können Sie eine URL abrufen, um in einem Webbrowser auf Ihre Videos und alle standardmäßigen Dynamic Media Classic-Viewer-Vorgaben zuzugreifen. Wenn Sie jedoch Ihre eigene Video-Viewer-Vorgabe anpassen oder erstellen, müssen Sie eine separate Veröffentlichung für den Image-Server ausführen.

- Erfahren Sie, wie Sie [Verknüpfen einer URL mit einer mobilen Site oder Website](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- Erfahren Sie, wie Sie [Einbetten des Video-Viewers auf einer Web-Seite](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

Sie können Ihr Video auch mit einem Drittanbieter- oder benutzerdefinierten Videoplayer bereitstellen.

Erfahren Sie, wie Sie [Bereitstellen von Videos mit einem Videoplayer von Drittanbietern](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

Wenn Sie auch die Videominiaturen verwenden möchten - das aus dem Video extrahierte Bild - müssen Sie außerdem eine Veröffentlichung auf dem Image-Server ausführen. Dies liegt daran, dass sich das Miniaturbild für das Video auf dem Image-Server befindet, während sich das Video selbst auf dem Video-Server befindet. Videominiaturen können in Videosuchergebnissen und Videowiedergabelisten verwendet werden und als erster &quot;Posterrahmen&quot;verwendet werden, der im Video-Viewer angezeigt wird, bevor das Video wiedergegeben wird.

Weitere Informationen [Arbeiten mit Videominiaturen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### Auswählen und Anpassen einer Viewer-Vorgabe

Der Prozess zum Auswählen und Anpassen einer Viewer-Vorgabe entspricht dem für Bilder. Sie können entweder eine Vorgabe erstellen oder eine vorhandene Vorgabe ändern und unter einem neuen Namen speichern, Bearbeitungen vornehmen und eine Image Serving-Veröffentlichung ausführen. Alle Viewer-Vorgaben werden auf dem Image-Server veröffentlicht, nicht nur Vorgaben für Bilder. Daher müssen Sie eine Bildveröffentlichung ausführen, um Ihre neuen oder modifizierten Vorgaben anzuzeigen.

>[!TIP]
>
>Führen Sie nach der Veröffentlichung Ihres Video-Servers eine Image Serving-Veröffentlichung aus, um Miniaturbilder zu veröffentlichen, die mit Ihren Videos verknüpft sind.

## Video Search Engine Optimization

Suchmaschinenoptimierung (SEO) ist der Prozess zur Verbesserung der Sichtbarkeit einer Website oder Webseite in Suchmaschinen. Suchmaschinen eignen sich zwar hervorragend für die Erfassung von Informationen über textbasierte Inhalte, können jedoch nur dann Informationen über Videos abrufen, wenn diese Informationen ihnen bereitgestellt werden. Mit Dynamic Media Classic Video SEO können Sie Metadaten verwenden, um Suchmaschinen Beschreibungen Ihrer Videos bereitzustellen. Mit der Video SEO-Funktion können Sie Video-Sitemaps und Media RSS (mRSS)-Feeds erstellen.

- **Video-Sitemap**. Informiert Google genau darüber, wo und was sich der Videoinhalt auf einer Site befindet. Daher können Videos in Google vollständig durchsucht werden. Beispielsweise kann eine Video-Sitemap die Laufzeit und Kategorien von Videos angeben.
- **mRSS-Feed**. Wird von Content-Herausgebern verwendet, um Mediendateien in Yahoo! zu übertragen! Videosuche. Google unterstützt sowohl das Video Sitemap- als auch das Media RSS (mRSS)-Feed-Protokoll zum Übermitteln von Informationen an Suchmaschinen.

Wenn Sie Video-Sitemaps und mRSS-Feeds erstellen, entscheiden Sie, welche Metadatenfelder aus Videodateien eingeschlossen werden sollen. Auf diese Weise beschreiben Sie Ihre Videos für Suchmaschinen, damit Suchmaschinen den Traffic präziser zu Videos auf Ihrer Website leiten können.

Nachdem die Sitemap oder der Feed erstellt wurde, können Sie Dynamic Media Classic automatisch veröffentlichen lassen, sie manuell veröffentlichen oder einfach eine Datei generieren lassen, die Sie später bearbeiten können. Außerdem kann Dynamic Media Classic diese Datei automatisch täglich generieren und veröffentlichen.

Am Ende des Prozesses senden Sie die Datei oder URL an Ihre Suchmaschine. Diese Aufgabe erfolgt außerhalb von Dynamic Media Classic. Wir diskutieren es jedoch kurz unten.

### Anforderungen für Sitemap-/mRSS-Dateien

Damit Google und andere Suchmaschinen Ihre Dateien nicht zurückweisen können, müssen sie das richtige Format aufweisen und bestimmte Informationen enthalten. Dynamic Media Classic generiert eine ordnungsgemäß formatierte Datei. Wenn die Informationen jedoch für einige Ihrer Videos nicht verfügbar sind, sind sie nicht in der Datei enthalten.

Die erforderlichen Felder sind Landingpage (die URL für die Seite, die das Video bereitstellt, nicht die URL für das Video selbst), Titel und Beschreibung. Jedes Video muss einen Eintrag für diese Elemente enthalten, oder es wird nicht in die generierte Datei aufgenommen. Optionale Felder sind Tags und Kategorie.

Es gibt zwei weitere erforderliche Felder: Inhalts-URL, URL zum Video-Asset selbst und Miniaturansicht, URL zu einem Miniaturbild des Videos. Dynamic Media Classic füllt diese Werte jedoch automatisch für Sie aus.

Der empfohlene Arbeitsablauf besteht darin, diese Daten vor dem Hochladen mit XMP Metadaten in Ihre Videos einzubetten. Dynamic Media Classic extrahiert sie beim Hochladen. Sie würden eine Anwendung wie Adobe Bridge verwenden, die in allen Adobe Creative Cloud-Anwendungen enthalten ist, um die Daten in Standard-Metadatenfelder zu füllen.

Nach dieser Methode müssen Sie diese Daten nicht manuell mit Dynamic Media Classic eingeben. Sie können jedoch auch Metadatenvorgaben in Dynamic Media Classic verwenden, um schnell und einfach dieselben Daten jedes Mal eingeben zu können.

Weitere Informationen zu diesem Thema finden Sie unter [Anzeigen, Hinzufügen und Exportieren von Metadaten](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![image](assets/video-overview/video-overview-7.jpg)

Nachdem die Metadaten ausgefüllt wurden, können Sie sie in der Detailansicht für dieses Video-Asset sehen. Keywords sind möglicherweise ebenfalls vorhanden, befinden sich jedoch auf der Registerkarte Keywords .

- Weitere Informationen [Hinzufügen von Suchbegriffen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- Weitere Informationen [Video SEO](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- Informationen zu [Einstellungen für Video SEO](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### Einrichten von Video SEO

Das Einrichten von Video SEO beginnt mit der Auswahl des gewünschten Formats, der Generierungsmethode und der Metadatenfelder, die in die Datei aufgenommen werden sollen.

1. Navigieren Sie zu **Einrichtung > Anwendungseinstellungen > Video SEO > Einstellungen**.
2. Im **Generierungsmodus** wählen Sie ein Dateiformat aus. Die Standardeinstellung ist Aus. Wählen Sie daher zur Aktivierung entweder Video-Sitemap, mRSS oder beides aus.
3. Wählen Sie aus, ob automatisch oder manuell generiert werden soll. Zur Vereinfachung empfehlen wir, **Automatischer Modus**. Wenn Sie &quot;Automatisch&quot;auswählen, legen Sie auch die **Zur Veröffentlichung markieren** oder die Datei(en) nicht live geschaltet werden. Die Sitemap- und RSS-Dateien sind Typen eines XML-Dokuments und müssen wie jedes andere Asset veröffentlicht werden. Verwenden Sie einen der manuellen Modi, wenn Sie nicht alle Informationen zur Verfügung haben oder nur eine einmalige Generierung durchführen möchten.
4. Füllen Sie die Metadaten-Tags, die in den Dateien verwendet werden. Dieser Schritt ist nicht optional. Sie müssen mindestens die drei mit einem Sternchen (\*) markierten Felder einschließen: **Landingpage** , **Titel** und **Beschreibung**. Um die Metadaten für diese Aufgaben zu verwenden, ziehen Sie die Felder aus dem Metadatenbedienfeld auf der rechten Seite in ein entsprechendes Feld im Formular. Dynamic Media Classic füllt das Platzhalterfeld automatisch mit den tatsächlichen Daten aus jedem Video. Sie müssen keine Metadatenfelder verwenden. Sie können hier stattdessen statischen Text eingeben, der jedoch für jedes Video angezeigt wird.
5. Nachdem Sie Informationen in die drei erforderlichen Felder eingegeben haben, aktiviert Dynamic Media Classic die **Speichern** und **Speichern und generieren** Schaltflächen. Klicken Sie auf eins, um Ihre Einstellungen zu speichern. Verwendung **Speichern** Wenn Sie sich im automatischen Modus befinden und möchten, dass Dynamic Media Classic die Dateien später generiert. Verwendung **Speichern und generieren** , um die Datei sofort zu erstellen.

### Testen und Veröffentlichen der Video-Sitemap, des mRSS-Feeds oder beider Dateien

Die generierten Dateien werden im Stammverzeichnis (Basisverzeichnis) Ihres Kontos angezeigt.

![image](assets/video-overview/video-overview-8.jpg)

Diese Dateien müssen veröffentlicht werden, da das Video SEO-Tool eine Veröffentlichung nicht allein ausführen kann. Solange sie zur Veröffentlichung markiert sind, werden sie beim nächsten Ausführen einer Veröffentlichung an die Veröffentlichungsserver gesendet.

Nach der Veröffentlichung sind Ihre Dateien mit diesem URL-Format verfügbar.

![image](assets/video-overview/video-url-format.png)

Beispiel:

![image](assets/video-overview/video-format-example.png)

### Übermitteln an Suchmaschinen

Der letzte Schritt des Prozesses besteht darin, Ihre Dateien und/oder URLs an Suchmaschinen zu senden. Dynamic Media Classic kann diesen Schritt nicht für Sie durchführen. Wenn Sie jedoch die URL und nicht die XML-Datei selbst senden, sollte der Feed aktualisiert werden, wenn Ihre Datei das nächste Mal generiert und eine Veröffentlichung erfolgt.

Die Methode zum Senden an Ihre Suchmaschine variiert. Für Google verwenden Sie jedoch Google Webmaster Tools. Gehen Sie dort einmal zu **Site-Konfiguration > Sitemaps** und klicken Sie auf **Senden einer Sitemap** Schaltfläche. Hier können Sie die Dynamic Media Classic-URL in Ihre SEO-Datei(en) einfügen.

### Video SEO-Bericht

Dynamic Media Classic stellt einen Bericht bereit, der anzeigt, wie viele Videos erfolgreich in die Dateien aufgenommen wurden, was noch wichtiger ist, welche nicht aufgrund von Fehlern enthalten waren. Um auf den Bericht zuzugreifen, navigieren Sie zu **Einrichtung > Anwendungseinstellungen > Video SEO > Bericht**.

![image](assets/video-overview/video-overview-9.jpg)

## Mobile Implementierung für MP4-Video

Dynamic Media Classic enthält keine Viewer-Vorgaben für Mobilgeräte, da Viewer nicht erforderlich sind, um Videos auf unterstützten Mobilgeräten wiederzugeben. Solange Sie das H.264 MP4-Format kodieren - entweder durch Konvertierung beim Hochladen oder durch Vorkodierung auf Ihrem Desktop - können unterstützte Tablets und Smartphones Ihre Videos wiedergeben, ohne einen Viewer zu benötigen. Dies wird auf Android- und iOS-Geräten (iPhone und iPad) unterstützt.

Der Grund, warum kein Viewer erforderlich ist, ist, dass beide Plattformen native H.264-Unterstützung haben. Sie können das Video entweder in eine HTML5-Webseite einbetten oder in die Anwendung selbst einbetten. Die Betriebssysteme Android und iOS bieten einen Controller, mit dem das Video wiedergegeben werden kann.

Daher gibt Ihnen Dynamic Media Classic keine URL für einen Viewer für Mobilgeräte, sondern eine URL direkt für das Video. Im Vorschaufenster für ein MP4-Video finden Sie Links für Desktop und Mobile. Die Mobil-URL verweist auf das veröffentlichte Video.

Wichtig zum Hinweis auf veröffentlichte Videos ist, dass die URL den vollständigen Pfad zum Video auflistet, nicht nur die Asset-ID. Beim Umgang mit Bildern rufen Sie das Bild unabhängig von der Ordnerstruktur anhand der Asset-ID auf. Für Videos müssen Sie jedoch auch die Ordnerstruktur angeben. In den oben genannten URLs wird das Video im Pfad gespeichert:

![image](assets/video-overview/mobile-implement-1.png)

Dies kann auch als Firmenname/Ordnerpfad/Videoname ausgedrückt werden.

### Methode 1: Browserwiedergabe - HTML5-Code

Verwenden Sie das HTML5-Video-Tag , um Ihr MP4-Video in eine Webseite einzubetten.

![image](assets/video-overview/browser-playback.png)

Diese Methode funktioniert auch für das Desktop-Web, kann jedoch bei der Browserunterstützung zu Problemen führen. Nicht alle Desktop-Webbrowser unterstützen nativ H.264-Videos, einschließlich Firefox.

### Methode 2: App-Wiedergabe in iOS - Media Player Framework

Alternativ können Sie das Dynamic Media Classic MP4-Video in den Code Ihrer Mobile App einbetten. Im Folgenden finden Sie ein allgemeines Beispiel für iOS, das das Media Player-Framework verwendet, das nur zu Veranschaulichungszwecken bereitgestellt wird:

![image](assets/video-overview/app-playback.png)

## Zusätzliche Ressourcen

Beobachten Sie die [Dynamic Media Skill Builder: Video in Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) On-Demand-Webinar zur Verwendung der Videofunktionen in Dynamic Media Classic.
