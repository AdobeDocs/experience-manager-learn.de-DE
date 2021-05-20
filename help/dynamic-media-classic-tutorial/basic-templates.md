---
title: Einführung in grundlegende Vorlagen
description: Erfahren Sie mehr über grundlegende Vorlagen in Dynamic Media Classic, bildbasierte Vorlagen, die vom Image-Server aufgerufen werden und aus Bildern und gerendertem Text bestehen. Eine Vorlage kann dynamisch über die URL geändert werden, nachdem die Vorlage veröffentlicht wurde. Sie erfahren, wie Sie eine Photoshop-PSD in Dynamic Media Classic hochladen, um sie als Grundlage für eine Vorlage zu verwenden. Erstellen Sie eine einfache Merchandising-Grundvorlage, die aus Bildebenen besteht. Fügen Sie Textebenen hinzu und ändern Sie sie mithilfe von Parametern. Erstellen Sie eine Vorlagen-URL und bearbeiten Sie das Bild dynamisch über den Webbrowser.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '6306'
ht-degree: 0%

---


# Einführung in grundlegende Vorlagen {#basic-templates}

In Dynamic Media Classic ist eine Vorlage ein Dokument, das nach der Veröffentlichung der Vorlage dynamisch über die URL geändert werden kann. Dynamic Media Classic bietet grundlegende Vorlagen, bildbasierte Vorlagen, die vom Image-Server aufgerufen werden und aus Bildern und gerendertem Text bestehen.

Einer der leistungsstärksten Aspekte von Vorlagen besteht darin, dass sie direkte Integrationspunkte haben, mit denen Sie sie mit Ihrer Datenbank verknüpfen können. So können Sie nicht nur ein Bild aufstellen und seine Größe ändern, Sie können Ihre Datenbank abfragen, um neue oder Verkaufselemente zu finden und diese als Überlagerung auf dem Bild erscheinen zu lassen. Sie können nach einer Beschreibung des Elements fragen und diese als Beschriftung in einer Schriftart, die Sie wählen und das Layout. Die Möglichkeiten sind grenzenlos.

Grundlegende Vorlagen können auf viele verschiedene Arten implementiert werden, von einfach bis komplex. Beispiel:

- Grundlegendes Merchandising. Verwendet Etiketten wie &quot;kostenloser Versand&quot;, wenn das Produkt kostenlosen Versand aufweist. Diese Beschriftungen werden vom Merchandise-Team in Photoshop eingerichtet und das Web verwendet Logik, um zu erfahren, wann sie auf das Bild angewendet werden.
- Erweitertes Merchandising. Jede Vorlage verfügt über mehrere Variablen und kann mehrere Optionen gleichzeitig anzeigen. Verwendet eine Datenbank, einen Bestand und Geschäftsregeln, um zu bestimmen, wann ein Produkt als &quot;Just In&quot;, &quot;Clearance&quot;oder &quot;Sold Out&quot;angezeigt werden soll. Kann auch Transparenz hinter dem Produkt verwenden, um es auf verschiedenen Hintergründen, wie in verschiedenen Räumen zu zeigen. Dieselben Vorlagen und/oder Assets können auf der Produktdetailseite erneut verwendet werden, um eine größere oder vergrößerbare Version desselben Produkts mit unterschiedlichen Hintergründen anzuzeigen.

Es ist wichtig zu verstehen, dass Dynamic Media Classic nur den visuellen Teil dieser vorlagenbasierten Anwendungen bereitstellt. Dynamic Media Classic-Unternehmen oder ihre Integrationspartner müssen die Geschäftsregeln, die Datenbank und Entwicklungsfähigkeiten bereitstellen, um die Anwendungen zu erstellen. Es gibt keine integrierte Vorlagenanwendung. Designer richten die Vorlage in Dynamic Media Classic ein und Entwickler verwenden URL-Aufrufe, um die Variablen in der Vorlage zu ändern.

Am Ende dieses Abschnitts des Tutorials erfahren Sie, wie Sie:

- Laden Sie eine Photoshop-PSD in Dynamic Media Classic hoch, um sie als Grundlage für eine Vorlage zu verwenden.
- Erstellen Sie eine einfache Merchandising-Grundvorlage, die aus Bildebenen besteht.
- Fügen Sie Textebenen hinzu und ändern Sie sie mithilfe von Parametern.
- Erstellen Sie eine Vorlagen-URL und bearbeiten Sie das Bild dynamisch über den Webbrowser.

>[!NOTE]
>
>Alle URLs in diesem Kapitel dienen nur Veranschaulichungszwecken. es sich nicht um Live-Links handelt.

## Übersicht über grundlegende Vorlagen

Die Definition einer einfachen Vorlage (oder kurz &quot;Vorlage&quot;) ist ein Bild mit URL-adressierbaren Ebenen. Das Endergebnis ist ein Bild, das jedoch durch die URL geändert werden kann. Es kann aus Fotos, Text oder Grafiken bestehen - einer beliebigen Kombination von P-TIFF-Assets in Dynamic Media Classic.

Vorlagen ähneln am ehesten Photoshop-PSD-Dateien, da sie einen ähnlichen Workflow und ähnliche Funktionen aufweisen.

- Beide bestehen aus Schichten, die wie Platten gestapelten Azetats sind. Sie können teilweise transparente Bilder zusammensetzen und durch die transparenten Bereiche einer Ebene zu den darunter liegenden Ebenen sehen.
- Die Ebenen können verschoben und gedreht werden, um den Inhalt neu zu positionieren. Die Deckkraft und die Mischmodi können geändert werden, um den Inhalt teilweise transparent zu machen.
- Sie können textbasierte Ebenen erstellen. Die Qualität kann sehr hoch sein, da der Image-Server dieselbe Text-Engine wie Photoshop und Illustrator verwendet.
- Einfache Ebenenstile können auf jede Ebene angewendet werden, um spezielle Effekte wie Schlagschatten oder Schein zu erzeugen.

Im Gegensatz zu Photoshop-PSDs können Ebenen jedoch vollständig dynamisch sein und über eine URL auf dem Image-Server gesteuert werden.

- Sie können allen Vorlageneigenschaften Variablen hinzufügen, um die Komposition schnell und einfach zu ändern.
- Mit Variablen, die als Parameter bezeichnet werden, können Sie nur den Teil der Vorlage verfügbar machen, den Sie ändern möchten.

Sie müssen nur für jede Ebene einen Platzhalter hinzufügen, der variiert, anstatt alle Ebenen wie in Photoshop in eine Datei zu verschieben und sie ein- und auszublenden (auch wenn Sie dies ggf. auch tun können).

