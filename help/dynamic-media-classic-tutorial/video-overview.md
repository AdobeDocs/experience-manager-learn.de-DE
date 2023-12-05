---
title: Videoüberblick
description: Dynamic Media Classic bietet eine automatische Konvertierung von Videos beim Hochladen, Video-Streaming auf Desktop- und Mobilgeräte sowie adaptive Video-Sets, die für die Wiedergabe je nach Gerät und Bandbreite optimiert sind. Erfahren Sie mehr über Videos in Dynamic Media Classic inklusive einer Einführung in Videokonzepte und -terminologie. Machen Sie sich anschließend damit vertraut, wie Sie Videos hochladen und kodieren, Videovorgaben für das Hochladen auswählen, eine Videovorgabe hinzufügen oder bearbeiten, Videos in einem Video-Viewer in der Vorschau anzeigen, Videos auf Web- und mobilen Sites bereitstellen, Untertitel und Kapitelmarken zu Videos hinzufügen und Video-Viewer für Desktop- und Mobilbenutzende veröffentlichen.
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: dfbf316f-3922-4bc7-b3f3-2a5bbdeb7063
duration: 1587
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '6032'
ht-degree: 100%

---

# Videoüberblick {#video-overview}

Dynamic Media Classic bietet eine automatische Konvertierung von Videos beim Hochladen, Video-Streaming auf Desktop- und Mobilgeräte sowie adaptive Video-Sets, die für die Wiedergabe je nach Gerät und Bandbreite optimiert sind. Eines der wichtigsten Details bezüglich Videos ist, dass der Workflow einfach ist – er wurde so entwickelt, dass alle ihn verwenden können, auch Benutzende, die nicht mit der Videotechnologie vertraut sind.

Am Ende dieses Tutorial-Abschnitts wissen Sie, wie Sie folgende Vorgänge durchführen:

- Hochladen und Kodieren (Transkodieren) von Videos in unterschiedlichen Größen und Formaten
- Treffen Sie Ihre Auswahl aus den verfügbaren Videovorgaben für das Hochladen
- Hinzufügen oder Bearbeiten von Videokodierungsvorgaben
- Vorschau von Videos in einem Video-Viewer
- Bereitstellen von Videos auf Web- und mobilen Sites
- Hinzufügen von Untertiteln und Kapitelmarken zu Videos
- Anpassen und Veröffentlichen von Video-Viewern für Desktop- und Mobilbenutzende

>[!NOTE]
>
>Alle URLs in diesem Kapitel dienen nur zur Veranschaulichung. Es handelt sich nicht um Livelinks.

## Überblick über Videos und Dynamic Media Classic

Zunächst einiges zu den Videonutzungsmöglichkeiten mit Dynamic Media Classic.

### Funktionen und Möglichkeiten

Die Dynamic Media Classic-Videoplattform bietet alle Teile einer Videolösung: den Upload, die Konvertierung und die Verwaltung von Videos und die Möglichkeiten, einem Video Untertitel und Kapitelmarken hinzuzufügen sowie Vorgaben zur einfachen Wiedergabe zu verwenden.

Dadurch ist es einfach, hochwertige adaptive Videos für das Streaming auf mehreren Bildschirmen zu veröffentlichen, einschließlich Desktop-, iOS-, Android™-, BlackBerry®- und Windows-Mobilgeräten. Ein adaptives Video-Set umfasst Versionen desselben Videos, die mit unterschiedlichen Bit-Raten und Formaten codiert wurden, wie 400 kBit/s, 800 kBit/s und 1000 kBit/s. Der Desktop-Computer oder das Mobilgerät erkennt die verfügbare Bandbreite.

Außerdem wird die Videoqualität automatisch geändert, wenn sich die Netzwerkbedingungen am Desktop oder Mobilgerät ändern. Wenn eine Kundin oder ein Kunde an einem Desktop-Computer in den Vollbildmodus wechselt, verwendet das adaptive Video-Set eine höhere Auflösung und sorgt so für ein besseres Wiedergabeerlebnis. Adaptive Video-Sets bieten die bestmögliche Wiedergabe für Kundschaft, die Dynamic Media Classic-Videos auf unterschiedlichen Bildschirmen und Geräten wiedergibt.

### Video-Management

Das Arbeiten mit Videos kann komplexer sein als das Arbeiten mit bewegungslosen digitalen Bildern. Bei Videos haben Sie es mit zahlreichen Formaten und Standards und der Frage zu tun, ob Ihre Zielgruppe Ihre Clips wiedergeben kann. Dynamic Media Classic erleichtert die Arbeit mit Videos und bietet viele leistungsstarke Tools im Hintergrund, wobei die Arbeit mit diesen unkompliziert ist.

Dynamic Media Classic erkennt viele verschiedene verfügbare Quellformate und kann mit ihnen arbeiten. Das Lesen des Videos ist jedoch nur ein Teil der Arbeit – Sie müssen das Video auch in ein Web-freundliches Format konvertieren. Dynamic Media Classic übernimmt diese Aufgabe, indem es Ihnen ermöglicht, Videos in H.264-Videos zu konvertieren.

Die Konvertierung des Videos selbst kann mit den vielen verfügbaren professionellen und Hobby-Tools kompliziert werden. Dynamic Media Classic hält es einfach, indem es einfache Vorgaben anbietet, die für unterschiedliche Qualitätseinstellungen optimiert sind. Wenn Sie jedoch etwas Individuelles wünschen, können Sie auch Ihre eigenen Voreinstellungen erstellen.

Wenn Sie viele Videos haben, werden Sie die Möglichkeit schätzen, alle Ihre Assets zusammen mit Ihren Bildern und anderen Medien in Dynamic Media Classic zu verwalten. Sie können Ihre Assets, einschließlich Video-Assets, mit robuster XMP-Metadatenunterstützung organisieren, katalogisieren und durchsuchen.

### Videowiedergabe

Ähnlich wie das Problem, Videos zu konvertieren, um sie Web-freundlich und barrierefrei zu machen, besteht auch das Problem bei der Implementierung und Bereitstellung von Videos auf Ihrer Site. Die Entscheidung, einen Player zu kaufen oder selbst zu bauen, ihn für verschiedene Geräte und Bildschirme kompatibel zu machen und ihn dann zu warten, kann eine Vollzeitbeschäftigung sein.

Auch hier besteht der Ansatz von Dynamic Media Classic darin, Ihnen zu ermöglichen, die Vorgabe und den Viewer auszuwählen, die Ihren Anforderungen entsprechen. Sie haben viele verschiedene Viewer-Optionen und eine Bibliothek mit zahlreichen Vorgaben zur Verfügung.

