---
title: Dynamic Media Classic - Hauptarbeitsablauf und Asset-Vorschau
description: 'Erfahren Sie mehr über den Hauptarbeitsablauf in Dynamic Media Classic, der die drei Schritte umfasst: Erstellen (und Hochladen), Verfassen (und Veröffentlichen) und Bereitstellen. Erfahren Sie dann, wie Sie eine Vorschau von Assets in Dynamic Media Classic anzeigen.'
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2734'
ht-degree: 5%

---


# Dynamic Media Classic Hauptarbeitsablauf und Vorschau von Assets {#main-workflow}

Dynamic Media unterstützt einen Workflow-Prozess zum Erstellen (und Hochladen), Erstellen (und Veröffentlichen) und Bereitstellen . Sie beginnen damit, Assets hochzuladen, dann etwas mit diesen Assets zu tun, z. B. ein Bildset zu erstellen und schließlich zu veröffentlichen, um sie live zu schalten. Der Schritt Erstellen ist für einige Workflows optional. Wenn Ihr Ziel beispielsweise darin besteht, Bilder nur dynamisch zu skalieren und zu zoomen oder Videos für Streaming zu konvertieren und zu veröffentlichen, sind keine erforderlichen Erstellungsschritte erforderlich.

![image](assets/main-workflow/create-author-deliver.jpg)

Der Workflow in Dynamic Media Classic-Lösungen besteht aus drei Hauptschritten:

1. Erstellen (und Hochladen) von SourceContent
2. Erstellen (und Veröffentlichen) von Assets
3. Bereitstellen von Assets

## Schritt 1: Erstellen (und Hochladen)

Dies ist der Anfang des Workflows. In diesem Schritt sammeln oder erstellen Sie den Quellinhalt, der in den verwendeten Workflow passt, und laden ihn in Dynamic Media Classic hoch. Das System unterstützt mehrere Dateitypen für Bilder, Videos und Schriftarten, aber auch für PDF, Adobe Illustrator und Adobe InDesign.