Mithilfe eines Platzhalters können Sie den Inhalt einer Ebene dynamisch mit einem anderen veröffentlichten Asset austauschen. Dabei werden automatisch dieselben Eigenschaften (z. B. Größe und Drehung) der Ebene verwendet, die ersetzt wurde.

Da grundlegende Vorlagen in der Regel in Photoshop entworfen, aber über eine URL bereitgestellt werden, erfordert ein Vorlagenprojekt eine Mischung aus Design- und technischen Fertigkeiten. Im Allgemeinen gehen wir davon aus, dass der Benutzer, der die kreativen Vorlagen bearbeitet, ein Photoshop-Designer ist und der Implementierer der Vorlage ein Web-Entwickler ist. Die Kreativ- und Entwicklungsteams müssen eng zusammenarbeiten, damit die Vorlage erfolgreich ist.

Vorlagenprojekte können je nach Geschäftsregeln und Anforderungen der Anwendung relativ einfach oder äußerst komplex sein. Grundlegende Vorlagen werden vom Image-Server aufgerufen. Aufgrund der Flexibilität der Dynamic Media Classic-Umgebung können Sie jedoch auch Vorlagen in anderen Vorlagen verschachteln, sodass Sie relativ komplexe Bilder erstellen können, die mit häufig benannten Variablen verknüpft werden können.