Sie können Videos einfach an Web- und Mobilgeräte senden, da Dynamic Media Classic HTML5-Videos unterstützt. So können Sie Benutzende, die verschiedene Browser ausführen, sowie Benutzende der Android™- und iOS-Plattform als Ziel auswählen. Streaming-Video ermöglicht eine reibungslose Wiedergabe von längeren oder hochauflösenden Inhalten, während progressive HTML5-Videos über Vorgaben verfügen, die für kleine Bildschirme optimiert sind.

Viewer-Vorgaben für Videos können je nach Viewer-Typ teilweise konfiguriert werden.

Wie alle Viewer erfolgt die Integration über eine einzelne Dynamic Media Classic-URL pro Viewer oder Video.

>[!NOTE]
>
>Verwenden Sie als Best Practice Dynamic Media Classic HTML5-Video-Viewer. Die in HTML5 Video-Viewern verwendeten Vorgaben sind robuste Video-Player. Durch die Kombination von HTML5- und CSS-Wiedergabekomponenten, eingebetteter Wiedergabe und adaptivem und progressivem Streaming je nach Browser-Funktionalität in einem einzigen Player können Sie die Reichweite Ihrer Rich-Media-Inhalte auf Desktop-, Tablet- und mobile Benutzende ausweiten und ein optimiertes Videoerlebnis gewährleisten.

Ein letzter Hinweis zu Dynamic Media Classic-Videos, die für einige Kundinnen und Kunden gelten können: Nicht alle Unternehmen haben die automatische Konvertierung, das Streaming oder die Videovorgaben für ihr Konto aktiviert. Wenn Sie aus irgendeinem Grund nicht auf die URLs für Streaming-Videos zugreifen können, kann dies der Grund sein. Sie können progressiv heruntergeladene Videos hochladen und veröffentlichen und Zugriff auf alle Video-Viewer haben. Um jedoch die gesamten Dynamic Media Classic-Videokompetenzen nutzen zu können, wenden Sie sich an Ihre Kundenbetreuung oder Vertriebsmitarbeitende, um diese Funktionen zu aktivieren.

Weitere Informationen finden Sie unter [Video in Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/quick-start-video.html?lang=de).

## Video 101

### Grundlegende Videokonzepte und Terminologie

Bevor wir anfangen, besprechen wir einige Begriffe, mit denen Sie vertraut sein sollten, um mit Videos zu arbeiten. Diese Konzepte sind nicht spezifisch für Dynamic Media Classic, und wenn Sie Videos für eine professionelle Website verwalten wollen, sollten Sie sich in diesem Bereich weiterbilden. Wir empfehlen einige Ressourcen am Ende dieses Abschnitts.

- **Codierung/Transcodierung.** Bei der Codierung wird die Videokomprimierung angewendet, um unkomprimierte Rohdaten in ein Format zu konvertieren, das die Arbeit erleichtert. Die Transkodierung bezieht sich auf die Konvertierung von einer Codierungsmethode in eine andere.

   - Master-Videodateien, die mit einer Videobearbeitungs-Software erstellt wurden, sind häufig zu groß und nicht im richtigen Format für die Bereitstellung an Online-Zielorte. Sie werden normalerweise für die schnelle Wiedergabe auf dem Desktop und für die Bearbeitung codiert, nicht aber für die Bereitstellung über das Internet.
   - Um digitale Videos in das richtige Format und die richtigen Spezifikationen für die Wiedergabe auf verschiedenen Bildschirmen zu konvertieren, werden die Videodateien in eine kleinere, effiziente Dateigröße transcodiert, die für die Bereitstellung im Internet und auf Mobilgeräten optimal ist.

- **Videokomprimierung.** Die Verringerung der Datenmenge, die zur Darstellung digitaler Videobilder verwendet wird, ist eine Kombination aus räumlicher Bildkomprimierung und zeitlicher Bewegungskompensation.

   - Die meisten Komprimierungstechniken sind verlustbehaftet, d. h. sie geben Daten aus, um eine kleinere Größe zu erreichen.
   - Beispielsweise ist DV-Video komprimiert relativ klein und ermöglicht Ihnen, das Quellmaterial einfach zu bearbeiten, aber es ist viel zu groß, um es über das Web zu verwenden oder sogar auf eine DVD zu kopieren.

- **Dateiformate.** Das Format ist ein Container, der einer ZIP-Datei ähnelt und bestimmt, wie Dateien in der Videodatei organisiert sind, normalerweise jedoch nicht, wie sie codiert sind.

   - Zu den gebräuchlichen Dateiformaten für Quellvideos gehören unter anderem Windows Media (WMV), QuickTime (MOV), Microsoft® AVI und MPEG. Das von Dynamic Media Classic veröffentlichte Format ist MP4.
   - Eine Videodatei enthält in der Regel mehrere Spuren: einen Video-Track (ohne Audio) und eine oder mehrere Audiospuren (ohne Video), die miteinander verknüpft und synchronisiert sind.
   - Das Videodateiformat bestimmt, wie diese verschiedenen Datenspuren und Metadaten organisiert werden.

- **Codec.** Ein Video-Codec beschreibt den Algorithmus, mit dem ein Video mithilfe der Komprimierung codiert wird. Audio wird auch über einen Audio-Codec codiert.

   - Codecs minimieren die Menge an Informationen, die zum Abspielen von Videos erforderlich sind. Statt Informationen zu jedem einzelnen Frame werden nur Informationen zu den Unterschieden zwischen einem Frame und dem nächsten gespeichert.
   - Da sich die meisten Videos kaum zwischen den einzelnen Frames ändern, ermöglichen Codecs hohe Komprimierungsraten, was zu kleineren Dateigrößen führt.
   - Ein Video-Player dekodiert das Video entsprechend seinem Codec und zeigt dann eine Reihe von Bildern – oder Frames – auf dem Bildschirm an.
   - Zu den gängigen Video-Codecs gehören H.264, On2 VP6 und H.263.

![Bild](assets/video-overview/bird-video.png)

- **Auflösung.** Die Höhe und Breite des Videos in Pixel.

   - Die Größe des Quellvideos wird durch die Kamera und die Ausgabe aus der Bearbeitungs-Software bestimmt. Eine HD-Kamera erzeugt ein Video mit einer hohen Auflösung von 1920 x 1080. Um jedoch eine reibungslose Wiedergabe im Web zu gewährleisten, sollten Sie die Kamera auf eine kleinere Auflösung wie 1280 x 720, 640 x 480 oder weniger komprimieren (anpassen).
   - Die Auflösung wirkt sich direkt auf die Dateigröße und die Bandbreite aus, die für die Wiedergabe des Videos erforderlich sind.

