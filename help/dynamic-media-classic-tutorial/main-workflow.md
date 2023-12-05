---
title: Hauptarbeitsablauf und Asset-Vorschau bei Dynamic Media Classic
description: 'Erfahren Sie mehr über den Hauptarbeitsablauf in Dynamic Media Classic, der die drei Schritte umfasst: Erstellen (und Hochladen), Verfassen (und Veröffentlichen) und Bereitstellen. Erfahren Sie, wie Sie Assets in Dynamic Media Classic in einer Vorschau anzeigen. '
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
duration: 700
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2658'
ht-degree: 98%

---

# Hauptarbeitsablauf und Asset-Vorschau bei Dynamic Media Classic {#main-workflow}

Dynamic Media unterstützt Workflow-Prozesse zum Erstellen (und Hochladen), Verfassen (und Veröffentlichen) und Bereitstellen. Sie beginnen damit, Assets hochzuladen, dann etwas mit diesen Assets zu tun, z. B. ein Bildset zu erstellen, und schließlich zu veröffentlichen, um sie live zu schalten. Der Schritt „Erstellen“ ist in einigen Workflows optional. Wenn Ihr Ziel beispielsweise darin besteht, Bilder nur dynamisch zu skalieren und zu zoomen oder Videos zum Streamen zu konvertieren und zu veröffentlichen, sind keine Erstellungsschritte erforderlich.

![Bild](assets/main-workflow/create-author-deliver.jpg)

Der Workflow bei Dynamic Media Classic-Lösungen besteht aus drei Hauptschritten:

1. Erstellen (und Hochladen) von Quellinhalten
2. Verfassen (und Veröffentlichen) von Assets
3. Bereitstellen von Assets

## Schritt 1: Erstellen (und Hochladen)

Dies ist der Anfang des Workflows. In diesem Schritt sammeln oder erstellen Sie den Quellinhalt, der in den verwendeten Workflow passt, und laden ihn in Dynamic Media Classic hoch. Das System unterstützt mehrere Dateitypen für Bilder, Videos und Schriftarten, aber auch für PDF, Adobe Illustrator und Adobe InDesign.