- Erfahren Sie mehr über [Vorlagengrundlagen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/quick-start-template-basics.html).
- Erfahren Sie, wie Sie eine [einfache Vorlage](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#creating_a_template) erstellen.

## Erstellen einer einfachen Vorlage

Beim Arbeiten mit einer einfachen Vorlage folgen Sie normalerweise den Workflow-Schritten im unten stehenden Diagramm. Schritte, die mit gepunkteten Linien markiert sind, sind optional, wenn Sie dynamische Textebenen verwenden. Sie werden in den Anweisungen unten als &quot;Text-Workflow&quot;angegeben. Wenn Sie keinen Text verwenden, folgen Sie nur dem Hauptpfad.

![image](assets/basic-templates/basic-templates-1.png)

_Workflow &quot;Grundlegende Vorlage&quot;._

1. Entwerfen und erstellen Sie Ihre Assets. Die meisten Benutzer tun dies in Adobe Photoshop. Entwerfen Sie Assets in der gewünschten Größe - wenn es sich um ein 200-Pixel-Bild für eine Miniaturseite handelt, erstellen Sie sie mit 200 Pixel. Wenn Sie darauf zoomen müssen, erstellen Sie es mit einer Größe von etwa 2000 Pixel. Verwenden Sie Photoshop (und/oder Illustrator, die als Bitmap gespeichert sind), um die Assets zu erstellen, und verwenden Sie Dynamic Media Classic, um die Teile zusammenzuführen, die Ebenen zu verwalten und Variablen hinzuzufügen.
2. Laden Sie nach dem Entwerfen von Grafik-Assets diese in Dynamic Media Classic hoch. Anstatt einzelne Assets aus der PSD hochzuladen, empfehlen wir, die gesamte PSD-Datei mit Ebenen hochzuladen und Dynamic Media Classic zu veranlassen, eine Datei pro Ebene zu erstellen, indem Sie beim Hochladen die Option **Ebenen beibehalten** verwenden (weitere Informationen finden Sie unten). _Text-Workflow: Wenn Sie dynamischen Text erstellen, laden Sie auch Ihre Schriftarten hoch. Dynamischer Text ist variabel und wird über die URL gesteuert. Wenn Ihr Text statisch ist oder nur wenige kurze Ausdrücke enthält, die sich nicht ändern - z. B. Tags, die &quot;Neu&quot;oder &quot;Verkauf&quot;anstelle von &quot;X% Aus&quot;mit dem X als Variablennummer angeben -, empfehlen wir, den Text in Photoshop vorab zu rendern und als gerasterte Ebenen als Bilder hochzuladen. Dies wird einfacher und Sie können den Text genau nach Bedarf gestalten._
3. Erstellen Sie die Vorlage in Dynamic Media Classic mithilfe des Vorlagen-Basics-Editors im Menü &quot;Erstellen&quot;und fügen Sie Bildebenen hinzu. Text-Workflow: Erstellen Sie Textebenen im selben Editor. Dieser Schritt ist beim manuellen Erstellen einer Vorlage in Dynamic Media Classic erforderlich. Wählen Sie eine Arbeitsflächengröße aus, die Ihrem Design entspricht, ziehen Sie Bilder auf die Arbeitsfläche und legen Sie die Ebeneneigenschaften fest (Größe, Drehung, Deckkraft usw.). Sie platzieren nicht alle möglichen Ebenen auf Ihrer Vorlage, sondern nur einen Platzhalter pro Bildebene. _Text-Workflow: Mit dem Text-Tool können Sie Textebenen erstellen, ähnlich wie Textebenen in Photoshop. Sie können eine Schriftart auswählen und sie mit den gleichen Optionen formatieren, die mit dem Tool Photoshop-Typ verfügbar sind._ Ein weiterer Workflow besteht darin, eine PSD hochzuladen und von Dynamic Media Classic eine &quot;kostenlose&quot;Vorlage zu generieren und sogar Textebenen neu zu erstellen. Hierauf gehen wir später noch genauer ein.
4. Nachdem die Ebenen erstellt wurden, fügen Sie Parameter (Variablen) zu jeder Eigenschaft jeder Ebene hinzu, die Sie über die URL steuern möchten, einschließlich der Quelle der Ebene (das Bild selbst ). _Text-Workflow: Sie können auch Parameter zu Textebenen hinzufügen, um den Inhalt und die Größe und Position der Ebene selbst sowie alle Formatierungsoptionen wie Schriftfarbe, Schriftgröße, horizontales Tracking usw. zu steuern._
5. Erstellen Sie eine Bildvorgabe, die der Größe Ihrer Vorlage entspricht. Es wird empfohlen, dies so zu tun, dass die Vorlage immer in einer Größe von 1:1 aufgerufen wird und dass auch allen großen Bildebenen, deren Größe an die Vorlage angepasst wird, eine Scharfzeichnung hinzugefügt wird. Wenn Sie eine Vorlage erstellen, die vergrößert werden soll, ist dieser Schritt nicht erforderlich.
6. Veröffentlichen Sie die URL, kopieren Sie sie aus der Dynamic Media Classic-Vorschau und testen Sie sie in einem Browser.

## Vorbereiten und Hochladen von Vorlagenassets in Dynamic Media Classic

Bevor Sie Ihre Vorlagen-Assets in Dynamic Media Classic hochladen, müssen Sie einige vorbereitende Schritte durchführen.

### PSD für das Hochladen vorbereiten

Bevor Sie Ihre Photoshop-Datei in Dynamic Media Classic hochladen, vereinfachen Sie die Ebenen in Photoshop, um die Arbeit mit und die größtmögliche Kompatibilität mit dem Image-Server zu erleichtern. Ihre PSD-Datei besteht oft aus vielen Elementen, die von Dynamic Media Classic nicht erkannt werden, und Sie können auch mit vielen kleinen Elementen enden, die schwierig zu handhaben sind. Achten Sie darauf, eine Sicherungskopie Ihrer Übergeordneten PSD zu speichern, falls Sie das Original später bearbeiten müssen. Sie laden die vereinfachte Kopie hoch, nicht die Übergeordnete.

![image](assets/basic-templates/basic-templates-2.jpg)

1. Vereinfachen Sie die Ebenenstruktur durch Zusammenführen/Reduzieren verwandter Ebenen, die zusammen in eine einzige Ebene eingeschaltet/deaktiviert werden müssen. Beispielsweise werden die Bezeichnung &quot;NEU&quot;und das blaue Banner in einer Ebene zusammengeführt, sodass Sie sie mit einem Klick ein- oder ausblenden können.
   ![image](assets/basic-templates/basic-templates-3.jpg)
2. Einige Ebenentypen und Ebeneneffekte werden von Dynamic Media Classic oder dem Image-Server nicht unterstützt und müssen vor dem Hochladen gerastert werden. Andernfalls können die Effekte ignoriert oder die Ebenen verworfen werden. Das Rastern einer Ebene bedeutet, dass die bearbeitbare Ebene in nicht bearbeitbar umgewandelt wird. Um Ebeneneffekte oder Textebenen zu rastern, erstellen Sie eine leere Ebene, wählen Sie beide aus und führen Sie sie mit **Ebenen > Ebenen zusammenführen** oder STRG + E/CMD + E zusammen.

   - Dynamic Media Classic kann Ebenen nicht gruppieren oder verknüpfen. Alle Ebenen einer Gruppe oder eines verknüpften Sets werden in separate Ebenen konvertiert, die nicht mehr gruppiert/verknüpft sind.
   - Ebenenmasken werden beim Hochladen in Transparenz umgewandelt.
   - Einstellungsebenen werden nicht unterstützt und verworfen.
   - Füllschichten, wie z. B. Farbschichten, werden gerastert.
   - Smart-Objekt-Ebenen und Vektorebenen werden beim Hochladen in normale Bilder gerastert und Smart-Filter werden angewendet und gerastert.
   - Textebenen werden auch gerastert, es sei denn, Sie verwenden die Option Text extrahieren . Weitere Informationen finden Sie unten.
   - Die meisten Ebeneneffekte werden ignoriert und nur wenige Mischmodi werden unterstützt. Fügen Sie im Zweifelsfall einfache Effekte in Dynamic Media Classic hinzu (z. B. Innen- oder Ablageschatten, Innen- oder Außengläser) oder verwenden Sie eine leere Ebene, um den Effekt in Photoshop zusammenzuführen und zu rastern.

### Arbeiten mit Schriftarten

Wenn Sie dynamischen Text generieren möchten, laden Sie Ihre Schriftarten hoch und veröffentlichen Sie sie. Die einzige in Dynamic Media Classic enthaltene Schriftart ist Arial.

Es liegt in der Verantwortung jedes Unternehmens, eine Lizenz für die Verwendung einer Schriftart im Internet zu erhalten - einfach wenn eine Schriftart auf Ihrem Computer installiert ist, haben Sie kein Recht, sie kommerziell im Internet zu verwenden. Ihr Unternehmen könnte sich dem Schriftverlag vor Gericht stellen, wenn es ohne Genehmigung verwendet wird. Außerdem variieren die Lizenzbedingungen. Möglicherweise benötigen Sie separate Lizenzen für die Druck- oder Bildschirmanzeige.

Dynamic Media Classic unterstützt standardmäßige OpenType- (OTF-), TrueType- (TTF-) und Type 1-Postscript-Schriftarten. Nur Mac-Kofferschriftarten, Typensammlungsdateien, Windows-Systemschriftarten und proprietäre Maschinenschriftarten (wie Schriftarten, die von Gravierungs- oder Stickereimaschinen verwendet werden) werden nicht unterstützt. Sie müssen sie in eines der Standardschriftformate konvertieren oder eine ähnliche Schriftart ersetzen, die in Dynamic Media Classic und auf dem Image-Server verwendet werden kann.

Nachdem Schriftarten wie jedes andere Asset in Dynamic Media Classic hochgeladen wurden, müssen sie auch auf dem Image-Server veröffentlicht werden. Ein sehr häufiger Vorlagenfehler besteht darin, die Veröffentlichung Ihrer Schriftarten zu vergessen, was zu einem Bildfehler führt. Der Image-Server ersetzt an seiner Stelle keine andere Schriftart. Wenn Sie außerdem beim Hochladen die Option **Text extrahieren** verwenden möchten, müssen Sie Ihre Schriftartdateien hochladen, bevor Sie die PSD hochladen, die diese Schriftarten verwendet. Mit der Funktion **Text extrahieren** wird versucht, Ihren Text als bearbeitbare Textebene neu zu erstellen und ihn in einer Dynamic Media Classic-Vorlage zu platzieren. Dies wird im nächsten Thema PSD-Optionen erläutert.

Beachten Sie, dass Schriftarten mehrere interne Namen haben, die sich häufig von ihrem externen Dateinamen unterscheiden. Sie können alle zugehörigen Namen auf der Detailseite für dieses Asset in Dynamic Media Classic sehen. Im Folgenden finden Sie die Namen der Schriftart Adobe Caslon Pro Semifold, die auf der Registerkarte Metadaten in Dynamic Media Classic aufgeführt ist:

![image](assets/basic-templates/basic-templates-4.jpg)

_Registerkarte &quot;Metadaten&quot;auf der Detailseite für eine Schriftart in Dynamic Media Classic._

Dynamic Media Classic verwendet den Dateinamen dieser Schriftart (ACaslonPro-Semibold) als Asset-ID. Dies ist jedoch nicht der von der Vorlage verwendete Name. Die Vorlage verwendet den unten aufgeführten Rich-Text-Format-Namen (RTF). RTF ist die native &quot;Sprache&quot;der Image Server-Textmodul.

Wenn Sie Schriftarten über die URL ändern müssen, müssen Sie zum RTF-Namen der Schriftart (nicht zur Asset-ID) aufrufen. Andernfalls wird ein Fehler ausgegeben. In diesem Fall wäre der richtige Name für diese Schriftart &quot;Adobe Caslon Pro&quot;. Wir werden weiter über Schriftarten und RTF im Thema RTF und Text Parameter weiter unten diskutieren.

Die gängigsten Schriftartendateiformate auf Windows- und Mac-Systemen sind OpenType und TrueType. OpenType verfügt über eine .OTF-Erweiterung, während TrueType .TTF ist. Beide Formate funktionieren in Dynamic Media Classic gleichermaßen gut.

### Auswählen von Optionen beim Hochladen der PSD

Sie müssen keine Photoshop-Datei (PSD) hochladen, um eine Vorlage zu erstellen. Eine Vorlage kann aus allen Bild-Assets in Dynamic Media Classic erstellt werden. Das Hochladen einer PSD kann das Authoring jedoch vereinfachen, da diese Assets normalerweise bereits in einer PSD mit Ebenen enthalten sind. Darüber hinaus generiert Dynamic Media Classic automatisch eine Vorlage, wenn Sie eine PSD mit Ebenen hochladen.

- **Ebenen beibehalten.** Dies ist die wichtigste Option. Dadurch wird Dynamic Media Classic angewiesen, pro Photoshop-Ebene ein Bild-Asset zu erstellen. Wenn diese Option deaktiviert ist, werden alle anderen Optionen deaktiviert und die PSD wird in ein einzelnes Bild reduziert.
- **** **CreateTemplate.** Bei Auswahl dieser Option werden die verschiedenen erzeugten Ebenen übernommen und automatisch eine Vorlage erstellt, indem diese wieder kombiniert wird. Ein Nachteil bei der Verwendung der automatisch generierten Vorlage besteht darin, dass Dynamic Media Classic alle Ebenen in einer Datei platziert, während wir nur einen einzigen Platzhalter pro Ebene benötigen. Es ist einfach genug, die zusätzlichen Ebenen zu löschen, aber wenn Sie viele Ebenen haben, ist es schneller, sie neu zu erstellen. Benennen Sie die neue Vorlage um. Wenn Sie dies nicht tun, wird es beim nächsten erneuten Hochladen derselben PSD überschrieben.
- **Text extrahieren.** Dadurch werden Textebenen in der PSD als Textebenen in der Vorlage unter Verwendung der von Ihnen hochgeladenen Schriftart neu erstellt. Dieser Schritt ist erforderlich, wenn sich Ihr Text in Photoshop auf einem Pfad befindet und Sie diesen Pfad in der Vorlage beibehalten möchten. Für diese Funktion müssen Sie die Option **Vorlage erstellen** verwenden, da der extrahierte Text nur von einer beim Hochladen generierten Vorlage erstellt werden kann.
- **Ebenen auf Hintergrundgröße ausdehnen.** Durch diese Einstellung wird jede Ebene auf dieselbe Größe wie die gesamte PSD-Arbeitsfläche eingestellt. Dies ist sehr nützlich für Ebenen, die immer fest in Position bleiben: andernfalls müssen Sie Bilder beim Austausch in dieselbe Ebene möglicherweise neu positionieren.
- **Ebenenbenennung.** Dadurch wird Dynamic Media Classic erläutert, wie die einzelnen Assets, die pro Ebene generiert wurden, benannt werden. Wir empfehlen entweder **Photoshop** **und Layer** **Name** oder Photoshop und **Layer** **Number**. Beide Optionen verwenden den PSD-Namen als ersten Teil des Namens und fügen entweder den Ebenennamen oder die Nummer am Ende hinzu. Wenn Sie beispielsweise eine PSD mit dem Namen &quot;shirt.psd&quot;haben und sie Ebenen mit dem Namen &quot;front&quot;, &quot;sleeves&quot;und &quot;kragen&quot;aufweist, wenn Sie die Option **Photoshop und** Layer **Name** hochladen, generiert Dynamic Media Classic die Asset-IDs &quot;shirt_front,&quot;&quot;shirt_sleeves,&quot;und &quot;shirt_shirt_shirt Kragen.&quot; Die Verwendung einer dieser Optionen hilft sicherzustellen, dass der Name in Dynamic Media Classic eindeutig ist.

## Erstellen einer Vorlage mit Bildebenen

Obwohl Dynamic Media Classic eine Vorlage automatisch aus einer PSD mit Ebenen erstellen kann, sollten Sie wissen, wie die Vorlage manuell erstellt wird. Wie oben erläutert, kann es vorkommen, dass Sie die von Dynamic Media Classic erstellte Vorlage nicht verwenden möchten.

### Die Benutzeroberfläche &quot;Vorlagengrundlagen&quot;

Machen wir uns zunächst mit der Benutzeroberfläche vertraut.

In der linken Mitte befindet sich Ihr Arbeitsbereich mit einer Vorschau Ihrer endgültigen Vorlage. Auf der rechten Seite befinden sich die Bereiche Ebenen und Ebeneneigenschaften . In diesen Bereichen werden Sie am meisten arbeiten.

![image](assets/basic-templates/basic-templates-5.jpg)

_Seite &quot;Grundlagen der Vorlage erstellen&quot;._

- **Vorschau/Arbeitsbereich.** Dies ist das Hauptfenster. Hier können Sie Ebenen mit der Maus verschieben, verkleinern und drehen. Ebenenentwürfe werden als gestrichelte Linien angezeigt.
- **Ebenen.** Dies ähnelt dem Bereich für Photoshop-Ebenen . Wenn Sie Ihrer Vorlage Ebenen hinzufügen, werden diese hier angezeigt. Ebenen werden von oben nach unten gestapelt. Die oberste Ebene im Ebenenbedienfeld wird über den anderen darunter in der Liste angezeigt.
- **Ebeneneigenschaften.** Hier können Sie alle Eigenschaften einer Ebene mithilfe numerischer Steuerelemente anpassen. Wählen Sie zuerst eine Ebene aus und passen Sie dann deren Eigenschaften an.
- **** **CompositeURL.** Am unteren Rand der Benutzeroberfläche befindet sich der Bereich &quot;Composite URL&quot;. Dies wird in diesem Abschnitt des Tutorials nicht besprochen. Hier wird Ihre Vorlage jedoch als eine Reihe von Image Serving URL-Modifikatoren dekonstruiert. Dieser Bereich kann bearbeitet werden. Wenn Sie mit Image-Server-Befehlen sehr vertraut sind, können Sie die Vorlage hier manuell bearbeiten. Sie können es jedoch auch beschädigen. Wie Photoshop beginnt die Ebenennummerierung bei 0. Die Arbeitsfläche ist Ebene 0, und die erste Ebene, die Sie selbst hinzufügen, ist Ebene 1. Die Mischmodi bestimmen, wie die Pixel einer Ebene mit den darunter liegenden Pixeln verschmelzen. Mithilfe von Mischmodi können Sie eine Vielzahl von Spezialeffekten erstellen.

#### Verwenden des Vorlagen-Grundlagen-Editors

Im Folgenden finden Sie die Workflow-Schritte zum Starten Ihrer einfachen Vorlage:

1. Gehen Sie in Dynamic Media Classic zu **Build > Vorlagengrundlagen**. Sie können entweder nichts auswählen oder ein Bild auswählen, das zur ersten Ebene Ihrer Vorlage wird.
2. Wählen Sie eine Größe aus und drücken Sie **OK**. Diese Größe sollte mit der in Photoshop entworfenen Größe übereinstimmen. Der Vorlageneditor wird geladen.
3. Wenn Sie in Schritt 1 kein Bild ausgewählt haben, suchen oder navigieren Sie im Asset-Bedienfeld auf der linken Seite zu einem Bild und ziehen Sie es in den Arbeitsbereich.

   - Die Größe des Bildes wird automatisch an die Größe der Arbeitsfläche angepasst. Wenn Sie Ihre hochauflösenden Bilder austauschen möchten, würden Sie normalerweise eines Ihrer großen (2000 px) P-TIFF-Bilder einbringen und als Platzhalter verwenden.
   - Dies sollte die unterste Ebene Ihrer Vorlage sein, Sie können die Ebenen jedoch später neu anordnen.

4. Ändern Sie die Größe oder Position der Ebene direkt im Arbeitsbereich oder passen Sie die Einstellungen im Bereich Ebeneneigenschaften an.
5. Ziehen Sie nach Bedarf weitere Bildebenen hinzu. Fügen Sie bei Bedarf Ebeneneffekte hinzu. Siehe das Thema _Hinzufügen von Ebeneneffekten_ weiter unten.
6. Klicken Sie auf **Save**, wählen Sie einen Speicherort aus und geben Sie der Vorlage einen Namen. Sie können eine Vorschau anzeigen, aber an dieser Stelle wird Ihre Vorlage genau wie ein reduziertes Photoshop-Bild aussehen - es kann noch nicht geändert werden.

### Hinzufügen von Ebeneneffekten

Der Image-Server unterstützt einige programmatische Ebeneneffekte - Spezialeffekte, die das Erscheinungsbild von Ebeneninhalten ändern. Sie funktionieren ähnlich wie Ebeneneffekte in Photoshop. Sie werden an eine Ebene angehängt, aber unabhängig von der Ebene gesteuert. Sie können sie anpassen oder entfernen, ohne eine dauerhafte Änderung an der Ebene selbst vorzunehmen.

- **Schlagschatten**. Wendet einen Schatten außerhalb der Ebenengrenzen an, der durch einen x- und y-Pixelversatz positioniert wird.
- **Schatten nach innen**. Wendet einen Schatten innerhalb der Ebenengrenzen an, der durch einen x- und y-Pixelversatz positioniert wird.
- **Äußeres Leuchten**. Wendet einen Glüheffekt gleichmäßig auf alle Kanten der Ebene an.
- **Glühen nach innen**. Wendet einen Glüheffekt gleichmäßig innerhalb aller Kanten der Ebene an.

![image](assets/basic-templates/basic-templates-6.jpg)

_Eine Ebene mit und ohne Schlagschatten_

Um einen Effekt hinzuzufügen, klicken Sie auf **Effekt hinzufügen** und wählen Sie einen Effekt aus dem Menü aus. Wie normale Ebenen können Sie einen Effekt im Ebenenbereich auswählen und die Einstellungen im Bereich Ebeneneigenschaften anpassen.

Schatteneffekte werden horizontal oder vertikal von der Ebene weg versetzt, während Schatteneffekte gleichmäßig in alle Richtungen angewendet werden. Inner-Effekte wirken auf die undurchsichtigen Teile der Ebene, während Außeneffekte nur die transparenten Bereiche betreffen.

Erfahren Sie mehr über[Hinzufügen von Ebeneneffekten](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template.html#using-shadow-and-glow-effects-on-layers).

### Parameter hinzufügen

Wenn Sie nur Ebenen kombinieren und speichern, unterscheidet sich das Nettoergebnis nicht von einem reduzierten Photoshop-Bild. Besonders wichtig ist die Möglichkeit, den Eigenschaften jeder Ebene Parameter hinzuzufügen, damit sie dynamisch über die URL geändert werden können.

In Dynamic Media Classic ist ein Parameter eine Variable, die mit einer Vorlageneigenschaft verknüpft werden kann, damit sie über eine URL bearbeitet werden kann. Wenn Sie einen Parameter zu einer Ebene hinzufügen, legt Dynamic Media Classic diese Eigenschaft in der URL offen, indem Sie dem Parameternamen ein Dollarzeichen ($) voranstellen. Wenn Sie beispielsweise einen Parameter namens &quot;size&quot;erstellen, um die Größe einer Ebene zu ändern, benennt Dynamic Media Classic Ihren Parameter $size um.

Wenn Sie keinen Parameter für eine Eigenschaft hinzufügen, bleibt diese Eigenschaft in der Dynamic Media Classic-Datenbank verborgen und wird nicht in der URL angezeigt.

![image](assets/basic-templates/parameters.png)

Ohne Parameter wären Ihre URLs in der Regel viel länger, insbesondere wenn Sie auch dynamischen Text verwenden. Text fügt viele Dutzende von zusätzlichen Zeichen zu jeder URL hinzu.

Schließlich wird aus Ihrem anfänglichen Parametersatz der Standardwert der Eigenschaften in der Vorlage. Wenn Sie Ihre Vorlage erstellen, Parameter hinzufügen und dann die URL ohne ihre Parameter aufrufen, erstellt der Image-Server das Bild mit allen Standardeinstellungen, die Sie in der Vorlage gespeichert haben. Parameter werden nur benötigt, wenn Sie eine Eigenschaft ändern möchten. Wenn eine Eigenschaft nicht geändert werden muss, müssen Sie keinen Parameter festlegen.

#### Erstellen von Parametern

Dies ist der Workflow zum Erstellen von Parametern:

1. Klicken Sie auf die Schaltfläche **Parameter** neben dem Namen der Ebene, für die Sie Parameter erstellen möchten. Der Bildschirm Parameter wird geöffnet. Es listet jede Eigenschaft auf der Ebene und deren Wert auf.
1. Wählen Sie die Option **On** neben dem Namen jeder Eigenschaft aus, die Sie in einen Parameter umwandeln möchten. Es wird ein standardmäßiger Parametername angezeigt. Sie können nur Parameter zu Eigenschaften hinzufügen, die sich vom Standardstatus geändert haben.

   - Wenn Sie z. B. eine Ebene hinzufügen und sie an ihrer standardmäßigen xy-Position 0,0 belassen, stellt Dynamic Media Classic keine **Position**-Eigenschaft bereit. Um das Problem zu beheben, verschieben Sie die Ebene mindestens ein Pixel. Jetzt stellt Dynamic Media Classic **Position** als Eigenschaft bereit, die Sie parametrisieren können.
   - Um der Eigenschaft &quot;Anzeigen/Verbergen&quot;einen Parameter hinzuzufügen (wodurch die Ebene ein- und ausgeschaltet wird), klicken Sie auf das Symbol **Anzeigen** oder **Ebene ausblenden** , um die Ebene zu deaktivieren (Sie können die Ebene bei Bedarf wieder einschalten). Dynamic Media Classic stellt jetzt eine **Eigenschaft zum Ausblenden** bereit, die parametrisiert werden kann.

1. Benennen Sie die standardmäßigen Parameternamen in einen Parameter um, der in der URL leichter zu identifizieren ist. Wenn Sie beispielsweise einen Parameter hinzufügen möchten, um die Bannerschicht auf einem Bild zu ändern, ändern Sie den Standardnamen von &quot;layer_2_src&quot;in &quot;banner&quot;.
1. Drücken Sie **Close** , um den Bildschirm &quot;Parameter&quot;zu verlassen.
1. Wiederholen Sie diesen Vorgang für andere Ebenen, indem Sie auf die Schaltfläche **Parameter** klicken und Parameter hinzufügen und umbenennen.
1. Speichern Sie Ihre Änderungen nach Abschluss.

>[!TIP]
>
>Benennen Sie Ihre Parameter in eine aussagekräftige Bezeichnung um und entwickeln Sie eine Benennungskonvention, um diese Namen zu standardisieren. Stellen Sie sicher, dass die Benennungskonvention im Voraus sowohl von den Design- als auch von den Entwicklungsteams vereinbart wird.
>
>Kann keinen Parameter hinzufügen, da die Eigenschaft nicht angezeigt wird? Ändern Sie einfach die Eigenschaft der Ebene von der Standardeinstellung (durch Verschieben, Ändern der Größe, Ausblenden usw.). Diese Eigenschaft sollte jetzt angezeigt werden.

Erfahren Sie mehr über [Vorlagenparameter](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html).

## Erstellen einer Vorlage mit Textebenen

Jetzt erfahren Sie, wie Sie eine einfache Vorlage erstellen, die Textebenen enthält.

### Grundlegendes zu dynamischem Text

Sie wissen jetzt, wie Sie eine einfache Vorlage mit Bildebenen erstellen. Für viele Anwendungen ist dies alles, was Sie benötigen. Wie Sie in der vorherigen Übung gesehen haben, können Ebenen mit einfachem Text (z. B. &quot;Verkauf&quot;und &quot;Neu&quot;) gerastert und als Bilder behandelt werden, da ihr Text nicht geändert werden muss.

Was ist jedoch erforderlich, wenn Sie:

- Fügen Sie eine Bezeichnung hinzu, um &quot;25 % aus&quot;mit dem Wert &quot;25 % aus&quot;variabel zu machen.
- Fügen Sie eine Textbeschriftung mit dem Namen des Produkts über dem Bild hinzu.
- Lokalisieren Sie Ihre Ebenen in verschiedenen Sprachen, je nach Land, in dem Ihre Vorlage angezeigt wird.

In diesem Fall sollten Sie einige dynamische Textebenen mit Parametern hinzufügen, um den Text und/oder die Formatierung zu steuern.

Um Text zu erstellen, müssen Sie einige Schriftarten hochladen. Andernfalls wird Dynamic Media Classic standardmäßig Arial verwendet. Die Schriftarten müssen auch auf dem Image-Server veröffentlicht werden. Andernfalls wird ein Fehler ausgegeben, wenn versucht wird, Text zu rendern, der diese Schriftart verwendet.

### RTF- und Text-Parameter

Um mit dem Tool &quot;Vorlagen-Grundlagen&quot;Variablen zu Text hinzuzufügen, sollten Sie wissen, wie Text gerendert wird. Der Image-Server generiert Text mit der Adobe Text Engine, die von Photoshop und Illustrator verwendet wird, und stellt ihn als Ebene im endgültigen Bild zusammen. Um mit der Engine zu kommunizieren, verwendet der Image-Server Rich Text Format (RTF).

RTF ist eine Dateiformatspezifikation, die von Microsoft zur Angabe der Formatierung von Dokumenten entwickelt wurde. Es handelt sich dabei um eine standardmäßige Markup-Sprache, die von den meisten Textverarbeitungs- und E-Mail-Programmen verwendet wird. Wenn Sie in eine URL geschrieben haben, &quot;&amp;text=\b1 Hallo&quot;, generiert der Image-Server ein Bild mit dem Wort &quot;Hallo&quot;im Fettdruck, da \b1 der RTF-Befehl ist, um den Text fett zu formatieren.

Die gute Nachricht ist, dass Dynamic Media Classic das RTF für Sie generiert. Wenn Sie Text in eine Vorlage eingeben und Formatierungen hinzufügen, schreibt Dynamic Media Classic den RTF-Code still in die Vorlage. Der Grund, warum wir es erwähnen, ist, dass Sie Parameter direkt zum RTF hinzufügen werden, also ist es wichtig, dass Sie sich damit ein wenig auskennen.

#### Erstellen von Textebenen

Sie können Textebenen in einer Vorlage in Dynamic Media Classic auf zwei Arten erstellen:

1. Text-Tool in Dynamic Media Classic. Wir werden diese Methode unten besprechen. Der Vorlagen-Basics-Editor verfügt über ein Tool, mit dem Sie ein Textfeld erstellen, Text eingeben und den Text formatieren können. Dynamic Media Classic generiert das RTF nach Bedarf und platziert es in einer separaten Ebene.
2. Text extrahieren (beim Hochladen). Die andere Methode besteht darin, die Textebene in Photoshop zu erstellen und als normale Textebene in der PSD zu speichern (anstatt sie als Bildebene zu rastern). Laden Sie die Datei dann in Dynamic Media Classic hoch und verwenden Sie die Option **Text extrahieren** . Dynamic Media Classic konvertiert jede Photoshop-Textebene mithilfe von RTF-Befehlen in eine Image Serving-Textebene. Wenn Sie diese Methode verwenden, stellen Sie sicher, dass Sie Ihre Schriftarten zuerst in Dynamic Media Classic hochladen. Andernfalls ersetzt Dynamic Media Classic beim Hochladen eine Standardschriftart und es gibt keine einfache Möglichkeit, die richtige Schriftart erneut zu ersetzen.

### Der Texteditor

Sie geben Text mit dem Texteditor ein. Der Texteditor ist eine WYSIWYG-Benutzeroberfläche, mit der Sie Text mit Formatierungssteuerelementen eingeben und formatieren können, die denen in Photoshop oder Illustrator ähneln.

![image](assets/basic-templates/basic-templates-9.jpg)

_Text-Editor für Vorlagengrundlagen_

Den Großteil Ihrer Arbeit erledigen Sie im Tab **Vorschau**, wo Sie Text eingeben und sehen können, wie er in der Vorlage aussehen wird. Es gibt auch eine Registerkarte **Quelle** , die bei Bedarf zum manuellen Bearbeiten des RTF verwendet wird.

Der allgemeine Workflow besteht darin, den Tab **Vorschau** zu verwenden, um Text einzugeben.

Wählen Sie dann den Text aus und wählen Sie mithilfe der Steuerelemente oben eine Formatierung wie Schriftfarbe, Schriftgröße oder Ausrichtung aus. Nachdem der Text wie gewünscht formatiert wurde, klicken Sie auf **Anwenden** , um die Aktualisierung in der Vorschau des Arbeitsbereichs anzuzeigen. Schließen Sie dann den Texteditor, um zum Hauptfenster &quot;Vorlagengrundlagen&quot;zurückzukehren.

#### Verwenden des Texteditors

Im Folgenden finden Sie die Workflow-Schritte zum Hinzufügen von Text auf der Build-Seite &quot;Vorlagengrundlagen&quot;:

1. Klicken Sie oben auf der Build-Seite auf die Schaltfläche **Text** .
2. Ziehen Sie ein Textfeld heraus, in dem Text angezeigt werden soll. Das Fenster Texteditor wird in einem modalen Fenster geöffnet. Im Hintergrund wird die Vorlage angezeigt, sie kann jedoch erst bearbeitet werden, nachdem Sie die Bearbeitung abgeschlossen haben.
3. Geben Sie den Beispieltext ein, der beim ersten Laden der Vorlage angezeigt werden soll. Wenn Sie beispielsweise ein Textfeld für ein personalisiertes E-Mail-Bild erstellen, lautet Ihr Text möglicherweise &quot;Hi Name&quot;. Jetzt ist es Zeit zu sparen!&quot; Später würden Sie einen Textparameter hinzufügen, um den Namen durch einen Wert zu ersetzen, den Sie für die URL senden. Ihr Text wird erst dann in der Vorlage unter dem Fenster angezeigt, wenn Sie auf **Anwenden** klicken.
4. Um den Text zu formatieren, wählen Sie ihn aus, indem Sie ihn mit der Maus ziehen und ein Formatierungssteuerelement in der Benutzeroberfläche auswählen.

   - Es gibt viele Formatierungsoptionen. Einige der häufigsten sind Schriftart (Gesicht), Schriftgröße und Schriftfarbe sowie linke/mittlere/rechte Ausrichtung.
   - Vergessen Sie nicht, zuerst den Text auszuwählen. Andernfalls können Sie keine Formatierung anwenden.
   - Um eine andere Schriftart auszuwählen, wählen Sie den Text aus und öffnen Sie das Menü Schriftart . Der Editor zeigt eine Liste aller Schriftarten an, die in Dynamic Media Classic hochgeladen wurden. Wenn eine Schrift auch auf Ihrem Computer installiert ist, wird sie schwarz angezeigt. Wenn es nicht auf Ihrem Computer installiert ist, wird es rot angezeigt. Sie wird jedoch weiterhin im Vorschaufenster gerendert, wenn Sie auf **Apply** klicken. Sie müssen nur Schriftarten in Dynamic Media Classic hochladen, damit sie für alle Benutzer verfügbar sind, die Dynamic Media Classic verwenden. Nach der Veröffentlichung verwendet der Image-Server diese Schriftarten, um den Text zu generieren. Ihre Benutzer müssen keine Schriftarten installieren, um den von Ihnen erstellten Text zu sehen, da er Teil eines Bildes ist.
   - Im Gegensatz zu Photoshop und Illustrator kann der Image-Server den Text im Textfeld vertikal ausrichten. Die Standardeinstellung ist die obere Ausrichtung. Um dies zu ändern, wählen Sie Ihren Text aus und wählen Sie **Middle** oder **Bottom** aus dem Menü **Vertikale Ausrichtung**.
   - Wenn Sie den Text zu groß für das Feld machen (oder Ihr Textfeld zu klein ist), wird er ganz oder teilweise abgeschnitten und ausgeblendet. Verkleinern Sie die Schriftgröße oder vergrößern Sie das Feld.

5. Klicken Sie auf **Anwenden** , um zu sehen, dass Ihre Änderungen im Arbeitsbereichfenster wirksam werden. Sie müssen auf **Anwenden** klicken. Andernfalls gehen Ihre Änderungen verloren.
6. Wenn Sie fertig sind, klicken Sie auf **Close**. Wenn Sie in den Bearbeitungsmodus zurückkehren möchten, doppelklicken Sie auf die Textebene, um den Texteditor erneut zu öffnen.

Der Texteditor zeigt genau die Größe der Schrift an, wenn die Schrift lokal auf Ihrem System installiert ist.

### Über das Hinzufügen von Parametern zu Textebenen

Wir gehen nun ähnlich vor wie bei Ebenenparametern vor, um Textparameter hinzuzufügen. Textebenen können auch Ebenenparameter für Größe, Position usw. verwenden; Sie können jedoch zusätzliche Parameter verwenden, mit denen Sie jeden Aspekt des RTF steuern können.

Im Gegensatz zu Ebenenparametern wählen Sie nur den Wert aus, den Sie ändern möchten, und fügen einen Parameter hinzu, anstatt einen Parameter zur gesamten Eigenschaft hinzuzufügen.

![image](assets/basic-templates/basic-templates-10.jpg)

Beispiel-RTF:

![image](assets/basic-templates/sample-rtf.png)

Bei der Untersuchung des RTF müssen Sie herausfinden, wo jede Einstellung geändert werden soll. In der obigen RTF-Datei kann es sinnvoll sein, einige davon zu verwenden, und Sie können sehen, woher die Formatierung stammt.

Sie können die Phrase Schokolade Mint Sandal sehen - das ist der Text selbst.

- Es gibt einen Verweis auf die Schriftart Poor Richard - hier werden Schriftarten ausgewählt.
- Ein RGB-Wert wird angezeigt: \red56\green53\blue4  - dies ist die Textfarbe.
- Obwohl die Schriftgröße 20 ist, wird die Zahl 20 nicht angezeigt. Sie sehen jedoch den Befehl \fs40 - aus irgendeinem seltsamen Grund misst RTF Schriftarten als Halbpunkte. So ist \fs40 die Schriftgröße!

Sie verfügen über genügend Informationen, um Ihre Parameter zu erstellen. Es gibt jedoch eine vollständige Referenz aller RTF-Befehle in der Image Serving-Dokumentation. Besuchen Sie die [Image Serving Documentation](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html#concept-0d3136db7f6f49668274541cd4b6364c).

#### Hinzufügen von Parametern zu Textebenen

Im Folgenden finden Sie die Schritte zum Hinzufügen von Parametern zu Textebenen.

1. Klicken Sie auf die Schaltfläche **Parameter** (ein &quot;P&quot;) neben dem Namen der Textebene, für die Sie Parameter erstellen möchten. Der Bildschirm Parameter wird geöffnet. Auf der Registerkarte **Common** werden alle Eigenschaften auf der Ebene und deren Wert aufgelistet. Hier können Sie reguläre Ebenenparameter hinzufügen.
1. Klicken Sie auf die Registerkarte **Text** . Hier sehen Sie die RTF oben. die von Ihnen hinzugefügten Parameter darunter liegen.
1. Um einen Parameter hinzuzufügen, markieren Sie zunächst den zu ändernden Wert und klicken Sie auf die Schaltfläche **Parameter hinzufügen** . Stellen Sie sicher, dass Sie nur die Werte für Befehle und nicht den gesamten Befehl selbst auswählen. Um beispielsweise einen Parameter für den Schriftnamen im obigen Beispiel-RTF festzulegen, würde ich nur &quot;Armer Richard&quot;hervorheben und einen Parameter hinzufügen, aber nicht auch &quot;\f0&quot;. Wenn Sie auf **Parameter hinzufügen** klicken, wird er in der folgenden Liste angezeigt und der Parameterwert wird in der RTF-Datei rot angezeigt, solange er noch ausgewählt ist. Wenn Sie einen Parameter entfernen müssen, klicken Sie auf das Kontrollkästchen neben **Ein**, um diesen Parameter zu deaktivieren. Er wird dann ausgeblendet.
1. Klicken Sie auf , um Ihren Parameter in einen aussagekräftigeren Namen umzubenennen.
1. Wenn Sie fertig sind, wird Ihr RTF grün hervorgehoben, wo Parameter vorhanden sind, und Ihre Parameternamen und Werte werden unten aufgeführt.
1. Klicken Sie auf **Close** , um den Bildschirm &quot;Parameter&quot;zu verlassen. Drücken Sie dann **Save** , um die Vorlage zu speichern. Wenn Sie die Bearbeitung abgeschlossen haben, drücken Sie **Schließen**, um die Seite &quot;Vorlagengrundlagen&quot;zu verlassen.
1. Klicken Sie auf **Vorschau** , um Ihre Vorlage in Dynamic Media Classic zu testen. Um Ihre Textparameter zu testen, geben Sie im Vorschaufenster neuen Text oder neue Werte ein. Um die Schriftart zu ändern, müssen Sie den genauen RTF-Namen der Schriftart eingeben.

>[!TIP]
>
>Um Parameter zur Textfarbe hinzuzufügen, fügen Sie Parameter für Rot, Grün und Blau separat hinzu. Wenn das RTF-Format beispielsweise `\red56\green53\blue46` ist, fügen Sie separate rote, grüne und blaue Parameter für die Werte 56, 53 und 46 hinzu. In der URL ändern Sie die Farbe, indem Sie alle drei aufrufen: `&$red=56&$green=53&$blue=46`.

Erfahren Sie, wie Sie [Dynamische Textparameter erstellen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/creating-template-parameters.html#creating-dynamic-text-parameters).

## Veröffentlichen und Erstellen von Vorlagen-URLs

### Erstellen einer Bildvorgabe

Das Erstellen einer Vorgabe für Ihre Vorlage ist kein erforderlicher Schritt. Wir empfehlen dies als Best Practice, sodass die Vorlage immer in einer Größe von 1:1 aufgerufen wird und auch Scharfzeichnung zu allen großen Bildebenen hinzugefügt werden soll, deren Größe an die Vorlage angepasst wird. Wenn Sie ein Bild ohne Vorgabe aufrufen, kann der Image-Server die Größe des Bildes beliebig auf die Standardgröße (etwa 400 Pixel) ändern und keine standardmäßige Scharfzeichnung anwenden.

Eine Bildvorgabe für eine Vorlage hat nichts Besonderes. Wenn Sie bereits über eine Vorgabe für ein statisches Bild mit derselben Größe verfügen, können Sie sie stattdessen verwenden.

### Veröffentlichen

Sie müssen eine Veröffentlichung ausführen, damit Ihre Änderungen live auf den Image-Server übertragen werden. Beachten Sie, was veröffentlicht werden muss: die verschiedenen Bild-Asset-Ebenen, die Schriftarten für dynamischen Text und die Vorlage selbst. Ähnlich wie bei anderen Rich-Media-Assets in Dynamic Media Classic wie Bildsets und Rotationssets ist eine einfache Vorlage eine künstliche Konstruktion. Sie ist ein Zeilenelement in der Datenbank, das mithilfe einer Reihe von Image Serving-Befehlen auf die Bilder und Schriftarten verweist. Wenn Sie also die Vorlage veröffentlichen, aktualisieren Sie lediglich die Daten auf dem Image-Server.

Erfahren Sie mehr über [Veröffentlichen Ihrer Vorlage](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/template-basics/publishing-templates.html).

### Vorlagen-URL-Erstellung

Eine einfache Vorlage hat dieselbe wesentliche URL-Syntax wie ein normaler Bildaufruf, wie zuvor erläutert. Eine Vorlage verfügt in der Regel über mehr Modifikatoren (Befehle durch ein kaufmännisches Und-Zeichen (&amp;) getrennt), z. B. Parameter mit Werten. Der Hauptunterschied besteht jedoch darin, dass Sie als Hauptbild die Vorlage aufrufen, anstatt ein statisches Bild aufzurufen.

![image](assets/basic-templates/basic-templates-11.jpg)

Im Gegensatz zu Bildvorgaben, die auf jeder Seite des Vorgabennamens ein Dollarzeichen ($) aufweisen, haben Parameter am Anfang ein einzelnes Dollarzeichen. Die Platzierung dieser Dollarzeichen ist wichtig.

**Richtig:**

`$text=46-inch LCD HDTV`

**Falsch:**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Wie bereits erwähnt, werden Parameter zum Ändern der Vorlage verwendet. Wenn Sie die Vorlage ohne Parameter aufrufen, wird sie wieder zu den Standardeinstellungen zurückgesetzt, die im Authoring-Tool &quot;Vorlagen-Grundlagen&quot;festgelegt wurden. Wenn eine Eigenschaft nicht geändert werden muss, müssen Sie keinen Parameter festlegen.

![](assets/basic-templates/sandals-without-with-parameters.png)
_imageBeispiele für eine Vorlage ohne Festlegen von Parametern (oben) und mit Parametern (unten)._