- **Anzeige-Seitenverhältnis.** Das Verhältnis zwischen Breite und Höhe eines Videos. Wenn das Seitenverhältnis des Videos nicht mit dem Seitenverhältnis des Players übereinstimmt, werden möglicherweise „schwarze Balken“ oder ein leerer Bereich angezeigt. Bei der Anzeige von Videos kommen zwei gängige Seitenverhältnisse zum Einsatz:

   - 4:3 (1.33:1). Wird für fast alle SD-TV-Inhalte verwendet.
   - 16:9 (1.78:1). Wird für fast alle Breitbild-, HDTV-Inhalte und Filme verwendet.

- **Bitrate/Datenrate.** Die kodierte Menge an Daten für eine Videowiedergabe von einer einzigen Sekunde (in Kilobits pro Sekunde).

   - Im Allgemeinen gilt: Je niedriger die Bitrate, desto erstrebenswerter ist sie für das Web, da sie schneller heruntergeladen werden kann. Dies kann aber auch bedeuten, dass die Qualität aufgrund von Komprimierungsverlusten gering ist.
   - Ein guter Codec sollte eine Balance zwischen niedriger Bitrate und guter Qualität schaffen.

- **Framerate (Frames pro Sekunde oder FPS).** Die Anzahl der Frames oder Standbilder für jede Sekunde eines Videos. In der Regel wird nordamerikanisches Fernsehen (NTSC) mit 29,97 FPS ausgestrahlt, europäisches sowie asiatisches Fernsehen (PAL) mit 25 FPS und (analoger und digitaler) Film mit 24 (23,976) FPS.

   - Noch verwirrender wird es dadurch, dass es auch noch progressive Frames und Interlaced-Frames gibt. Jeder progressive Frame enthält einen kompletten Bild-Frame, während Interlaced-Frames jede zweite Pixelzeile in einem Bild-Frame umfassen. Die Frames werden dann schnell wiedergegeben und scheinen miteinander zu verschmelzen. Film verwendet eine progressive Scan-Methode, während digitale Videos normalerweise verschachtelt („interlaced“) sind.
   - Im Allgemeinen spielt es keine Rolle, ob Ihr Quellfilmmaterial interlaced ist oder nicht – Dynamic Media Classic behält die Scan-Methode im konvertierten Video bei.
   - Streaming/progressive Bereitstellung. Beim Video-Streaming handelt es sich um das Senden von Medien in einem kontinuierlichen Stream, der beim Eintreffen wiedergegeben werden kann, während das progressiv heruntergeladene Video wie jede andere Datei von einem Server heruntergeladen und lokal in Ihrem Browser zwischengespeichert wird.

Hoffentlich hilft Ihnen diese Einführung, die verschiedenen Optionen bei der Verwendung von Dynamic Media Classic-Videos zu verstehen.

## Video-Workflow

Beim Arbeiten mit Videos in Dynamic Media Classic folgen Sie einem grundlegenden Workflow, der der Arbeit mit Bildern ähnelt.

![Bild](assets/video-overview/video-overview-2.png)