Siehe die vollständige Liste der [Unterstützten Dateitypen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

Sie können Quellinhalte auf verschiedene Arten hochladen:

- Direkt über Ihren Desktop oder Ihr lokales Netzwerk. [Erfahren Sie, wie](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- Von einem Dynamic Media Classic FTP-Server aus. [Erfahren Sie, wie](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

Der Standardmodus ist Von Desktop, wo Sie nach Dateien in Ihrem lokalen Netzwerk suchen und den Upload starten.

![image](assets/main-workflow/upload.jpg)

>[!TIP]
>
>Fügen Sie Ihre Ordner nicht manuell hinzu. Führen Sie stattdessen einen Upload von FTP aus und verwenden Sie die Option **Unterordner einschließen** , um Ihre Ordnerstruktur in Dynamic Media Classic neu zu erstellen.

Die beiden wichtigsten Upload-Optionen sind standardmäßig aktiviert - **Zur Veröffentlichung markieren**, was wir zuvor besprochen haben, und **Überschreiben**. &quot;Überschreiben&quot;bedeutet, dass die neue Datei die vorhandene Version ersetzt, wenn die hochgeladene Datei denselben Namen wie eine bereits im System enthaltene Datei hat. Wenn Sie diese Option deaktivieren, wird die Datei möglicherweise nicht hochgeladen.

### Optionen beim Hochladen von Bildern überschreiben

Es gibt vier Varianten der Option &quot;Bild überschreiben&quot;, die für Ihr gesamtes Unternehmen festgelegt werden können und häufig missverstanden werden. Kurz gesagt: Sie legen die Regeln so fest, dass Assets mit demselben Namen häufiger überschrieben werden sollen, oder Sie möchten, dass Überschreibungen seltener auftreten (in diesem Fall wird das neue Bild mit der Erweiterung &quot;-1&quot;oder &quot;-2&quot;umbenannt).

- **Im aktuellen Ordner Bilder mit demselben Namen und derselben Erweiterung überschreiben**. Diese Option ist die strengste Ersetzungsregel. Das Ersatzbild muss in den Ordner des Originalbilds hochgeladen werden und dieselbe Dateierweiterung haben wie das Originalbild. Wenn diese Voraussetzungen nicht erfüllt sind, wird ein Duplikat erstellt.

- **Im aktuellen Ordner Assets mit demselben Namen überschreiben, unabhängig von der Erweiterung**.
Erfordert das Hochladen des Ersatzbilds in denselben Ordner wie das Originalbild, die Dateinamenerweiterung kann jedoch vom Original abweichen. Beispielsweise ersetzt &quot;chair.tif&quot;die Datei &quot;chair.jpg&quot;.

- **In jedem Ordner Assets mit demselben Namen und derselben Erweiterung überschreiben**. Setzt voraus, dass das Ersatzbild dieselbe Dateierweiterung wie das Originalbild hat (beispielsweise muss chair.jpg die Datei &quot;chair.jpg&quot;ersetzen, nicht jedoch die Datei &quot;chair.tif&quot;). Sie können das Ersatzbild jedoch in einen anderen Ordner hochladen als den, in dem sich das Original befindet. Das hochgeladene Bild bleibt dann im neuen Ordner; die Datei befindet sich also nicht mehr am ursprünglichen Speicherort..

- **In jedem Ordner Assets mit ident. Namen unabhängig von der Erweiterung** überschreiben. Diese Option ist die am wenigsten einschränkende Ersetzungsregel. Sie können ein Ersatzbild in einen anderen Ordner hochladen als den, in dem sich das Originalbild befindet, und eine Datei mit einer anderen Dateierweiterung verwenden, um die Originaldatei zu ersetzen. Wenn sich die Originaldatei in einem anderen Ordner befindet, bleibt das Ersatzbild in dem neuen Ordner, in den es hochgeladen wurde.

Erfahren Sie mehr über die Option [Bilder überschreiben](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Obwohl dies nicht erforderlich ist, können Sie beim Hochladen mit einer der beiden oben genannten Methoden &quot;Auftragsoptionen&quot;für diesen bestimmten Upload angeben, z. B. um einen wiederkehrenden Upload zu planen, beim Hochladen Zuschneideoptionen festzulegen und viele andere. Diese können für einige Workflows nützlich sein. Daher sollten Sie überlegen, ob sie für Ihre Aufgaben geeignet sind.

Erfahren Sie mehr über [Auftragsoptionen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

Das Hochladen ist der erste erforderliche Schritt in einem Workflow, da Dynamic Media Classic nicht mit Inhalten arbeiten kann, die noch nicht in seinem System enthalten sind. Hinter den Kulissen beim Hochladen registriert das System jedes hochgeladene Asset mit der zentralisierten Dynamic Media Classic-Datenbank, weist eine ID zu und kopiert es in den Speicher. Darüber hinaus konvertiert das System Bilddateien in ein Format, das die dynamische Größenanpassung und den Zoom ermöglicht, und konvertiert Videodateien in das webfreundliche MP4-Format.

### Konzept: Was passiert mit Bildern, wenn Sie sie in Dynamic Media Classic hochladen?

Wenn Sie ein Bild eines beliebigen Typs in Dynamic Media Classic hochladen, wird es in ein Übergeordnetes Bildformat namens Pyramid TIFF oder P-TIFF konvertiert. Ein P-TIFF-Bild ähnelt dem Format eines mehrschichtigen TIFF-Bitmapbilds, allerdings enthält die Datei anstelle verschiedener Ebenen mehrere Größen (Auflösungen) desselben Bildes.

![image](assets/main-workflow/pyramid-p-tiff.png)

Beim Konvertieren des Bildes erstellt Dynamic Media Classic einen &quot;Schnappschuss&quot;der Bildgröße, skaliert ihn um die Hälfte und speichert ihn, skaliert ihn erneut um die Hälfte und speichert ihn usw., bis es mit mehreren Originalgrößen gefüllt ist. Eine 2000-Pixel-P-TIFF-Datei hat beispielsweise eine Größe von 1000, 500, 250 und 125 Pixel (und kleiner) in derselben Datei. Die P-TIFF-Datei ist das Format eines so genannten &quot;Übergeordneten Bildes&quot;in Dynamic Media Classic.

Wenn Sie ein Bild einer bestimmten Größe anfordern, ermöglicht es das Erstellen des P-TIFF-Programms dem Image-Server für Dynamic Media Classic, schnell die nächste größere Größe zu finden und sie zu verkleinern. Wenn Sie beispielsweise ein 2000-Pixel-Bild hochladen und ein 100-Pixel-Bild anfordern, findet Dynamic Media Classic die 125-Pixel-Version und skaliert sie auf 100 Pixel, anstatt von 2000 auf 100 Pixel zu skalieren. Dadurch wird der Betrieb sehr schnell. Darüber hinaus ermöglicht es der Zoom-Viewer beim Zoomen eines Bildes, anstelle des gesamten Bild mit voller Auflösung nur eine Kachel des Bildes anzufordern, das gezoomt wird. So unterstützt das Übergeordnete Bildformat, die P-TIFF-Datei, sowohl dynamische Größenanpassung als auch Zoom.

Auf ähnliche Weise können Sie Ihr Übergeordnetes Quellvideo in Dynamic Media Classic hochladen und Dynamic Media Classic kann es beim Hochladen automatisch in der Größe ändern und in das Web-freundliche MP4-Format konvertieren.

### Regeln zur Bestimmung der optimalen Größe für die hochgeladenen Bilder

**Laden Sie Bilder in der benötigten Größe hoch.**

- Wenn Sie zoomen müssen, laden Sie ein Bild mit hoher Auflösung mit einem Bereich von 1500 bis 2500 Pixel in der längsten Dimension hoch. Überlegen Sie, wie viele Details Sie geben möchten, die Qualität Ihrer Quellbilder und die Größe des angezeigten Produkts. Laden Sie beispielsweise ein 1000-Pixel-Bild für einen winzigen Ring hoch, aber ein 3000-Pixel-Bild für einen ganzen Raum-Zen.
- Wenn Sie nicht zoomen müssen, laden Sie es in der genauen Größe hoch, die es angezeigt wird. Wenn Sie beispielsweise Logos oder Splash-/Banner-Bilder auf Ihren Seiten platzieren möchten, laden Sie sie genau in der Größe von 1:1 hoch und rufen Sie sie genau in dieser Größe auf.

**Laden Sie Ihre Bilder nie hoch oder blasen Sie sie auf, bevor Sie sie in Dynamic Media Classic hochladen.** Beispiel: Sie sollten kein kleineres Bild hochladen, um es zu einem 2000-Pixel-Bild zu machen. Es wird nicht gut aussehen. Machen Sie Ihre Bilder so perfekt wie möglich, bevor Sie sie hochladen.

**Für den Zoom gibt es keine Mindestgröße, aber die Viewer zoomen standardmäßig nicht über 100 % hinaus.** Wenn Ihr Bild zu klein ist, zoomt es überhaupt nicht oder zoomt nur einen winzigen Wert, um zu verhindern, dass es schlecht aussieht.

**Obwohl es kein Minimum für die Bildgröße gibt, empfehlen wir nicht, riesige Bilder hochzuladen.** Ein riesiges Bild kann als 4000+ Pixel betrachtet werden. Das Hochladen von Bildern dieser Größe kann mögliche Fehler wie Staub- oder Haarkörner im Bild zeigen. Solche Bilder benötigen auch mehr Speicherplatz auf dem Dynamic Media Classic-Server, was dazu führen kann, dass Sie die vertraglich festgelegten Speichergrenzen überschreiten.

Erfahren Sie mehr über [Hochladen von Dateien](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Schritt 2: Autor (und Veröffentlichen)

Nach dem Erstellen und Hochladen Ihres Inhalts erstellen Sie neue Rich-Media-Assets aus Ihren hochgeladenen Assets, indem Sie einen oder mehrere Unter-Workflows ausführen. Dazu gehören alle Arten von Set-Sammlungen - Bild-, Farb-, Rotations- und gemischten Mediensets sowie Vorlagen. Es enthält auch Video. Später werden wir noch viel detaillierter über die einzelnen Arten von Bild-Sammlungsset und Video-Rich-Media-Daten sprechen. In fast allen Fällen wählen Sie jedoch zunächst ein oder mehrere Assets aus (oder haben Sie keine Assets ausgewählt) und wählen dann den Typ des Assets aus, das Sie erstellen möchten. Sie können beispielsweise ein Hauptbild und einige Ansichten dieses Bildes auswählen und ein Bildset erstellen, eine Sammlung von alternativen Ansichten desselben Produkts.

>[!IMPORTANT]
>
>Stellen Sie sicher, dass alle Assets zur Veröffentlichung markiert sind. Während standardmäßig alle Assets beim Hochladen automatisch zur Veröffentlichung markiert werden, müssen neu erstellte Assets aus Ihrem hochgeladenen Inhalt ebenfalls zur Veröffentlichung markiert werden.

Nachdem Sie Ihr neues Asset erstellt haben, führen Sie einen Veröffentlichungsauftrag aus. Sie können dies manuell tun oder einen Veröffentlichungsauftrag planen, der automatisch ausgeführt wird. Durch die Veröffentlichung werden alle Inhalte aus dem privaten Dynamic Media Classic-Bereich in den öffentlichen Veröffentlichungsserver-Bereich der Gleichung kopiert. Das Produkt eines Dynamic Media-Veröffentlichungsauftrags ist eine eindeutige URL für jedes veröffentlichte Asset.

Der Server, auf dem Sie veröffentlichen, hängt vom Inhaltstyp und Workflow ab. Beispielsweise werden alle Bilder an den Image-Server gesendet und das Video an den FMS-Server gestreamt. Aus praktischen Gründen sprechen wir von einer &quot;Veröffentlichung&quot; als ein einziges Ereignis auf einem einzelnen Server.

Durch die Veröffentlichung werden alle Inhalte veröffentlicht, die zur Veröffentlichung markiert sind - nicht nur Ihre Inhalte. Ein einzelner Administrator veröffentlicht in der Regel für alle Benutzer und nicht für einzelne Benutzer, die eine Veröffentlichung ausführen. Der Administrator kann nach Bedarf veröffentlichen oder einen wiederkehrenden täglichen, wöchentlichen oder sogar alle 10 Minuten stattfindenden Auftrag einrichten, der automatisch veröffentlicht wird. Veröffentlichen Sie die Inhalte nach einem für Ihr Unternehmen sinnvollen Zeitplan.

>[!TIP]
>
>Automatisieren Sie Ihre Veröffentlichungsaufträge und planen Sie die Ausführung einer vollständigen Veröffentlichung täglich um 12:00 Uhr oder zu einem beliebigen späteren Zeitpunkt am Abend.

### Konzept: Grundlegendes zur Dynamic Media Classic-URL

Das Endprodukt eines Dynamic Media Classic-Workflows ist eine URL, die auf das Asset verweist (unabhängig davon, ob es sich um ein Bildset oder ein adaptives Videoset handelt). Diese URLs sind sehr vorhersagbar und folgen demselben Muster. Bei Bildern wird jedes Bild aus dem Übergeordneten P-TIFF-Bild generiert.

Hier finden Sie die Syntax für die URL eines Bildes mit einigen Beispielen:

![image](assets/main-workflow/dmc-url.jpg)

In der URL ist alles, was links neben dem Fragezeichen steht, der virtuelle Pfad zu einem bestimmten Bild. Alles rechts neben dem Fragezeichen befindet sich ein Image Server-Modifikator, eine Anleitung zur Verarbeitung des Bildes. Wenn Sie mehrere Modifikatoren haben, werden diese durch kaufmännische Und-Zeichen getrennt.

Im ersten Beispiel ist der virtuelle Pfad zum Bild &quot;Backpack_A&quot;`http://sample.scene7.com/is/image/s7train/BackpackA`. Die Bildserver-Modifikatoren ändern die Größe des Bildes auf eine Breite von 250 Pixel (von wid=250) und führen eine Neuberechnung des Bildes mithilfe des Lanczos-Interpolationsalgorithmus durch, der beim Vergrößern der Größe scharfgezeichnet wird (von resMode=sharp2).

Das zweite Beispiel wendet das so genannte &quot;Bildvorgabe&quot;auf dasselbe Backpack_A-Bild an, wie durch $! angegeben!_template300$. Die $ -Symbole auf beiden Seiten des Ausdrucks zeigen an, dass eine Bildvorgabe, ein zusammengesetzter Satz von Bild-Modifikatoren, auf das Bild angewendet wird.

Sobald Sie wissen, wie Dynamic Media Classic-URLs zusammengestellt werden, verstehen Sie, wie Sie sie programmgesteuert ändern und wie Sie sie tiefer in Ihre Site- und Backend-Systeme integrieren können.

### Konzept: Grundlagen zur Caching-Verzögerung

Neu hochgeladene und veröffentlichte Assets werden sofort angezeigt, aktualisierte Assets hingegen können mit einer 10-Stunden-Cache-Verzögerung versehen werden. Standardmäßig haben alle veröffentlichten Assets mindestens 10 Stunden vor ihrem Ablauf. Wir sagen &quot;Minimum&quot;, da jedes Mal, wenn das Bild angezeigt wird, es eine Uhr startet, die nicht abläuft, bis 10 Stunden vergangen sind, in der niemand dieses Bild gesehen hat. Dieser 10-Stunden-Zeitraum ist die &quot;Time to Live&quot;für ein Asset. Sobald der Cache für dieses Asset abläuft, kann die aktualisierte Version bereitgestellt werden.

Dies ist in der Regel kein Problem, es sei denn, es ist ein Fehler aufgetreten und das Bild/Asset hat denselben Namen wie die zuvor veröffentlichte Version. Es gibt jedoch ein Problem mit dem Bild. Beispielsweise haben Sie versehentlich eine Version mit niedriger Auflösung hochgeladen oder Ihr Kunstdirektor hat das Bild nicht genehmigt. In diesem Fall möchten Sie das Originalbild abrufen und es mit derselben Asset-ID durch eine neue Version ersetzen.

Erfahren Sie, wie Sie [den Cache für die zu aktualisierenden URLs manuell löschen](https://docs.adobe.com/content/help/en/experience-manager-65/assets/dynamic/invalidate-cdn-cached-content.html).

>[!TIP]
>
>Um Probleme mit der Caching-Verzögerung zu vermeiden, arbeiten Sie immer voraus - einen Abend, einen Tag, zwei Wochen usw. Führen Sie rechtzeitig eine Qualitätssicherung/Akzeptanz durch, damit interne Parteien Ihre Arbeit vor der Veröffentlichung nachweisen können. Auch wenn Sie einen Abend vorher arbeiten, können Sie an diesem Abend Änderungen vornehmen und erneut veröffentlichen. Am Morgen sind die zehn Stunden vergangen und der Cache wird mit dem richtigen Bild aktualisiert.

- Erfahren Sie mehr über [Erstellen eines Veröffentlichungsauftrags](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- Erfahren Sie mehr über [Publishing](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Schritt 3: Versand

Beachten Sie, dass das Endprodukt eines Dynamic Media Classic-Workflows eine URL ist, die auf das Asset verweist. Die URL kann auf ein einzelnes Bild, ein Bildset, ein Rotationsset oder eine andere Bildset-Sammlung oder ein Video verweisen. Sie müssen diese URL verwenden und etwas daran ändern, z. B. Ihren HTML-Code so bearbeiten, dass die `<IMG>` -Tags auf das Dynamic Media Classic-Bild verweisen, anstatt auf ein Bild zu verweisen, das von Ihrer aktuellen Site stammt.

Im Schritt Versand müssen Sie diese URLs in Ihre Website, App, E-Mail-Kampagne oder einen anderen digitalen Touchpoint integrieren, auf dem das Asset angezeigt werden soll.

Beispiel für die Integration der Dynamic Media Classic-URL für ein Bild in eine Website:

![image](assets/main-workflow/example-url-1.png)

Die URL in Rot ist das einzige für Dynamic Media Classic spezifische Element.

Ihr IT-Team oder Integrationspartner kann die Führung beim Schreiben und Ändern von Code übernehmen, um Dynamic Media Classic-URLs in Ihre Site zu integrieren. Adobe verfügt über ein Beratungsteam, das entweder durch technische, kreative oder allgemeine Beratung bei diesen Bemühungen helfen kann.

Bei komplexeren Lösungen wie Zoom-Viewern oder Viewern, die Zoom und alternative Ansichten kombinieren, verweist die URL normalerweise auf einen Viewer, der von Dynamic Media Classic gehostet wird, und auch innerhalb dieser URL ist ein Verweis auf eine Asset-ID.

Beispiel eines Links (in rot), der ein Bildset in einem Viewer in einem neuen Popup-Fenster öffnet:

![image](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Sie müssen die Dynamic Media Classic-URLs in Ihre Website, App, E-Mail und andere digitale Touchpoints integrieren - Dynamic Media Classic kann das nicht für Sie tun!

## Anzeigen von Assets in einer Vorschau

Wahrscheinlich möchten Sie eine Vorschau der hochgeladenen Assets anzeigen oder diese erstellen oder bearbeiten, um sicherzustellen, dass sie bei der Ansicht Ihrer Kunden wie gewünscht angezeigt werden. Sie können auf das Vorschaufenster zugreifen, indem Sie auf eine beliebige **Vorschau**-Schaltfläche klicken, entweder auf der Miniaturansicht des Assets, oben im **Durchsuchen/Erstellen-Bedienfeld** oder indem Sie zu **Datei > Vorschau** navigieren. In einem Browserfenster wird die Vorschau des Assets angezeigt, das sich derzeit im Bedienfeld befindet, unabhängig davon, ob es sich um ein Bild, ein Video oder ein erstelltes Asset wie ein Bildset handelt.

### Dynamic Size Preview (Bildvorgaben)

Mit der Vorschau **Größen** können Sie eine Vorschau Ihrer Bilder in mehreren Größen anzeigen. Dadurch wird eine Liste der verfügbaren Bildvorgaben geladen. Wir werden später über Bildvorgaben diskutieren, aber wir stellen uns diese als &quot;Rezepte&quot;für das Laden Ihres Bildes in einer benannten Größe mit bestimmten Mengen an Scharfzeichnung und Bildqualität vor.

### Zoom-Vorschau

Sie können auch die Option **Zoom** verwenden, um Ihre Bildvorschau in einer von vielen vordefinierten Zoom-Vorgaben anzuzeigen, die auf verschiedenen eingeschlossenen Zoom-Viewern basieren.

Erfahren Sie mehr über [Vorschau von Assets](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/previewing-asset.html).