Siehe die vollständige Liste der [unterstützten Dateitypen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=de#supported-asset-file-formats).

Sie können Quellinhalte auf verschiedene Arten hochladen:

- Direkt über Ihren Desktop oder Ihr lokales Netzwerk. [Erfahren Sie, wie](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=de#upload-files-using-sps-desktop-application).
- Von Dynamic Media Classic über einen FTP-Server. [Erfahren Sie, wie](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=de#upload-files-using-via-ftp).

Der Standardmodus ist „Vom Desktop“, bei dem Sie nach Dateien in Ihrem lokalen Netzwerk suchen und den Upload starten.

![Bild](assets/main-workflow/upload.jpg)

>[!TIP]
>
>Fügen Sie Ihre Ordner nicht manuell hinzu. Führen Sie stattdessen einen Upload von FTP aus und verwenden Sie die Option **Unterordner einschließen**, um Ihre Ordnerstruktur in Dynamic Media Classic neu zu erstellen.

Die beiden wichtigsten Upload-Optionen sind standardmäßig aktiviert – **Zur Veröffentlichung markieren**, worüber wir zuvor gesprochen haben, und **Überschreiben**. „Überschreiben“ bedeutet, dass die neue Datei die vorhandene Version ersetzt, wenn die hochgeladene Datei denselben Namen wie eine bereits im System enthaltene Datei hat. Wenn Sie diese Option deaktivieren, wird die Datei möglicherweise nicht hochgeladen.

### Überschreibungsoptionen beim Hochladen von Bildern

Es gibt vier Varianten der Option „Bild überschreiben“, die für Ihr gesamtes Unternehmen festgelegt werden können und häufig missverstanden werden. Kurz gesagt: Sie legen die Regeln so fest, dass Assets mit demselben Namen häufiger überschrieben werden sollen, oder Sie möchten, dass Überschreibungen seltener auftreten (in diesem Fall wird das neue Bild mit der Erweiterung „-1“ oder „-2“ umbenannt).

- **Im aktuellen Ordner Bilder mit demselben Namen und derselben Erweiterung überschreiben**.
Diese Option stellt die strengste Ersetzungsregel dar. Das Ersatzbild muss in den Ordner des Originalbilds hochgeladen werden und dieselbe Dateierweiterung haben wie das Originalbild. Wenn diese Voraussetzungen nicht erfüllt sind, wird ein Duplikat erstellt.

- **Im aktuellen Ordner Assets mit identischem Basisnamen unabhängig von Erweiterungen überschreiben**.
Erfordert das Hochladen des Ersatzbilds in denselben Ordner wie das Originalbild, die Dateinamenerweiterung kann jedoch vom Original abweichen. Beispielsweise ersetzt „chair.tif“ die Datei „chair.jpg“.

- **In belieb. Ordner Assets mit ident. Namen/Erweiterung überschreiben**.
Setzt voraus, dass das Ersatzbild dieselbe Dateierweiterung wie das Originalbild hat (beispielsweise muss chair.jpg die Datei „chair.jpg“ ersetzen, nicht jedoch die Datei „chair.tif“). Sie können das Ersatzbild jedoch in einen anderen Ordner hochladen als den, in dem sich das Original befindet. Das aktualisierte Bild befindet sich im neuen Ordner. Die Datei befindet sich nicht mehr am ursprünglichen Speicherort.

- **In einem beliebigen Ordner Assets mit ident. Namen unabhängig von Erweiterung überschreiben**.
Diese Option ist die am wenigsten einschränkende Ersetzungsregel. Sie können ein Ersatzbild in einen anderen Ordner hochladen als den, in dem sich das Originalbild befindet, und eine Datei mit einer anderen Dateierweiterung verwenden, um die Originaldatei zu ersetzen. Wenn sich die Originaldatei in einem anderen Ordner befindet, bleibt das Ersatzbild in dem neuen Ordner, in den es hochgeladen wurde.

Hier finden Sie weitere Informationen zur [Option „Bilder überschreiben“](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=de#using-the-overwrite-images-option).

Obwohl dies nicht erforderlich ist, können Sie beim Hochladen mit einer der beiden oben genannten Methoden Auftragsoptionen für diesen bestimmten Upload angeben, z. B. um einen wiederkehrenden Upload zu planen, beim Hochladen Zuschneideoptionen festzulegen und viele andere. Diese können für einige Workflows nützlich sein. Daher sollten Sie überlegen, ob sie für Ihre Aufgaben geeignet sind.

Hier finden Sie weitere Informationen zu [Auftragsoptionen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=de#upload-options).

Das Hochladen ist der erste erforderliche Schritt in einem Workflow, da Dynamic Media Classic nicht mit Inhalten arbeiten kann, die noch nicht im System enthalten sind. Hinter den Kulissen registriert das System beim Hochladen jedes hochgeladene Asset mit der zentralisierten Dynamic Media Classic-Datenbank, weist ihm eine ID zu und kopiert es in den Speicher. Darüber hinaus konvertiert das System Grafikdateien in ein Format, das eine dynamische Größenanpassung und Zoomen ermöglicht, und konvertiert Videodateien in das Web-freundliche MP4-Format.

### Konzept: Folgendes passiert mit Bildern, wenn Sie sie in Dynamic Media Classic hochladen

Wenn Sie ein Bild eines beliebigen Typs in Dynamic Media Classic hochladen, wird es in ein übergeordnetes Bildformat namens Pyramid-TIFF oder P-TIFF konvertiert. Ein P-TIFF ähnelt dem Format eines TIFF-Bitmap-Bilds mit mehreren Ebenen, allerdings enthält die Datei nicht mehrere Ebenen, sondern mehrere Größen (Auflösungen) eines Bildes.

![Bild](assets/main-workflow/pyramid-p-tiff.png)

Während der Bildkonvertierung erstellt Dynamic Media Classic einen „Schnappschuss“ der Bildgröße, skaliert ihn um die Hälfte und speichert ihn, skaliert ihn erneut um die Hälfte und speichert ihn usw., bis er mit geradzahligen Vielfachen der Originalgröße gefüllt ist. Beispielsweise hat ein 2000-Pixel-P-TIFF eine Größe von 1000, 500, 250 und 125 Pixel (und kleiner) in derselben Datei. Die P-TIFF-Datei ist das Format eines sogenannten „übergeordneten Bildes“ in Dynamic Media Classic.

Wenn Sie ein Bild einer bestimmten Größe anfordern, ermöglicht es das Erstellen des P-TIFF dem Image-Server von Dynamic Media Classic, die nächstgrößere Größe schnell zu finden und sie zu verkleinern. Wenn Sie beispielsweise ein 2000-Pixel-Bild hochladen und ein 100-Pixel-Bild anfordern, findet Dynamic Media Classic die 125-Pixel-Version und skaliert sie auf 100 Pixel, anstatt von 2000 auf 100 Pixel zu skalieren. Dadurch ist der Betrieb sehr schnell. Darüber hinaus ermöglicht dies dem Zoom-Viewer beim Zoomen eines Bildes, anstelle des gesamten Bildes mit voller Auflösung nur eine Kachel des Bildes anzufordern, das gezoomt wird. So unterstützt das übergeordnete Bildformat, die P-TIFF-Datei, sowohl das dynamische Dimensionieren als auch das Zoomen.

Auf ähnliche Weise können Sie Ihr übergeordnetes Quellvideo in Dynamic Media Classic hochladen und Dynamic Media Classic kann es beim Hochladen automatisch vergrößern und in das Web-freundliche MP4-Format konvertieren.

### Faustregeln zur Bestimmung der optimalen Größe für die hochgeladenen Bilder

**Laden Sie Bilder in der benötigten Größe hoch.**

- Wenn Sie zoomen müssen, laden Sie ein Bild mit hoher Auflösung mit einem Bereich von 1500 bis 2500 Pixel in der längsten Dimension hoch. Überlegen Sie, wie viele Details Sie zeigen möchten, welche Qualität Ihre Quellbilder und welche Größe das angezeigte Produkt haben soll. Laden Sie beispielsweise ein 1000-Pixel-Bild für einen kleinen Ring hoch, aber ein 3000-Pixel-Bild für eine ganze Raumszene.
- Wenn Sie nicht zoomen müssen, laden Sie es in der genauen Größe hoch, in der es angezeigt wird. Wenn Sie beispielsweise Logos oder Splash-/Banner-Bilder auf Ihren Seiten platzieren möchten, laden Sie sie genau in der 1:1-Größe hoch und rufen Sie sie genau in dieser Größe auf.

**Führen Sie nie ein Upsampling Ihrer Bilder durch oder blähen Sie sie auf, bevor Sie sie in Dynamic Media Classic hochladen.** Beispiel: Sie sollten kein kleineres Bild zu einem 2000-Pixel-Bild machen. Es wird einfach nicht gut aussehen. Machen Sie Ihre Bilder so perfekt wie möglich, bevor Sie sie hochladen.

**Für den Zoom gibt es keine minimale Größe, aber die Viewer zoomen standardmäßig nicht über 100 % hinaus.** Wenn Ihr Bild zu klein ist, wird es überhaupt nicht gezoomt oder nur sehr geringfügig, um zu verhindern, dass es schlecht aussieht.

**Obwohl es kein Minimum für die Bildgröße gibt, empfehlen wir nicht, riesige Bilder hochzuladen.** Als riesiges Bild kann eines mit mehr als 4000 Pixeln gelten. Das Hochladen von Bildern dieser Größe kann mögliche Schwachstellen wie Staubkörner oder Haare im Bild zeigen. Solche Bilder nehmen mehr Speicherplatz auf dem Dynamic Media Classic-Server ein, was dazu führen kann, dass Sie die vertraglich festgelegten Speichergrenzen überschreiten.

Hier finden Sie weitere Informationen zum [Hochladen von Dateien](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=de#uploading-your-files).

## Schritt 2: Erstellen (und veröffentlichen)

Nach dem Erstellen und Hochladen Ihres Inhalts erstellen Sie neue Rich-Media-Assets aus Ihren hochgeladenen Assets, indem Sie einen oder mehrere Unter-Workflows ausführen. Dazu gehören alle Arten von Set-Sammlungen: Bild-, Farbfeld-, Rotations- und gemischte Medien-Sets sowie Vorlagen. Es umfasst auch Video. Später werden wir noch viel detaillierter über die einzelnen Arten von Bild-Sammlungs-Sets und Video-Rich-Media-Daten sprechen. In fast allen Fällen wählen Sie jedoch zunächst ein oder mehrere Assets aus (oder Sie haben keine Assets ausgewählt) und dann den Typ des Assets, das Sie erstellen möchten. Sie können beispielsweise ein Hauptbild und einige Ansichten dieses Bildes auswählen und ein Bild-Set erstellen, d. h. eine Sammlung von verschiedenen Ansichten desselben Produkts.

>[!IMPORTANT]
>
>Stellen Sie sicher, dass all Ihre Assets zur Veröffentlichung markiert sind. Während standardmäßig alle Assets beim Hochladen automatisch zur Veröffentlichung markiert werden, müssen alle Assets, die Sie aus Ihren hochgeladenen Inhalten neu erstellen, ebenfalls zur Veröffentlichung markiert werden.

Führen Sie dazu einen Veröffentlichungsvorgang aus, nachdem Sie ein neues Asset erstellt haben. Sie können dies manuell tun oder einen Veröffentlichungsvorgang planen, der automatisch ausgeführt wird. Beim Veröffentlichen werden alle Inhalte aus dem privaten Dynamic Media Classic-Bereich in den öffentlichen Veröffentlichungs-Server-Bereich der Gleichung kopiert. Das Ergebnis eines Dynamic Media-Veröffentlichungsvorgangs ist eine eindeutige URL für jedes veröffentlichte Asset.

Der Server, auf dem Sie veröffentlichen, hängt vom Inhaltstyp und Workflow ab. Beispielsweise werden alle Bilder an den Image-Server gesendet und Streaming-Video an den FMS-Server. Aus praktischen Gründen sprechen wir von einer „Veröffentlichung“ als einem einzelnen Ereignis auf einem einzelnen Server.

Durch die Veröffentlichung werden alle Inhalte veröffentlicht, die zur Veröffentlichung markiert sind – nicht nur Ihre Inhalte. Einzelne Admins veröffentlichen in der Regel für alle Benutzenden und nicht für einzelne Benutzende, die eine Veröffentlichung ausführen. Admins können nach Bedarf veröffentlichen oder einen wiederkehrenden täglichen, wöchentlichen oder sogar alle 10 Minuten ausgeführten Vorgang einrichten, der automatisch veröffentlicht wird. Veröffentlichen Sie die Inhalte nach einem für Ihr Unternehmen passenden Zeitplan.

>[!TIP]
>
>Automatisieren Sie Ihre Veröffentlichungsvorgänge und planen Sie die Ausführung einer vollständigen Veröffentlichung etwa täglich um Mitternacht oder zu einem beliebigen Zeitpunkt am späten Abend.

### Konzept: Grundlagen zur Dynamic Media Classic-URL

Das Endprodukt eines Dynamic Media Classic-Workflows ist eine URL, die auf das Asset verweist (unabhängig davon, ob es sich um ein Bild-Set oder ein adaptives Video-Set handelt). Diese URLs sind stark vorhersagbar und folgen immer demselben Muster. Bei Bildern wird jedes Bild aus dem übergeordneten P-TIFF-Bild generiert.

Hier sehen Sie die Syntax für die URL eines Bildes mit einigen Beispielen:

![Bild](assets/main-workflow/dmc-url.jpg)

In der URL ist alles, was links neben dem Fragezeichen steht, der virtuelle Pfad zu einem bestimmten Bild. Alles rechts neben dem Fragezeichen ist ein Modifikator für den Bild-Server, eine Anweisung, wie das Bild zu verarbeiten ist. Wenn Sie mehrere Modifikatoren haben, werden diese durch kaufmännische Und-Zeichen getrennt.

Im ersten Beispiel hat das Bild „Backpack_A“ den virtuellen Pfad `http://sample.scene7.com/is/image/s7train/BackpackA`. Die Bild-Server-Modifikatoren ändern die Größe des Bildes auf eine Breite von 250 Pixel (durch wid=250) und führen eine Neuberechnung des Bildes mithilfe des Lanczos-Interpolationsalgorithmus durch, der beim Vergrößern scharfgezeichnet wird (durch resMode=sharp2).

Im zweiten Beispiel wird auf dasselbe Bild „Backpack_A“ die sogenannte „Bildvorgabe“ angewendet, wie angegeben durch $!_template300$. Die $-Symbole auf beiden Seiten des Ausdrucks zeigen an, dass eine Bildvorgabe, ein zusammengesetzter Satz von Bild-Modifikatoren, auf das Bild angewendet wird.

Sobald Sie überblicken, wie Dynamic Media Classic-URLs zusammengestellt werden, wissen Sie, wie Sie sie programmgesteuert ändern und tiefer in Ihre Site- und Backend-Systeme integrieren können.

### Konzept: Erläuterungen zur Caching-Verzögerung

Neu hochgeladene und veröffentlichte Assets werden sofort angezeigt, aktualisierte Assets können hingegen einer 10-Stunden-Cache-Verzögerung unterliegen. Standardmäßig liegen mindestens 10 Stunden vor dem Ablauf aller veröffentlichten Assets. Wir sagen „mindestens“, da jedes Mal, wenn das Bild angezeigt wird, eine Uhr gestartet wird, die erst abläuft, wenn 10 Stunden verstrichen sind, in denen niemand das Bild angesehen hat. Dieser 10-Stunden-Zeitraum ist die „Time to Live“ für ein Asset. Sobald der Cache für dieses Asset abläuft, kann die aktualisierte Version bereitgestellt werden.

Dies ist in der Regel kein Problem, es sei denn, es ist ein Fehler aufgetreten und das Bild/Asset hat denselben Namen wie die zuvor veröffentlichte Version, aber es gibt ein Problem mit dem Bild. Beispielsweise haben Sie versehentlich eine Version mit niedriger Auflösung hochgeladen oder Ihr Bild wurde von höherer Seite nicht genehmigt. In diesem Fall sollten Sie das Originalbild abrufen und es durch eine neue Version mit derselben Asset-ID ersetzen.

Erfahren Sie, wie Sie [manuell den Cache für die zu aktualisierenden URLs löschen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=de).

>[!TIP]
>
>Um Probleme mit der Caching-Verzögerung zu vermeiden, arbeiten Sie immer im Voraus – einen Abend, einen Tag, zwei Wochen usw. Stellen Sie sicher, dass rechtzeitig eine Qualitätssicherung/Abnahme durchgeführt wird, wobei interne Parteien Ihre Arbeit vor der Veröffentlichung überprüfen. Auch wenn Sie einen Abend vorher gearbeitet haben, können Sie am selben Abend Änderungen vornehmen und erneut veröffentlichen. Am Morgen sind dann die zehn Stunden vergangen, und der Cache wird mit dem richtigen Bild aktualisiert.

- Weitere Informationen zum [Erstellen eines Veröffentlichungsvorgangs](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html?lang=de#creating-a-publish-job).
- Weitere Informationen zum [Veröffentlichen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html?lang=de).

## Schritt 3: Bereitstellung

Beachten Sie, dass das Endprodukt eines Dynamic Media Classic-Workflows eine URL ist, die auf das Asset verweist. Die URL kann auf ein einzelnes Bild, ein Bild-Set, ein Rotations-Set oder eine andere Sammlung von Bild-Sets oder ein Video verweisen. Sie müssen diese URL nehmen und etwas damit tun, z. B. Ihre HTML so bearbeiten, dass die `<IMG>`-Tags auf das Dynamic Media Classic-Bild anstatt auf ein Bild verweisen, das von Ihrer aktuellen Site stammt.

Im Bereitstellungsschritt müssen Sie diese URLs in Ihre Website, App, E-Mail-Kampagne oder einen anderen digitalen Touchpoint integrieren, auf dem das Asset angezeigt werden soll.

Beispiel für die Integration der Dynamic Media Classic-URL für ein Bild in eine Website:

![Bild](assets/main-workflow/example-url-1.png)

Die rot markierte URL ist das einzige spezifische Dynamic Media Classic-Element.

Ihr IT-Team oder Integrationspartner kann die Führung beim Schreiben und Ändern von Code übernehmen, um Dynamic Media Classic-URLs in Ihre Site zu integrieren. Adobe verfügt über ein Beratungs-Team, das durch technische, kreative oder allgemeine Beratung bei diesen Bemühungen helfen kann.

Bei komplexeren Lösungen wie Zoom-Viewern oder Viewern, die einen Zoom mit alternativen Ansichten kombinieren, verweist die URL normalerweise auf einen Viewer, der von Dynamic Media Classic gehostet wird. Auch innerhalb einer solchen URL ist ein Verweis auf eine Asset-ID.

Beispiel eines (rot markierten) Links, der ein Bild-Set in einem Viewer in einem neuen Popup-Fenster öffnet:

![Bild](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Sie müssen die Dynamic Media Classic-URLs in Ihre Website, App, E-Mail und andere digitale Touchpoints integrieren – Dynamic Media Classic kann das nicht für Sie tun!

## Anzeigen von Assets in einer Vorschau

Wahrscheinlich möchten Sie Ihre hochgeladenen, erstellten oder bearbeiteten Assets in der Vorschau anzeigen, um sicherzustellen, dass sie beim Besuch Ihrer Kundinnen und Kunden wie gewünscht angezeigt werden. Sie können auf das Vorschaufenster zugreifen, indem Sie entweder auf die **Vorschau**-Schaltfläche in der Miniaturansicht des Assets oben im **Bedienfeld „Durchsuchen/Erstellen“** klicken, oder über **Datei > Vorschau**. In einem Browser-Fenster wird die Vorschau des Assets angezeigt, das sich derzeit im Bedienfeld befindet, unabhängig davon, ob es sich um ein Bild, ein Video oder ein erstelltes Asset wie ein Bild-Set handelt.

### Vorschau der dynamischen Größe (Bildvorgaben)

Sie können eine Vorschau Ihrer Bilder in mehreren Größen anzeigen, indem Sie die **Größenvorschau** anzeigen. Dadurch wird eine Liste der verfügbaren Bildvorgaben geladen. Wir werden später über Bildvorgaben reden, aber stellen Sie sie sich als „Rezepte“ für das Laden Ihres Bildes in einer benannten Größe mit einem bestimmten Umfang an Scharfzeichnung und Bildqualität vor.

### Zoom-Vorschau

Sie können auch die **Zoom-Option** nutzen, um eine Vorschau Ihres Bildes in einer von vielen vordefinierten Zoom-Vorgaben anzuzeigen, die auf verschiedenen eingeschlossenen Zoom-Viewern basieren.

Hier finden Sie weitere Informationen zur [Vorschau von Assets](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html?lang=de).