1. Laden Sie zunächst die Videodateien nach Dynamic Media Classic hoch. Öffnen Sie dazu unten im Dynamic Media Classic-Erweiterungsbedienfeld das **Tools-Menü** und wählen Sie **Nach Dynamic Media Classic hochladen > Dateien in Ordnernamen** oder **Nach Dynamic Media Classic hochladen > Ordner in Ordnernamen**. „Ordnername“ ist dabei der Ordner, den Sie aktuell mit der Erweiterung anzeigen. Videodateien können groß sein. Daher wird empfohlen, FTP zum Hochladen großer Dateien zu verwenden. Wählen Sie im Rahmen des Uploads eine oder mehrere Videovorgaben für die Videokodierung aus. Das Video kann beim Hochladen in das MP4-Video transkodiert werden. Weitere Informationen zum Verwenden und Erstellen von Kodierungsvorgaben finden Sie unten im Thema „Videovorgaben“. Zusätzliche Details finden Sie unter [Hochladen und Kodieren von Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html?lang=de).
2. Wählen Sie eine Video-Viewer-Vorgabe aus oder wählen Sie eine aus und ändern diese, und zeigen Sie das Video in einer Vorschau an. Wählen Sie dazu entweder eine vorkonfigurierte Viewer-Vorgabe aus oder passen Sie eine eigene an. Wenn Sie Mobile-Benutzende ansprechen, müssen Sie hier nichts tun, da Mobile-Plattformen weder Viewer noch Vorgaben benötigen. Weitere Informationen finden Sie unter [Vorschau von Videos in einem Video-Viewer](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html?lang=de) und [Hinzufügen oder Bearbeiten von Video-Viewer-Vorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html?lang=de#adding-or-editing-a-video-viewer-preset).
3. Führen Sie eine Videoveröffentlichung aus, rufen Sie die URL ab und integrieren Sie sie. Der Hauptunterschied zwischen diesem Schritt für den Video-Workflow und dem Bild-Workflow besteht darin, dass Sie eine spezielle Videoveröffentlichung anstelle (oder vielleicht plus) der standardmäßigen Veröffentlichung zur Bildbereitstellung ausführen. Die Video-Viewer-Integration auf dem Desktop funktioniert genau wie die Bild-Viewer-Integration. Bei Mobilgeräten ist sie jedoch noch einfacher. Sie benötigen lediglich die URL zum Video selbst.

### Über Transcodierung

Die Transcodierung wurde früher als der Konvertierungsprozess von einer Codierungsmethode zu einer anderen definiert. Bei Dynamic Media Classic wird das Quellvideo aus dem aktuellen Format in das MP4-Format konvertiert. Dies ist erforderlich, bevor Ihr Video im Desktop-Browser oder auf einem Mobilgerät angezeigt wird.

Dynamic Media Classic kann die Transcodierung für Sie durchführen – ein großer Vorteil. Sie können das Video selbst transcodieren und die Dateien hochladen, die bereits in MP4 konvertiert wurden. Dies kann jedoch ein komplexer Prozess sein, der eine hoch entwickelte Software erfordert. Wenn Sie nicht wissen, was Sie tun, werden Sie in der Regel beim ersten Versuch keine guten Ergebnisse erzielen.

Dynamic Media Classic konvertiert nicht nur die Dateien für Sie, sondern erleichtert auch die Bereitstellung benutzerfreundlicher Vorgaben. Sie müssen wirklich nicht viel über die technische Seite dieses Prozesses wissen – alles, was Sie wissen sollten, ist ungefähr die endgültige(n) Größe(en), die Sie aus dem System holen möchten, und ein ungefähres Verständnis für die Bandbreite, die Ihre Endnutzenden haben.

Die vorgefertigten Vorgaben sind zwar praktisch und decken die meisten Bedürfnisse ab, aber manchmal möchte man etwas Individuelles. In diesem Fall können Sie eine eigene Codierungsvorgabe erstellen. In Dynamic Media Classic wird eine Codierungsvorgabe als Videovorgabe bezeichnet. Dies wird weiter unten in diesem Kapitel erläutert.

### Über Streaming

Eine weitere wichtige Funktion ist das Video-Streaming, eine Standardfunktion der Dynamic Media Classic-Videoplattform. Streaming-Medien werden ständig von Endnutzenden empfangen und ihnen präsentiert, während sie übertragen werden. Dies ist aus mehreren Gründen von Bedeutung und wünschenswert.

Streaming erfordert in der Regel weniger Bandbreite als progressiver Download, da nur der Teil des Videos bereitgestellt wird, der angesehen wird. Der Dynamic Media Classic-Video-Streaming-Server und die Viewer verwenden die automatische Bandbreitenerkennung, um den bestmöglichen Stream für die Internetverbindung von Benutzenden bereitzustellen.

Beim Streaming beginnt die Videowiedergabe früher als mit anderen Methoden. Außerdem werden Netzwerkressourcen effizienter genutzt, da nur die angezeigten Teile des Videos an den Client gesendet werden.

Die andere Versandmethode ist der progressive Download. Im Vergleich zum Video-Streaming hat der progressive Download nur einen einzigen Vorteil: Sie brauchen keinen Streaming-Server, um das Video zu übertragen. Und genau hier kommt Dynamic Media Classic ins Spiel: Dynamic Media Classic hat einen Streaming-Server in die Plattform integriert, sodass Sie sich den Ärger und die zusätzlichen Kosten für die Wartung dieser speziellen Hardware sparen können.

Progressive Download-Videos können von jedem normalen Web-Server bereitgestellt werden. Dies kann praktisch und potenziell kostengünstig sein. Beachten Sie jedoch, dass progressive Downloads nur über eingeschränkte Such- und Navigationsfunktionen verfügen und Benutzende auf Ihre Inhalte zugreifen und diese wiederverwenden können. In einigen Situationen, wie z. B. der Wiedergabe hinter strikten Netzwerk-Firewalls, kann die Streaming-Bereitstellung blockiert werden. In diesen Fällen kann es wünschenswert sein, die Wiedergabe auf progressive Bereitstellungen zurückzusetzen.

Der progressive Download ist eine gute Wahl für Hobbyisten oder Websites mit geringem Datenverkehr, wenn es ihnen nichts ausmacht, dass ihre Inhalte auf dem Computer einer Benutzerin bzw. eines Benutzers zwischengespeichert werden, wenn sie nur Videos von kürzerer Länge (unter 10 Minuten) bereitstellen müssen oder wenn ihre Besuchenden aus irgendeinem Grund kein Streaming-Video empfangen können.

Sie müssen Ihre Videos streamen, wenn Sie erweiterte Funktionen und Kontrolle über die Videobereitstellung benötigen und/oder wenn Sie Videos für ein größeres Publikum (z. B. mehrere 100 gleichzeitige Zuschauende) anzeigen, Nutzungs- oder Anzeigestatistiken verfolgen und auswerten oder Ihren Zuschauenden das beste interaktive Wiedergabeerlebnis bieten möchten.

Wenn Sie Ihre Medien aus Gründen des geistigen Eigentums oder der Rechteverwaltung schützen möchten, bietet Streaming eine sicherere Bereitstellung von Videos, da die Medien beim Streaming nicht im Cache des Clients gespeichert werden.

## Videovorgaben

Beim Hochladen eines Videos wählen Sie eine oder mehrere Vorgaben aus, die die Einstellungen zum Konvertieren des Master-Videos in ein Web-freundliches Format durch Codierung enthalten. Videovorgaben sind in zwei Varianten verfügbar: „Adaptive Videovorgaben“ und „Einzelne Codierungsvorgaben“.

Siehe [Verfügbare Videovorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=de#video-presets-for-encoding-video-files).

Adaptive Videovorgaben sind standardmäßig aktiviert, d. h. sie sind für die Codierung verfügbar. Wenn Sie eine einzelne Codierungsvorgabe verwenden möchten, muss Ihr Admin sie aktivieren, damit sie in der Liste der Videovorgaben angezeigt wird.

Erfahren Sie, wie Sie [Videovorgaben aktivieren oder deaktivieren](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html?lang=de#activating-or-deactivating-video-encoding-presets).

Sie können entweder eine von vielen vordefinierten Vorgaben aus Dynamic Media Classic auswählen oder eigene Vorgaben erstellen. Standardmäßig werden jedoch keine Vorgaben für den Upload ausgewählt. Mit anderen Worten: **Wenn Sie beim Hochladen keine Videovorgabe auswählen, wird Ihr Video nicht konvertiert und kann möglicherweise nicht veröffentlicht werden.**. Sie können das Video jedoch selbst offline konvertieren und einfach hochladen und veröffentlichen. Videovorgaben sind nur erforderlich, wenn Dynamic Media Classic die Konvertierung für Sie durchführen soll.

Beim Hochladen wählen Sie eine Videovorgabe aus, indem Sie **Videooptionen** im Bedienfeld „Auftragsoptionen“ wählen. Anschließend wählen Sie aus, ob Sie die Codierung für Computer, Mobilgeräte oder Tablets vornehmen möchten.

- Der Computer ist für den Desktopgebrauch bestimmt. Hier finden Sie in der Regel größere Vorgaben (z. B. HD), die mehr Bandbreite verbrauchen.
- Mobil und Tablet erstellen MP4-Videos für Geräte wie iPhones und Android™-Smartphones. Der einzige Unterschied zwischen Mobil und Tablet besteht darin, dass die Tablet-Vorgaben in der Regel eine höhere Bandbreite aufweisen, da sie auf der WLAN-Nutzung basieren. Mobile Vorgaben sind für eine langsamere 3G-Nutzung optimiert.

### Fragen, die Sie sich stellen sollten, bevor Sie eine Vorgabe wählen

Bei der Auswahl einer Vorgabe sollten Sie Ihre Zielgruppe und Ihr Quellmaterial kennen. Was wissen Sie über Ihre Kundschaft? Wie sieht sie sich das Video an – am Computer-Monitor oder Mobilgerät?

Welche Auflösung hat Ihr Video? Wenn Sie eine Vorgabe auswählen, die größer als das Original ist, erhalten Sie möglicherweise ein unscharfes/verpixeltes Video. Es ist in Ordnung, wenn Ihr Video größer als die Vorgabe ist, aber wählen Sie keine Vorgabe, die größer als Ihr Quellvideo ist.

Welches Seitenverhältnis hat es? Wenn Sie um das konvertierte Video schwarze Balken sehen, wählen Sie das falsche Seitenverhältnis. Dynamic Media Classic kann diese Einstellungen nicht automatisch erkennen, da dazu die Datei vor dem Hochladen überprüft werden müsste.

### Aufschlüsselung der Videooptionen

Anhand von Videovorgaben wird bestimmt, wie das Video kodiert wird, indem diese Einstellungen festgelegt werden. Wenn Sie mit diesen Begriffen nicht vertraut sind, lesen Sie bitte die Informationen zu grundlegenden Videokonzepten und -terminologie weiter oben.

![Bild](assets/video-overview/video-overview-3.jpg)

- **Seitenverhältnis.** Standard 4:3 oder Breitbild 16:9.
- **Größe.** Dies entspricht der Anzeigeauflösung und wird in Pixeln gemessen. Dies hängt mit dem Seitenverhältnis zusammen. Bei einem Verhältnis von 16:9 hat ein Video 432 x 240 Pixel, bei 4:3 sind es 320 x 240 Pixel.
- **FPS.** Die standardmäßigen Bildraten betragen 30, 25 oder 24 Frames pro Sekunde (fps), je nach Videostandard – NTSC, PAL oder Film. Diese Einstellung spielt jedoch keine Rolle, da Dynamic Media Classic immer dieselbe Frame-Rate wie das Quellvideo verwendet.
- **Format.** Dies ist MP4.
- **Bandbreite.** Dies ist die gewünschte Verbindungsgeschwindigkeit Ihrer Zielgruppe. Haben sie eine schnelle oder langsame Internetverbindung? Verwenden sie meist Desktop-Computer oder Mobilgeräte? Dies hängt auch mit der Auflösung (Größe) zusammen, da mehr Bandbreite benötigt wird, je größer das Video ist.

### Bestimmen der Datenrate oder „Bit-Rate“ für Ihr Video

Die Berechnung der Bit-Rate für Ihr Video ist einer der am wenigsten bekannten Faktoren bei der Bereitstellung von Videos im Internet, aber möglicherweise der wichtigste, da sie sich direkt auf das Erlebnis der Benutzenden auswirkt. Wenn Sie Ihre Bit-Rate zu hoch einstellen, erreichen Sie eine hohe Videoqualität, aber eine schlechte Leistung. Benutzende mit langsameren Internetverbindungen müssen warten, da das Video bei der Wiedergabe andauernd stoppt. Wenn Sie sie jedoch zu niedrig einstellen, leidet die Qualität. Bei der Videovorgabe schlägt Dynamic Media Classic je nach Zielbandbreite einen Datenbereich vor. Das ist ein guter Ausgangspunkt.

Wenn Sie es selbst herausfinden möchten, brauchen Sie einen Bit-Raten-Rechner. Dies ist ein Tool, das häufig von Videoprofis und -Fans verwendet wird, um zu schätzen, wie viele Daten in einen bestimmten Stream oder ein bestimmtes Medienelement passen (z. B. eine DVD).

## Erstellen einer benutzerdefinierten Videovorgabe

Manchmal benötigen Sie eine spezielle Videovorgabe, die nicht mit den Einstellungen der nativen Kodierungs-Videovorgaben übereinstimmt. Dies kann bei einem benutzerdefinierten Video einer bestimmten Größe vorkommen, z. B. eines, das mit 3D-Animationssoftware erstellt wurde, oder eines, das verkleinert wurde und nicht mehr die Originalgröße hat. Vielleicht möchten Sie mit verschiedenen Bandbreiteneinstellungen experimentieren, um Videos mit höherer oder geringerer Qualität bereitzustellen. Erstellen Sie in jedem Fall eine einzelne benutzerdefinierte Kodierungs-Videovorgabe.

### Videovorgaben-Workflow

1. Videovorgaben befinden sich unter **Einrichtung > Anwendungseinrichtung > Videovorgaben**. Hier finden Sie eine Liste aller für Ihr Unternehmen verfügbaren Kodierungsvorgaben.

   - Jedes Streaming-Video-Konto verfügt über Dutzende von Vorgaben; wenn Sie eigene benutzerdefinierte Vorgaben erstellen, sehen Sie sie auch hier.
   - Über das Dropdown-Menü können Sie nach Typ filtern. Die Vorgaben sind in Computer, Mobil und Tablet unterteilt.
     ![Bild](assets/video-overview/video-overview-4.jpg)

2. In der Spalte „Aktiv“ können Sie auswählen, ob alle Vorgaben beim Hochladen oder nur die von Ihnen ausgewählten angezeigt werden sollen. Wenn Sie sich in den USA befinden, möchten Sie möglicherweise die europäischen PAL-Vorgaben deaktivieren und in Großbritannien/EMEA die NTSC-Vorgaben deaktivieren.
3. Klicken Sie auf die Schaltfläche **Hinzufügen**, um eine benutzerdefinierte Vorgabe zu erstellen. Dadurch wird das Bedienfeld „Videovorgabe hinzufügen“ geöffnet. Der Prozess hier ähnelt dem Erstellen einer Bildvorgabe.
4. Geben Sie ihr zunächst einen **Vorgabennamen**, damit sie in der Liste der Voreinstellungen erscheint. Im obigen Beispiel ist diese Vorgabe für Video-Tutorials zur Bildschirmaufnahme vorgesehen.
5. Die **Beschreibung** ist optional, gibt Ihren Benutzenden jedoch eine QuickInfo, in der der Zweck dieser Vorgabe beschrieben wird.
6. **Encode File Suffix** wird an das Ende des Namens des Videos angehängt, das Sie hier erstellen. Denken Sie daran, dass Sie sowohl ein Master-Video als auch dieses codierte Video haben, das ein Derivat des Masters ist, und dass keine zwei Assets in Dynamic Media Classic die gleiche Asset-ID haben können.
7. **Wiedergabegerät** ist der Bereich, in dem Sie das gewünschte Videodateiformat auswählen (Computer, Mobil oder Tablet). Beachten Sie, dass Mobil und Tablet dasselbe MP4-Format erzeugen. Dynamic Media Classic muss nur wissen, in welcher Kategorie die Vorgabe platziert werden soll. Der theoretische Unterschied besteht darin, dass Tablet-Vorgaben normalerweise für eine schnellere Internetverbindung sorgen, da alle WLAN unterstützen.
8. **Ziel-Datenrate** ist etwas, das Sie selbst herausfinden müssen, aber Sie können einen vorgeschlagenen Bereich auf dem folgenden Bild sehen. Alternativ können Sie den Schieberegler auf die ungefähre Zielbandbreite ziehen. Verwenden Sie einen Bitratenrechner, um eine genauere Zahl zu erhalten. Man muss ein bisschen herumprobieren und Fehler machen.

   ![Bild](assets/video-overview/video-overview-5.jpg)

9. Stellen Sie das **Seitenverhältnis** Ihrer Quelldatei ein. Diese Einstellung ist direkt an die unten stehende Größe gebunden. Wenn Sie _Benutzerdefiniert_ wählen, müssen Sie sowohl Breite als auch Höhe manuell eingeben.
10. Wenn Sie ein Seitenverhältnis wählen, stellen Sie einen Wert für **Auflösung** ein, und Dynamic Media Classic wird den anderen Wert automatisch ausfüllen. Geben Sie jedoch für ein benutzerdefiniertes Seitenverhältnis beide Werte ein. Ihre Größe sollte Ihrer Datenrate entsprechen. Wenn Sie eine niedrige Datenrate und eine große Datenmenge festlegen, wird die Qualität voraussichtlich schlecht sein.
11. Klicken Sie auf **Speichern**, um Ihre Vorgabe zu speichern. Im Gegensatz zu anderen Vorgaben müssen Sie zu diesem Zeitpunkt keine Veröffentlichung vornehmen, da die Vorgaben nur zum Hochladen von Dateien verwendet werden. Später müssen Sie die codierten Videos veröffentlichen, aber die Vorgaben dienen nur der internen Dynamic Media Classic-Verwendung.
12. Um zu überprüfen, ob Ihre Videovorgabe in der Upload-Liste enthalten ist, klicken Sie auf **Hochladen**. Wählen Sie **Job-Optionen** und erweitern Sie **Video-Optionen**. Ihre Vorgabe wird in der Kategorie für das Wiedergabegerät aufgelistet, das Sie ausgewählt haben (Computer, Mobil oder Tablet).

Weitere Informationen zum [Hinzufügen oder Bearbeiten von Videovorgaben](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html?lang=de#adding-or-editing-a-video-encoding-preset).

## Hinzufügen von Untertiteln zu Ihrem Video

Manchmal kann es nützlich sein, Ihrem Video Untertitel hinzuzufügen, z. B. wenn Sie das Video für Betrachtende in mehreren Sprachen bereitstellen müssen, aber nicht das Audio in einer anderen Sprache abspielen oder das Video erneut in separaten Sprachen aufzeichnen möchten. Darüber hinaus bietet das Hinzufügen von Untertiteln für Personen mit eingeschränktem Hörvermögen und die Verwendung von Untertiteln eine größere Barrierefreiheit. Dynamic Media Classic erleichtert das Hinzufügen von Untertiteln zu Videos.

Erfahren Sie mehr über das [Hinzufügen von Untertiteln zu Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-captions-video.html?lang=de).

## Hinzufügen von Kapitelmarken zu Ihrem Video

Bei langen Videos schätzen Ihre Betrachtenden wahrscheinlich die Möglichkeiten und den Komfort, den die Navigation durch Kapitelmarken bietet. Dynamic Media Classic ermöglicht das einfache Hinzufügen von Kapitelmarken zu Ihrem Video.

Erfahren Sie mehr über das [Hinzufügen von Kapitelmarken zu Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-chapter-markers-video.html?lang=de).

## Themen zur Videoimplementierung

### Veröffentlichen und kopieren von URLs

Der letzte Schritt im Dynamic Media Classic-Workflow besteht darin, Ihre Videoinhalte zu veröffentlichen. Für Videos gibt es jedoch einen eigenen Veröffentlichungsauftrag mit der Bezeichnung „Video-Server-Veröffentlichung“, zu finden unter „Erweitert“.

![Bild](assets/video-overview/video-overview-6.jpg)

Erfahren Sie über das [Veröffentlichen Ihres Videos](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html?lang=de#publishing-video).

Nachdem Sie eine Videoveröffentlichung ausgeführt haben, können Sie eine URL abrufen, um in einem Webbrowser auf Ihre Videos und alle standardmäßigen Dynamic Media Classic-Viewer-Vorgaben zuzugreifen. Wenn Sie jedoch Ihre eigene Video-Viewer-Vorgabe anpassen oder erstellen, müssen Sie eine separate Veröffentlichung für den Bild-Server ausführen.

- Erfahren Sie mehr über das [Verknüpfen einer URL mit einer mobilen Site oder Website](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html?lang=de#linking-a-video-url-to-a-mobile-site-or-a-website).
- Erfahren Sie, wie Sie [den Video-Viewer auf einer Web-Seite einbetten](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html?lang=de#embedding-the-video-viewer-on-a-web-page).

Sie können Ihr Video auch mit einem Drittanbieter- oder benutzerdefinierten Video-Player bereitstellen.

Erfahren Sie, wie Sie [Videos mit dem Video-Player eines Drittanbieters bereitstellen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html?lang=de#deploying-video-using-a-third-party-video-player).

Wenn Sie auch die Videominiaturen (aus dem Video extrahierte Bilder) verwenden möchten, müssen Sie außerdem eine Veröffentlichung auf dem Bild-Server ausführen. Dies liegt daran, dass sich das Miniaturbild für das Video auf dem Bild-Server befindet, das Video selbst aber auf dem Video-Server. Videominiaturen können in Video-Suchergebnissen und Video-Wiedergabelisten und als erster „Poster-Frame“ verwendet werden, der im Video-Viewer angezeigt wird, bevor das Video wiedergegeben wird.

Erfahren Sie mehr darüber, wie Sie [mit Videominiaturen arbeiten](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html?lang=de#working-with-video-thumbnails).

### Auswählen und Anpassen einer Viewer-Vorgabe

Der Prozess zum Auswählen und Anpassen einer Viewer-Vorgabe entspricht dem für Bilder. Sie können entweder eine Vorgabe erstellen oder eine vorhandene Vorgabe ändern und unter einem neuen Namen speichern, Bearbeitungen vornehmen und eine Veröffentlichung zur Bildbereitstellung ausführen. Alle Viewer-Vorgaben werden auf dem Bild-Server veröffentlicht, nicht nur Vorgaben für Bilder. Daher müssen Sie eine Bildveröffentlichung ausführen, um Ihre neuen oder geänderten Vorgaben anzuzeigen.

>[!TIP]
>
>Führen Sie nach der Video-Server-Veröffentlichung eine Veröffentlichung zur Bildbereitstellung aus, um mit Ihren Videos verknüpfte Miniaturbilder zu veröffentlichen.

## Video-Suchmaschinen-Optimierung

Suchmaschinen-Optimierung (Search Engine Optimization, SEO) ist der Prozess zur Verbesserung der Sichtbarkeit einer Website oder Web-Seite in Suchmaschinen. Suchmaschinen eignen sich zwar hervorragend zum Sammeln von Informationen über textbasierte Inhalte, können jedoch nur dann Informationen über Videos adäquat abrufen, wenn ihnen diese Informationen zur Verfügung gestellt werden. Mit der Video-SEO-Funktion von Dynamic Media Classic können Sie Metadaten verwenden, um Suchmaschinen Beschreibungen Ihrer Videos bereitzustellen, sowie Video-Sitemaps und Media RSS(mRSS)-Feeds erstellen.

- **Video-Sitemap**. Informiert Google genau über die Position und Art der Videoinhalte auf einer Site. Daher können Videos in Google vollständig durchsucht werden. Beispielsweise kann eine Video-Sitemap die Laufzeit und Kategorien von Videos angeben.
- **mRSS-Feed**. Wird von Inhaltsherausgebern verwendet, um Mediendateien nach Yahoo! zu übertragen. Videosuche. Google unterstützt das Video-Sitemap- und Media RSS(mRSS)-Feed-Protokoll zum Übermitteln von Informationen an Suchmaschinen.

Wenn Sie Video-Sitemaps und mRSS-Feeds erstellen, entscheiden Sie, welche Metadatenfelder aus Videodateien eingeschlossen werden sollen. Auf diese Weise beschreiben Sie Ihre Videos für Suchmaschinen, damit diese den Traffic präziser zu Videos auf Ihrer Website leiten können.

Nach der Erstellung können Sie die Sitemap oder den Feed von Dynamic Media Classic automatisch veröffentlichen lassen, die Sitemap oder den Feed manuell veröffentlichen oder einfach eine Datei generieren, die Sie später bearbeiten können. Außerdem können Sie mithilfe von Dynamic Media Classic diese Datei automatisch täglich generieren und veröffentlichen.

Am Ende des Prozesses senden Sie die Datei oder URL an Ihre Suchmaschine. Diese Aufgabe wird außerhalb von Dynamic Media Classic durchgeführt, wir möchten im Folgenden aber trotzdem kurz darauf eingehen.

### Anforderungen für Sitemap-/mRSS-Dateien

Damit Google und andere Suchmaschinen Ihre Dateien nicht zurückweisen, müssen sie im richtigen Format vorliegen und bestimmte Informationen enthalten. Dynamic Media Classic generiert eine ordnungsgemäß formatierte Datei. Wenn die Informationen jedoch für einige Ihrer Videos nicht verfügbar sind, sind sie nicht in der Datei enthalten.

Die erforderlichen Felder sind „Landingpage“ (die URL für die Seite, die das Video bereitstellt, nicht die URL zum Video selbst), „Titel“ und „Beschreibung“. Jedes Video muss einen Eintrag für diese Elemente aufweisen oder es wird nicht in die generierte Datei eingeschlossen. Optionale Felder sind „Tags“ und „Kategorie“.

Es gibt zwei weitere erforderliche Felder: „Inhalts-URL“ (die URL zum Video-Asset selbst) und „Miniaturansicht“ (die URL zu einem Miniaturbild des Videos). Dynamic Media Classic füllt diese Werte jedoch automatisch für Sie aus.

Der empfohlene Workflow besteht darin, diese Daten vor dem Hochladen mithilfe von XMP-Metadaten in Ihre Videos einzubetten. Beim Hochladen werden sie dann von Dynamic Media Classic extrahiert. Zum Auffüllen der Daten in Standard-Metadatenfelder verwenden Sie schließlich eine Anwendung wie Adobe Bridge, die in allen Adobe Creative Cloud-Anwendungen enthalten ist.

Nach dieser Methode müssen Sie diese Daten nicht manuell mit Dynamic Media Classic eingeben. Sie können jedoch auch Metadatenvorgaben in Dynamic Media Classic verwenden, um dieselben Daten jedes Mal schnell und einfach eingeben zu können.

Weitere Informationen zu diesem Thema finden Sie unter [Anzeigen, Hinzufügen und Exportieren von Metadaten](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html?lang=de).

![Bild](assets/video-overview/video-overview-7.jpg)

Nachdem die Metadaten aufgefüllt wurden, können Sie sie in der Detailansicht für dieses Video-Asset sehen. Keywords sind möglicherweise ebenfalls vorhanden, befinden sich jedoch auf der Registerkarte „Keywords“.

- Weitere Informationen zum Hinzufügen von Keywords finden Sie [hier](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html?lang=de#add-or-edit-keywords).
- Weitere Informationen zur Suchmaschinen-Optimierung von Videos finden Sie [hier](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html?lang=de).
- Weitere Informationen zu Einstellungen für die Suchmaschinen-Optimierung von Videos finden Sie [hier](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html?lang=de#choosing-video-seo-settings).

#### Einrichten von Video SEO

Das Einrichten von Video-SEO beginnt mit der Auswahl des gewünschten Formats, der Generierungsmethode und der Metadatenfelder, die in die Datei aufgenommen werden sollen.

1. Navigieren Sie zu **Einrichtung > Anwendungseinstellungen > Video-SEO > Einstellungen**.
2. Wählen Sie im Menü **Generierungsmodus** ein Dateiformat aus. Die Standardeinstellung ist „Aus“. Wählen Sie daher zur Aktivierung entweder Video-Sitemap, mRSS oder beides.
3. Wählen Sie aus, ob automatisch oder manuell generiert werden soll. Der Einfachheit halber empfehlen wir Ihnen die Einstellung **Automatikmodus**. Wenn Sie die Option &quot;Automatisch&quot; wählen, müssen Sie auch die Option **Zur Veröffentlichung markieren** aktivieren, da die Datei(en) sonst nicht veröffentlicht werden können. Die Sitemap- und RSS-Dateien sind Typen eines XML-Dokuments und müssen wie jedes andere Asset veröffentlicht werden. Verwenden Sie einen der manuellen Modi, wenn Sie nicht alle Informationen zur Verfügung haben oder nur eine einmalige Generierung durchführen möchten.
4. Füllen Sie die Metadaten-Tags, die in den Dateien verwendet werden. Dieser Schritt ist nicht optional. Sie müssen mindestens die drei mit einem Sternchen (\*) gekennzeichneten Felder ausfüllen: **Landingpage**, **Titel**, und **Beschreibung**. Um Ihre Metadaten für diese Aufgaben zu verwenden, ziehen Sie die Felder aus dem Metadaten-Bedienfeld auf der rechten Seite per Drag &amp; Drop in ein entsprechendes Feld des Formulars. Dynamic Media Classic füllt das Platzhalterfeld automatisch mit den tatsächlichen Daten der einzelnen Videos. Sie müssen keine Metadatenfelder verwenden. Sie können stattdessen auch einen statischen Text eingeben, aber dieser Text wird für jedes Video angezeigt.
5. Sobald Sie die Informationen in die drei erforderlichen Felder eingegeben haben, aktiviert Dynamic Media Classic die Schaltflächen **Speichern** und **Speichern &amp; Generieren**. Klicken Sie auf eine, um Ihre Einstellungen zu speichern. Verwenden Sie **Speichern**, wenn Sie sich im automatischen Modus befinden und die Dateien später von Dynamic Media Classic generiert werden sollen. Verwenden Sie **Speichern &amp; Generieren**, um die Datei sofort zu erstellen.

### Testen und Veröffentlichen der Video-Sitemap, des mRSS-Feeds oder beider Dateien

Die generierten Dateien werden im Stammverzeichnis (Basisverzeichnis) Ihres Kontos angezeigt.

![Bild](assets/video-overview/video-overview-8.jpg)

Diese Dateien müssen veröffentlicht werden, da das Video-SEO-Tool eine Veröffentlichung nicht allein ausführen kann. Solange sie zur Veröffentlichung markiert sind, werden sie bei der nächsten Veröffentlichung an die Veröffentlichungs-Server gesendet.

Nach der Veröffentlichung sind Ihre Dateien mit diesem URL-Format verfügbar.

![Bild](assets/video-overview/video-url-format.png)

Beispiel:

![Bild](assets/video-overview/video-format-example.png)

### Übermitteln an Suchmaschinen

Der letzte Schritt des Prozesses besteht darin, Ihre Dateien und/oder URLs an Suchmaschinen zu senden. Dynamic Media Classic kann diesen Schritt nicht für Sie durchführen. Wenn Sie jedoch die URL und nicht die XML-Datei selbst senden, sollte der Feed aktualisiert werden, wenn die Datei das nächste Mal generiert und eine Veröffentlichung erfolgt.

Die Methode zum Senden an Ihre Suchmaschine variiert. Für Google verwenden Sie jedoch Google Webmaster Tools. Gehen Sie dort zu **Seitenkonfiguration > Sitemaps** und klicken Sie auf die Schaltfläche **Sitemap absenden**. Hier können Sie die Dynamic Media Classic-URL in Ihre SEO-Datei(en) einfügen.

### Video-SEO-Bericht

Dynamic Media Classic stellt einen Bericht zur Verfügung, aus dem hervorgeht, wie viele Videos erfolgreich in die Dateien aufgenommen wurden und, was noch wichtiger ist, welche Videos aufgrund von Fehlern nicht aufgenommen wurden. Um auf den Bericht zuzugreifen, gehen Sie zu **Einrichtung > Anwendungseinrichtung > Video-SEO > Bericht**.

![Bild](assets/video-overview/video-overview-9.jpg)

## Mobil-Implementierung für MP4-Video

Dynamic Media Classic enthält keine Viewer-Vorgaben für Mobilgeräte, da Viewer nicht erforderlich sind, um Videos auf unterstützten Mobilgeräten wiederzugeben. Solange Sie das H.264 MP4-Format codieren – entweder durch Konvertierung beim Hochladen oder durch Vorcodierung auf Ihrem Desktop – können unterstützte Tablets und Smartphones Ihre Videos wiedergeben, ohne einen Viewer zu benötigen. Dies wird auf Android- und iOS-Geräten (iPhone und iPad) unterstützt.

Der Grund, warum kein Viewer erforderlich ist, ist, dass beide Plattformen native H.264-Unterstützung haben. Sie können das Video entweder in eine HTML5-Web-Seite einbetten oder in die Applikation selbst einbetten. Die Betriebssysteme Android und iOS bieten einen Controller, mit dem das Video wiedergegeben werden kann.

Aus diesem Grund erhalten Sie mit Dynamic Media Classic keine URL zu einem Viewer für Mobilgeräte, sondern eine URL direkt zum Video. Im Vorschaufenster für ein MP4-Video finden Sie Links für Desktop und Mobil. Die Mobil-URL verweist auf das veröffentlichte Video.

Ein wichtiger Punkt bei veröffentlichten Videos ist, dass die URL den vollständigen Pfad zum Video enthält, nicht nur die Asset-ID. Beim Umgang mit Bildern rufen Sie das Bild unabhängig von der Ordnerstruktur anhand der Asset-ID auf. Für Videos müssen Sie jedoch auch die Ordnerstruktur angeben. In den oben genannten URLs wird das Video im Pfad gespeichert:

![Bild](assets/video-overview/mobile-implement-1.png)

Dies kann auch als Firmenname/Ordnerpfad/Videoname ausgedrückt werden.

### Methode 1: Browser-Wiedergabe – HTML5-Code

Verwenden Sie das HTML5-Video-Tag, um Ihr MP4-Video in eine Web-Seite einzubetten.

![Bild](assets/video-overview/browser-playback.png)

Diese Methode funktioniert auch für das Desktopweb, kann jedoch bei der Browser-Unterstützung zu Problemen führen. Nicht alle Desktop-Webbrowser unterstützen nativ H.264-Videos, darunter auch Firefox.

### Methode 2: App-Wiedergabe unter iOS – Media Player Framework

Alternativ können Sie das Dynamic Media Classic-MP4-Video in den Code Ihrer App einbetten. Im Folgenden finden Sie ein allgemeines, nur der Veranschaulichung dienendes iOS-Beispiel mit dem Media Player Framework:

![Bild](assets/video-overview/app-playback.png)

## Zusätzliche Ressourcen

Sehen Sie sich das On-Demand-Webinar [Dynamic Media Skill Builder: Video in Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) an, um zu erfahren, wie Videofunktionen in Dynamic Media Classic verwendet werden.
