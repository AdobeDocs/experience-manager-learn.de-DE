---
title: Einführung in grundlegende Vorlagen
description: Erfahren Sie mehr über grundlegende Vorlagen in Dynamic Media Classic – bildbasierte Vorlagen, die vom Bild-Server aufgerufen werden und aus Bildern und gerendertem Text bestehen. Eine Vorlage kann dynamisch über die URL geändert werden, nachdem die Vorlage veröffentlicht wurde. Erfahren Sie, wie Sie eine Photoshop-PSD in Dynamic Media Classic hochladen, um sie als Grundlage für eine Vorlage zu verwenden. Erstellen Sie eine grundlegende Merchandising-Vorlage, die aus Bildebenen besteht. Fügen Sie Textebenen hinzu und gestalten Sie sie mithilfe von Parametern variabel. Erstellen Sie eine Vorlagen-URL und bearbeiten Sie das Bild dynamisch über den Webbrowser.
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: d4e16b45-0095-44b4-8c16-89adc15e0cf9
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: ht
source-wordcount: '6260'
ht-degree: 100%

---

# Einführung in grundlegende Vorlagen {#basic-templates}

In Dynamic Media Classic ist eine Vorlage ein Dokument, das nach der Veröffentlichung der Vorlage dynamisch über die URL geändert werden kann. Dynamic Media Classic bietet grundlegende Vorlagen – bildbasierte Vorlagen, die vom Bild-Server aufgerufen werden und aus Bildern und gerendertem Text bestehen.

Einer der leistungsstärksten Aspekte von Vorlagen besteht darin, dass sie direkte Integrationspunkte haben, über die Sie sie mit Ihrer Datenbank verknüpfen können. So können Sie nicht nur ein Bild bereitstellen und seine Größe ändern, Sie können auch Ihre Datenbank abfragen, um neue oder preisreduzierte Artikel zu finden und diese als Überlagerung auf dem Bild anzeigen zu lassen. Sie können eine Beschreibung des Artikels anfordern und diese als Beschriftung in der gewünschten Schriftart und mit dem gewünschten Layout verwenden. Die Möglichkeiten sind grenzenlos.

Grundlegende Vorlagen können auf viele verschiedene Arten implementiert werden, von einfach bis komplex. Zum Beispiel:

- Einfaches Merchandising. Es werden Beschriftungen wie „kostenloser Versand“ verwendet, wenn das Produkt kostenlos versendet wird. Diese Beschriftungen werden vom Merchandise-Team in Photoshop eingerichtet, und das Web ermittelt anhand von Logik, wann sie auf das Bild anzuwenden sind.
- Erweitertes Merchandising. Jede Vorlage verfügt über mehrere Variablen und kann verschiedene Optionen gleichzeitig anzeigen. Anhand einer Datenbank, des Warenbestands und von Geschäftsregeln wird bestimmt, wann für ein Produkt die Beschriftungen „Neu“, „Ausverkauf“ oder „Ausverkauft“ angezeigt werden sollen. Dabei kann auch ein Transparenzeffekt hinter dem Produkt verwendet werden, um es mit verschiedenen Hintergründen anzuzeigen, beispielsweise in verschiedenen Räumen. Dieselben Vorlagen und/oder Assets können auf der Seite mit den Produktdetails erneut verwendet werden, um eine größere oder vergrößerbare Version desselben Produkts mit unterschiedlichen Hintergründen anzuzeigen.

Es ist wichtig zu verstehen, dass Dynamic Media Classic nur den visuellen Teil dieser vorlagenbasierten Anwendungen bereitstellt. Dynamic Media Classic-Unternehmen oder ihre Integrationspartner müssen die Geschäftsregeln, die Datenbank und die Entwicklungsfähigkeiten bereitstellen, um die Anwendungen zu erstellen. Es gibt keine „integrierte“ Vorlagenanwendung. Designerinnen und Designer richten die Vorlage in Dynamic Media Classic ein, und das Entwickler-Team ändert die Variablen in der Vorlage mithilfe von URL-Aufrufen.

Am Ende dieses Tutorial-Abschnitts wissen Sie, wie Sie folgende Vorgänge durchführen:

- Hochladen einer Photoshop-PSD-Datei in Dynamic Media Classic, um sie als Grundlage für eine Vorlage zu verwenden
- Erstellen einer grundlegenden Merchandising-Vorlage, die aus Bildebenen besteht
- Hinzufügen von Textebenen und deren Gestaltung mithilfe von Parametern
- Erstellen einer Vorlagen-URL und dynamisches Bearbeiten des Bildes über den Webbrowser

>[!NOTE]
>
>Alle URLs in diesem Kapitel dienen nur zur Veranschaulichung. Es handelt sich nicht um Livelinks.

## Überblick über grundlegende Vorlagen

Die Definition einer grundlegenden Vorlage (oder kurz „Vorlage“) ist ein URL-adressierbares Bild mit Ebenen. Das Endergebnis ist ein Bild, das jedoch durch die URL geändert werden kann. Es kann aus Fotos, Text oder Grafiken bestehen – einer beliebigen Kombination von P-TIFF-Assets in Dynamic Media Classic.

Vorlagen ähneln am ehesten Photoshop-PSD-Dateien, da sie einen ähnlichen Workflow und ähnliche Funktionen aufweisen.

- Beide bestehen aus Ebenen (oder Schichten). Sie können teilweise transparente Bilder zusammensetzen und durch die transparenten Bereiche einer Ebene die darunter liegenden Ebenen sehen.
- Die Ebenen können verschoben und gedreht werden, um den Inhalt neu zu positionieren. Die Deckkraft und die Mischmodi können geändert werden, um den Inhalt teilweise transparent zu gestalten.
- Sie können textbasierte Ebenen erstellen. Die Qualität kann sehr hoch sein, da der Bild-Server dieselbe Text-Engine wie Photoshop und Illustrator verwendet.
- Einfache Ebenenstile können auf jede Ebene angewendet werden, um spezielle Effekte wie „Schlagschatten“ oder „Schein“ zu erzeugen.

Im Gegensatz zu Photoshop-PSD-Dateien können Ebenen jedoch vollständig dynamisch sein und über eine URL auf dem Bild-Server gesteuert werden.

- Sie können Variablen zu allen Vorlageneigenschaften hinzufügen, um die Komposition schnell und spontan zu ändern.
- Mit Variablen, die als Parameter bezeichnet werden, haben Sie die Möglichkeit, nur den Teil der Vorlage verfügbar zu machen, den Sie ändern möchten.

Sie müssen nur für jede Ebene, die variiert, einen Platzhalter hinzufügen, anstatt wie in Photoshop alle Ebenen in eine Datei aufzunehmen und sie ein- und auszublenden (aber auch das ist möglich, sofern Sie dies bevorzugen).

Mithilfe eines Platzhalters können Sie den Inhalt einer Ebene dynamisch durch ein anderes veröffentlichtes Asset austauschen. Dabei werden automatisch dieselben Eigenschaften (z. B. Größe und Drehung) der Ebene verwendet, die ersetzt wurde.

Da grundlegende Vorlagen in der Regel in Photoshop entworfen, aber über eine URL bereitgestellt werden, erfordert ein Vorlagenprojekt eine Mischung aus Design- und technischen Kenntnissen. Im Allgemeinen gehen wir davon aus, dass es sich bei der Person, die die Vorlagen kreativ bearbeitet, um eine Photoshop-Designerin oder einen -Designer handelt und bei der Person, die die Vorlage implementiert, um eine Web-Entwicklerin oder einen -Entwickler. Die Kreativ- und Entwicklungs-Teams müssen eng zusammenarbeiten, damit die Vorlage erfolgreich verwendet werden kann.

Vorlagenprojekte können je nach den Geschäftsregeln und Anforderungen der Anwendung relativ einfach oder äußerst komplex sein. Grundlegende Vorlagen werden vom Bild-Server aufgerufen. Aufgrund der Flexibilität der Dynamic Media Classic-Umgebung können Sie jedoch auch Vorlagen in anderen Vorlagen verschachteln, sodass Sie relativ komplexe Bilder erstellen können, die sich mit allgemein bezeichneten Variablen verknüpften lassen.

- Weitere Informationen über [Vorlagengrundlagen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/quick-start-template-basics.html?lang=de)
- Weitere Informationen zum Erstellen einer [grundlegenden Vorlage](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html?lang=de#creating_a_template)

## Erstellen grundlegender Vorlagen

Beim Arbeiten mit einer grundlegenden Vorlage halten Sie sich normalerweise an die Workflow-Schritte in der nachfolgenden Abbildung. Schritte, die mit gepunkteten Linien markiert sind, sind optional, wenn Sie dynamische Textebenen verwenden. Sie sind in den Anweisungen unten mit dem Hinweis „Text-Workflow“ gekennzeichnet. Wenn Sie keinen Text verwenden, folgen Sie nur dem Hauptpfad.

![Bild](assets/basic-templates/basic-templates-1.png)

_Workflow zum Erstellen einer grundlegenden Vorlage_

1. Entwerfen und erstellen Sie Ihre Assets. Die meisten Benutzenden nutzen hierfür Adobe Photoshop. Entwerfen Sie Assets mit der gewünschten Größe – wenn es sich um ein 200-Pixel-Bild für eine Miniaturseite handelt, erstellen Sie es mit 200 Pixeln. Wenn Sie darauf zoomen müssen, erstellen Sie es mit einer Größe von etwa 2000 Pixeln. Verwenden Sie Photoshop (und/oder Illustrator bei als Bitmap gespeicherten Dateien), um die Assets zu erstellen, und verwenden Sie Dynamic Media Classic, um die Teile zusammenzuführen, die Ebenen zu verwalten und Variablen hinzuzufügen.
2. Laden Sie die grafischen Assets nach dem Erstellen in Dynamic Media Classic hoch. Anstatt einzelne Assets aus der PSD-Datei hochzuladen, sollten Sie die gesamte PSD-Datei mit Ebenen hochladen und Dynamic Media Classic mithilfe der Option **Ebenen beibehalten** veranlassen, beim Hochladen eine Datei pro Ebene zu erstellen (weitere Informationen unten). _Text-Workflow: Wenn Sie dynamischen Text erstellen, laden Sie auch die Schriftarten hoch. Dynamischer Text ist variabel und wird über die URL gesteuert. Wenn Ihr Text statisch ist oder nur wenige kurze Ausdrücke enthält, die sich nicht ändern, z. B. die Tags „Neu“ oder „Sale“ anstelle von „X % Rabatt“ (wobei das X für eine variable Zahl steht), empfehlen wir, den Text vorab in Photoshop zu rendern und die gerasterten Ebenen als Bilder hochzuladen. Dies ist nicht nur einfacher, sondern Sie können den Text auch genau nach Ihren Wünschen gestalten._
3. Erstellen Sie die Vorlage in Dynamic Media Classic mit dem Vorlagengrundlagen-Editor des Menüs „Erstellen“ und fügen Sie Bildebenen hinzu. Text-Workflow: Erstellen von Textebenen in demselben Editor. Dieser Schritt ist beim manuellen Erstellen einer Vorlage in Dynamic Media Classic erforderlich. Wählen Sie eine dem Design entsprechende Arbeitsflächengröße aus, ziehen Sie Bilder auf die Arbeitsfläche und legen Sie die Ebeneneigenschaften fest (Größe, Drehung, Deckkraft usw.). Sie platzieren dabei nicht alle möglichen Ebenen in Ihre Vorlage, sondern nur einen Platzhalter pro Bildebene. _Text-Workflow: Erstellen Sie Textebenen mit dem Text-Tool, ähnlich wie die Erstellung von Textebenen in Photoshop. Sie können eine Schriftart auswählen und sie mit den gleichen Optionen formatieren, die beim Photoshop-Textwerkzeug verfügbar sind._ Ein weiterer Workflow besteht darin, eine PSD-Datei hochzuladen und über Dynamic Media Classic eine „freie“ Vorlage zu generieren. Dabei ist es sogar möglich, Textebenen neu zu erstellen. Hierauf wird später ausführlicher eingegangen.
4. Nachdem die Ebenen erstellt wurden, fügen Sie Parameter (Variablen) zu einer Eigenschaft einer beliebigen Ebene hinzu, die Sie über die URL steuern möchten, einschließlich der Ebenenquelle (das Bild selbst). _Text-Workflow: Sie können Parameter auch zu Textebenen hinzufügen, um den Textinhalt, die Größe und Position der Ebene sowie alle Formatierungsoptionen wie Schriftfarbe, Schriftgrad, horizontales Tracking usw. zu steuern._
5. Erstellen Sie eine Bildvorgabe, die der Größe Ihrer Vorlage entspricht. Dies wird empfohlen, damit die Vorlage immer mit einer Größe von 1:1 aufgerufen wird. Außerdem sollte allen großen Bildebenen, deren Größe an die Vorlage angepasst wird, ein Scharfzeichnungseffekt hinzugefügt werden. Wenn Sie eine Vorlage erstellen, die gezoomt werden soll, ist dieser Schritt nicht erforderlich.
6. Veröffentlichen Sie, kopieren Sie die URL aus der Dynamic Media Classic-Vorschau und testen Sie sie in einem Browser.

## Vorbereiten und Hochladen von Vorlagen-Assets in Dynamic Media Classic

Bevor Sie Ihre Vorlagen-Assets in Dynamic Media Classic hochladen, müssen Sie einige vorbereitende Schritte durchführen.

### Vorbereiten der PSD-Datei zum Hochladen

Bevor Sie Ihre Photoshop-Datei in Dynamic Media Classic hochladen, vereinfachen Sie die Ebenen in Photoshop, um einfacher arbeiten zu können und die größtmögliche Kompatibilität mit dem Bild-Server zu erhalten. Die PSD-Datei besteht oft aus vielen Elementen, die Dynamic Media Classic nicht erkennt. Möglicherweise müssen Sie sich auch mit vielen kleinen Elementen auseinandersetzen, die schwer zu verwalten sind. Achten Sie darauf, ein Backup Ihrer PSD-Primärdatei zu speichern, falls Sie das Original später bearbeiten müssen. Sie laden nämlich die vereinfachte Kopie hoch, nicht die Primärdatei.

![Bild](assets/basic-templates/basic-templates-2.jpg)

1. Vereinfachen Sie die Ebenenstruktur, indem Sie verwandte Ebenen, die zusammen aktiviert/deaktiviert werden müssen, zu einer einzigen Ebene zusammenführen/reduzieren. Beispielsweise werden die Beschriftung „NEU“ und das blaue Banner zu einer Ebene zusammengeführt, sodass Sie sie mit einem Klick ein- oder ausblenden können.
   ![Bild](assets/basic-templates/basic-templates-3.jpg)
2. Einige Ebenentypen und -effekte werden von Dynamic Media Classic oder dem Bild-Server nicht unterstützt und müssen vor dem Hochladen gerastert werden. Andernfalls werden möglicherweise die Effekte ignoriert oder die Ebenen verworfen. Eine Ebene zu rastern bedeutet, eine bearbeitbare Ebene in eine nicht bearbeitbare Ebene umzuwandeln. Um Ebeneneffekte oder Textebenen zu rastern, erstellen Sie eine leere Ebene, wählen Sie beide aus und fügen Sie diese über **Ebenen > Ebenen zusammenführen** oder Strg+E (bzw. Befehlstaste+E) zusammen.

   - Dynamic Media Classic kann Ebenen weder gruppieren noch verknüpfen. Alle Ebenen einer Gruppe oder eines verknüpften Satzes werden in separate Ebenen umgewandelt, die nicht mehr gruppiert/verknüpft sind.
   - Ebenenmasken werden beim Hochladen transparent umgewandelt.
   - Einstellungsebenen werden nicht unterstützt und deswegen verworfen.
   - Füllebenen, wie z. B. Farbflächenebenen, werden gerastert.
   - Smart-Objekt-Ebenen und Vektorebenen werden beim Hochladen zu normalen Bildern gerastert, und Smart-Filter werden angewendet und gerastert.
   - Textebenen werden ebenfalls gerastert, es sei denn, Sie verwenden die Option „Text extrahieren“. Weitere Informationen dazu finden Sie unten.
   - Die meisten Ebeneneffekte werden ignoriert, und nur wenige Mischmodi werden unterstützt. Fügen Sie im Zweifelsfall einfache Effekte in Dynamic Media Classic hinzu (z. B. „Schatten nach außen“ oder „Schlagschatten“, „Schein nach innen“ oder „Schein nach außen“) oder verwenden Sie eine leere Ebene, um den Effekt in Photoshop zusammenzuführen und zu rastern.

### Arbeiten mit Schriftarten

Wenn Sie dynamischen Text generieren möchten, müssen Sie Ihre Schriftarten hochladen und veröffentlichen. Die einzige in Dynamic Media Classic enthaltene Schriftart ist Arial.

Es liegt in der Verantwortung jedes Unternehmens, sich eine Lizenz zur Nutzung einer Schriftart im Internet zu beschaffen. Das bloße Installieren einer Schriftart auf Ihrem Computer gibt Ihnen nicht das Recht, diese kommerziell im Internet zu verwenden. Die Herausgeber der Schriftart könnten bei einer Nutzung ohne Genehmigung rechtliche Schritte gegen Ihr Unternehmen einleiten. Außerdem variieren die Lizenzbedingungen. Möglicherweise benötigen Sie separate Lizenzen für die Druck- und Bildschirmanzeige.

Dynamic Media Classic unterstützt standardmäßige OpenType(OTF)-, TrueType(TTF)- und Type 1-PostScript-Schriftarten. Mac-exklusive Suitcase-Schriftarten, Schriftart-Sammlungsdateien, Windows-Systemschriftarten und proprietäre Maschinenschriftarten (z. B. von Gravier- oder Stickmaschinen verwendete Schriftarten) werden nicht unterstützt. Sie müssen sie in eines der Standardschriftformate konvertieren oder durch eine ähnliche Schriftart für Dynamic Media Classic und den Bild-Server ersetzen.

Nachdem die Schriftarten wie jedes andere Asset in Dynamic Media Classic hochgeladen wurden, müssen sie auch für den Bild-Server veröffentlicht werden. Ein sehr häufiger Fehler im Zusammenhang mit Vorlagen besteht darin, die Veröffentlichung der Schriftarten zu vergessen, was zu einem Bildfehler führt. Der Bild-Server ersetzt sie nämlich durch keine andere Schriftart. Wenn Sie die Option **Text extrahieren** beim Hochladen verwenden möchten, müssen Sie zudem Ihre Schriftartendateien hochladen, bevor Sie die PSD-Datei hochladen, in der diese Schriftarten verwendet werden. Die Funktion **Text extrahieren** versucht, Ihren Text als bearbeitbare Textebene neu zu erstellen und ihn in einer Dynamic Media Classic-Vorlage zu platzieren. Dies wird im Rahmen des nächsten Themas, PSD-Optionen, erläutert.

Beachten Sie, dass Schriftarten verschiedene interne Namen haben können, die sich häufig von ihrem externen Dateinamen unterscheiden. Sie können alle zugehörigen Namen auf der Detailseite des jeweiligen Assets in Dynamic Media Classic sehen. Im Folgenden finden Sie die Namen der Schriftart „Adobe Caslon Pro Semifold“, die in Dynamic Media Classic auf der Registerkarte „Metadaten“ aufgeführt sind:

![Bild](assets/basic-templates/basic-templates-4.jpg)

_Registerkarte „Metadaten“ auf der Detailseite für eine Schriftart in Dynamic Media Classic_

Dynamic Media Classic verwendet den Dateinamen dieser Schriftart (ACaslonPro-Semibold) als Asset-ID. Dies ist jedoch nicht der von der Vorlage verwendete Name. Die Vorlage verwendet den unten aufgeführten Rich-Text-Format(RTF)-Namen. RTF ist die native „Sprache“ der Text-Engine des Bild-Servers.

Wenn Sie Schriftarten über die URL ändern müssen, müssen Sie den RTF-Namen der Schriftart (nicht die Asset-ID) aufrufen. Andernfalls wird ein Fehler ausgegeben. In diesem Fall wäre der richtige Name für diese Schriftart „Adobe Caslon Pro“. Weitere Erläuterungen zu Schriftarten und zum RTF-Format finden Sie weiter unten im Abschnitt „RTF und Textparameter“.

Die gängigsten Schriftarten-Dateiformate auf Windows- und Mac-Systemen sind OpenType und TrueType. OpenType hat die Erweiterung „OTF“, TrueType die Erweiterung „TTF“. Beide Formate funktionieren in Dynamic Media Classic gleichermaßen gut.

### Auswählen von Optionen beim Hochladen der PSD-Datei

Sie müssen keine Photoshop-Datei (PSD) hochladen, um eine Vorlage zu erstellen. Eine Vorlage kann aus allen Bild-Assets in Dynamic Media Classic erstellt werden. Das Hochladen einer PSD-Datei kann das Authoring jedoch vereinfachen, da diese Assets normalerweise bereits in einer PSD-Datei mit Ebenen vorhanden sind. Darüber hinaus generiert Dynamic Media Classic automatisch eine Vorlage, wenn Sie eine PSD-Datei mit Ebenen hochladen.

- **Ebenen beibehalten.** Dies ist die wichtigste Option. Dadurch wird Dynamic Media Classic angewiesen, pro Photoshop-Ebene ein Bild-Asset zu erstellen. Wenn diese Option deaktiviert ist, sind alle anderen Optionen deaktiviert und die PSD-Datei wird zu einem einfachen Bild reduziert.
- **Vorlage** **erstellen.** Bei Auswahl dieser Option wird durch erneutes Verbinden der verschiedenen erzeugten Ebenen automatisch eine Vorlage erstellt. Ein Nachteil beim Verwenden der automatisch generierten Vorlage besteht darin, dass Dynamic Media Classic alle Ebenen in einer Datei platziert, obwohl wir nur einen einzigen Platzhalter pro Ebene benötigen. Die zusätzlichen Ebenen lassen sich einfach löschen. Aber wenn viele Ebenen vorhanden sind, ist eine Neuerstellung schneller. Achten Sie darauf, die neue Vorlage umzubenennen. Andernfalls wird sie überschrieben, wenn Sie dieselbe PSD-Datei das nächste Mal erneut hochladen.
- **Text extrahieren.** Durch diese Option werden Textebenen in der PSD-Datei als Textebenen in der Vorlage unter Verwendung der von Ihnen hochgeladenen Schriftart neu erstellt. Dieser Schritt ist erforderlich, wenn sich Ihr Text in Photoshop unter einem Pfad befindet und dieser Pfad in der Vorlage beibehalten werden soll. Für diese Funktion müssen Sie die Option **Vorlage erstellen** verwenden, da der extrahierte Text nur durch eine beim Hochladen generierte Vorlage erstellt werden kann.
- **Ebenen auf Hintergrundgröße ausdehnen.** Durch diese Einstellung nimmt die Ebene die gleiche Größe wie die globale PSD-Arbeitsfläche an. Dies ist sehr nützlich bei Ebenen, deren Position sich nicht ändert: Wenn Sie Bilder in dieselbe Ebene tauschen, müssen Sie sie andernfalls eventuell neu positionieren.
- **Ebenenbenennung.** Durch diese Option erfährt Dynamic Media Classic, wie die einzelnen Assets, die pro Ebene generiert wurden, benannt werden sollen. Wir empfehlen hierzu **Photoshop** und **Ebenenname****** oder Photoshop und **Ebenennummer******. Beide Optionen verwenden den PSD-Namen als ersten Teil des Namens und fügen entweder den Ebenennamen oder die Ebenennummer am Ende hinzu. Ein Beispiel: Eine PSD-Datei mit dem Namen „hemd.psd“ und den Ebenen „vorderseite“, „ärmel“ und „kragen“ wird mit der Option **Photoshop und** **Ebenennamen** hochgeladen. Dynamic Media Classic generiert daraufhin die Asset-IDs „hemd_vorderseite&quot;, „hemd_ärmel“ und „hemd_kragen“. Wird eine dieser Optionen verwendet, ist ein eindeutiger Name in Dynamic Media Classic sichergestellt.

## Erstellen einer Vorlage mit Bildebenen

Auch wenn Dynamic Media Classic automatisch eine Vorlage aus einer PSD-Datei mit mehreren Ebenen erstellen kann, sollten Sie wissen, wie Vorlagen manuell erstellt werden. Wie oben erläutert, kann es vorkommen, dass Sie die von Dynamic Media Classic erstellte Vorlage nicht verwenden möchten.

### Die Vorlagengrundlagen-Benutzeroberfläche

Sehen wir uns zunächst die Bearbeitungsoberfläche an.

Mittig links befindet sich der Arbeitsbereich mit einer Vorschau Ihrer endgültigen Vorlage. Auf der rechten Seite sind die Bedienfelder „Ebenen“ und „Ebeneneigenschaften“. In diesen Bereichen arbeiten Sie am meisten.

![Bild](assets/basic-templates/basic-templates-5.jpg)

_Seite „Vorlagengrundlagen erstellen“_

- **Vorschau/Arbeitsbereich.** Dies ist das Hauptfenster. Hier können Sie Ebenen mit der Maus verschieben, verkleinern/vergrößern und drehen. Ebenenkonturen werden als gestrichelte Linien angezeigt.
- **Ebenen.** Dieser Bereich ähnelt dem Photoshop-Bedienfeld für Ebenen. Wenn Sie Ihrer Vorlage Ebenen hinzufügen, werden diese hier angezeigt. Ebenen werden von oben nach unten gestapelt – die oberste Ebene im Bedienfeld „Ebenen“ wird über den anderen, in der Liste nachfolgenden Ebenen angezeigt.
- **Ebeneneigenschaften.** Hier können Sie alle Eigenschaften einer Ebene mithilfe numerischer Steuerelemente anpassen. Wählen Sie zuerst eine Ebene aus und passen Sie dann deren Eigenschaften an.
- **Zusammengesetzte** **URL.** Unten in der Benutzeroberfläche befindet sich der Bereich „Zusammengesetzte URL“. Darauf wird in diesem Abschnitt des Tutorials nicht näher eingegangen. Allerdings wird hier Ihre Vorlage als eine Reihe von URL-Modifikatoren für die Bildbereitstellung dekonstruiert. Dieser Bereich kann bearbeitet werden. Wenn Sie sich mit den Bild-Server-Befehlen gut auskennen, können Sie die Vorlage hier manuell bearbeiten. Sie können sie aber auch beschädigen. Wie in Photoshop beginnt die Nummerierung der Ebenen bei 0. Die Arbeitsfläche entspricht der Ebene 0, und die erste Ebene, die Sie selbst hinzufügen, ist Ebene 1. Die Mischmodi bestimmen, wie die Pixel einer Ebene mit den darunter liegenden Pixeln überblendet werden. Mithilfe von Mischmodi können Sie eine Vielzahl von speziellen Effekten erstellen.

#### Verwenden des Vorlagengrundlagen-Editors

Im Folgenden finden Sie die Workflow-Schritte zum Erstellen einer grundlegenden Vorlage:

1. Navigieren Sie in Dynamic Media Classic zu **Erstellen > Vorlagengrundlagen**. Sie können entweder nichts auswählen oder sich für ein Bild entscheiden, das zur ersten Ebene Ihrer Vorlage wird.
2. Wählen Sie eine Größe und dann **OK** aus. Diese Größe sollte mit der in Photoshop festgelegten Größe übereinstimmen. Der Vorlageneditor wird geladen.
3. Wenn Sie in Schritt 1 kein Bild ausgewählt haben, suchen oder navigieren Sie im Asset-Bedienfeld auf der linken Seite zu einem Bild und ziehen Sie es in den Arbeitsbereich.

   - Die Größe des Bildes wird automatisch an die Größe der Arbeitsfläche angepasst. Wenn Sie Ihre hochauflösenden Bilder austauschen möchten, würden Sie normalerweise eines Ihrer (2000 px) großen P-TIFF-Bilder als Platzhalter verwenden.
   - Dies sollte die unterste Ebene Ihrer Vorlage sein. Sie können die Ebenen aber später neu anordnen.

4. Ändern Sie die Größe oder Position der Ebene direkt im Arbeitsbereich oder passen Sie die Einstellungen im Bedienfeld „Ebeneneigenschaften“ an.
5. Fügen Sie nach Bedarf weitere Bildebenen hinzu. Fügen Sie ggf. Ebeneneffekte hinzu. Weitere Informationen finden Sie unten im Abschnitt _Hinzufügen von Ebeneneffekten_.
6. Klicken Sie auf **Speichern**, wählen Sie einen Speicherort aus und geben Sie der Vorlage einen Namen. Sie können eine Vorschau aufrufen. Allerdings sieht Ihre Vorlage zu diesem Zeitpunkt genau wie ein reduziertes Photoshop-Bild aus – es kann noch nicht geändert werden.

### Hinzufügen von Ebeneneffekten

Der Bild-Server unterstützt einige programmgesteuerte Ebeneneffekte – spezielle Effekte, die das Erscheinungsbild von Ebeneninhalten ändern. Sie funktionieren ähnlich wie die Ebeneneffekte in Photoshop. Sie werden an eine Ebene angehängt, aber unabhängig von der Ebene gesteuert. Sie können sie anpassen oder entfernen, ohne eine dauerhafte Änderung an der Ebene selbst vorzunehmen.

- **Schlagschatten**. Wendet einen Schatten außerhalb der Ebenengrenzen an, der durch einen X- und Y-Pixel-Versatz positioniert wird.
- **Schatten nach innen**. Wendet einen Schatten innerhalb der Ebenengrenzen an, der durch einen X- und Y-Pixel-Versatz positioniert wird.
- **Schein nach außen**. Wendet einen Glüheffekt gleichmäßig um alle Kanten der Ebene an.
- **Schein nach innen**. Wendet einen Glüheffekt gleichmäßig innerhalb aller Kanten der Ebene an.

![Bild](assets/basic-templates/basic-templates-6.jpg)

_Eine Ebene mit und ohne Schlagschatten_

Um einen Effekt hinzuzufügen, klicken Sie auf **Effekt hinzufügen** und wählen Sie einen Effekt aus dem Menü. Wie bei normalen Ebenen können Sie einen Effekt im Bedienfeld „Ebenen“ auswählen und die zugehörigen Einstellungen im Bedienfeld „Ebeneneigenschaften“ anpassen.

Schatteneffekte werden horizontal oder vertikal von der Ebene weg versetzt, während Scheineffekte (d. h. Glüheffekte) gleichmäßig in alle Richtungen angewendet werden. „Nach innen“-Effekte wirken auf die nicht transparenten Teile der Ebene, während „Nach außen“-Effekte nur die transparenten Bereiche betreffen.

Weitere Informationen dazu finden Sie unter [Hinzufügen von Ebeneneffekten](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html?lang=de#using-shadow-and-glow-effects-on-layers).

### Hinzufügen von Parametern

Wenn Sie ausschließlich Ebenen kombinieren und speichern, unterscheidet sich das Endergebnis letztendlich nicht von einem reduzierten Photoshop-Bild. Vorlagen zeichnen sich jedoch dadurch aus, dass es möglich ist, den Eigenschaften jeder Ebene Parameter hinzuzufügen, damit sie dynamisch über die URL geändert werden können.

In Dynamic Media Classic ist ein Parameter eine Variable, die mit einer Vorlageneigenschaft verknüpft werden kann, damit sie über eine URL bearbeitbar ist. Wenn Sie einen Parameter zu einer Ebene hinzufügen, zeigt Dynamic Media Classic diese Eigenschaft in der URL an, indem dem Parameternamen ein Dollarzeichen ($) vorangestellt wird. Wenn Sie beispielsweise den Parameter „size“ erstellen, um die Größe einer Ebene zu ändern, benennt Dynamic Media Classic Ihren Parameter in $size um.

Wenn Sie keinen Parameter für eine Eigenschaft hinzufügen, bleibt diese Eigenschaft in der Dynamic Media Classic-Datenbank verborgen und wird nicht in der URL angezeigt.

![Bild](assets/basic-templates/parameters.png)

Ohne Parameter wären Ihre URLs in der Regel viel länger, insbesondere wenn Sie auch dynamischen Text verwenden. Durch Text werden Dutzende von zusätzlichen Zeichen zu jeder URL hinzugefügt.

Schließlich wird der anfängliche Parametersatz zum Standardwert für die Eigenschaften in der Vorlage. Wenn Sie eine Vorlage erstellen, Parameter hinzufügen und dann die URL ohne die Parameter aufrufen, erstellt der Bild-Server das Bild mit allen in der Vorlage gespeicherten Standardwerten. Parameter werden nur benötigt, wenn Sie eine Eigenschaft ändern möchten. Wenn eine Eigenschaft nicht geändert werden muss, brauchen Sie keinen Parameter festzulegen.

#### Erstellen von Parametern

Zur Erstellung von Parametern wird folgendermaßen vorgegangen:

1. Klicken Sie auf **Parameter** neben dem Namen der Ebene, für die Sie Parameter erstellen möchten. Der Bildschirm „Parameter“ wird geöffnet. Er listet jede Eigenschaft der Ebene und ihren Wert auf.
1. Wählen Sie die Option **Ein** neben dem Namen jeder Eigenschaft, die Sie zu einem Parameter machen möchten. Daraufhin wird ein standardmäßiger Parametername angezeigt. Sie können nur Parameter zu Eigenschaften hinzufügen, die sich vom Standardstatus geändert haben.

   - Wenn Sie z. B. eine Ebene hinzufügen und sie an der Standard-XY-Position von 0,0 belassen, wird Dynamic Media Classic keine **Positions**-Eigenschaft anzeigen. Um das Problem zu beheben, verschieben Sie die Ebene um mindestens ein Pixel. Jetzt stellt Dynamic Media Classic **Position** als eine Eigenschaft zur Verfügung, die Sie parametrisieren können.
   - Um der Eigenschaft „Ein-/Ausblenden“ (die die Ebene ein- und ausschaltet) einen Parameter hinzuzufügen, klicken Sie auf das Symbol **Ebene einblenden** bzw. **Ebene ausblenden**, um die Ebene auszuschalten (Sie können sie später wieder einschalten, wenn Sie möchten). Dynamic Media Classic stellt jetzt eine **Ausblenden**-Eigenschaft zur Verfügung, die parametrisiert werden kann.

1. Benennen Sie die standardmäßigen Parameternamen in einen Parameter um, der in der URL leichter zu identifizieren ist. Wenn Sie beispielsweise einen Parameter hinzufügen möchten, um die Bannerebene auf einem Bild zu ändern, ändern Sie den Standardnamen von „layer_2_src“ in „banner“.
1. Klicken Sie auf **Schließen**, um den Bildschirm „Parameter“ zu verlassen.
1. Wiederholen Sie diesen Vorgang für andere Ebenen, indem Sie auf die Schaltfläche **Parameter** klicken und Parameter hinzufügen und umbenennen.
1. Speichern Sie nach Abschluss Ihre Änderungen.

>[!TIP]
>
>Benennen Sie Ihre Parameter in eine aussagekräftige Bezeichnung um und entwickeln Sie eine Namenskonvention, um diese Namen zu standardisieren. Stellen Sie sicher, dass die Namenskonvention im Voraus sowohl mit dem Design- als auch mit dem Entwicklungs-Team abgesprochen wurde.
>
>Sie können keinen Parameter hinzufügen, weil Sie die Eigenschaft nicht sehen? Ändern Sie einfach die Eigenschaft der Ebene von der Standardeinstellung (durch Verschieben, Ändern der Größe, Ausblenden usw.). Die entsprechende Eigenschaft sollte jetzt angezeigt werden.

Weitere Informationen zu [Vorlagenparametern](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html?lang=de).

## Erstellen einer Vorlage mit Textebenen

Hier erfahren Sie, wie Sie eine einfache Vorlage erstellen, die Textebenen enthält.

### Grundlegendes zu dynamischem Text

Sie wissen jetzt, wie Sie eine einfache Vorlage mit Bildebenen erstellen. Für viele Anwendungen ist dies alles, was Sie benötigen. Wie Sie in der vorherigen Übung gesehen haben, können Ebenen mit einfachem Text (z. B. „Verkauf“ und „Neu“) gerastert und als Bilder behandelt werden, da ihr Text nicht geändert werden muss.

Dies sieht jedoch anders aus, wenn Sie etwa:

- Eine Bezeichnung mit der Aufschrift „25 % Rabatt“ hinzufügen, wobei der Wert von 25 % variabel ist
- Eine Textbeschriftung mit dem Namen des Produkts über dem Bild hinzufügen
- Ihre Ebenen in verschiedene Sprachen lokalisieren, je nachdem, in welchem Land Ihre Vorlage angezeigt wird

In solchen Fällen sollten Sie einige dynamische Textebenen mit Parametern hinzufügen, um den Text und/oder die Formatierung zu steuern.

Um Text zu erstellen, müssen Sie einige Schriftarten hochladen. Andernfalls wird Dynamic Media Classic standardmäßig Arial verwenden. Die Schriftarten müssen auch auf dem Bild-Server veröffentlicht werden. Andernfalls wird ein Fehler ausgegeben, wenn versucht wird, Text zu rendern, der diese Schriftart verwendet.

### RTF- und Text-Parameter

Um mit dem Vorlagengrundlagen-Tool Variablen zu Text hinzuzufügen, sollten Sie wissen, wie Text gerendert wird. Der Bild-Server generiert Text mit der Adobe Text Engine, die von Photoshop und Illustrator verwendet wird, und stellt ihn als Ebene im endgültigen Bild zusammen. Um mit der Engine zu kommunizieren, verwendet der Bild-Server das Rich Text Format (RTF).

RTF ist eine Dateiformat-Spezifikation, die von Microsoft für die Formatierung von Dokumenten entwickelt wurde. Es handelt sich dabei um eine standardmäßige Markup-Sprache, die von den meisten Textverarbeitungs- und E-Mail-Programmen verwendet wird. Wenn Sie in eine URL „&amp;text=\b1 Hallo“ schreiben, würde der Bild-Server ein Bild mit dem Wort „Hallo“ in fetter Schrift erzeugen, denn „\b1“ ist der RTF-Befehl, mit dem der Text fett dargestellt wird.

Die gute Nachricht ist, dass Dynamic Media Classic das RTF für Sie generiert. Wenn Sie Text in eine Vorlage eingeben und Formatierungen hinzufügen, schreibt Dynamic Media Classic den RTF-Code automatisch in die Vorlage. Der Grund, warum wir es trotzdem erwähnen, ist, dass Sie Parameter direkt zum RTF hinzufügen können. Daher ist es wichtig, dass Sie damit ein wenig vertraut sind.

#### Erstellen von Textebenen

Sie können Textebenen in einer Vorlage in Dynamic Media Classic auf zwei Arten erstellen:

1. Text-Tool in Dynamic Media Classic. Wir werden diese Methode weiter unten besprechen. Der Vorlagengrundlagen-Editor verfügt über ein Tool, mit dem Sie ein Textfeld erstellen, Text eingeben und den Text formatieren können. Dynamic Media Classic generiert das RTF nach Bedarf und platziert es in einer separaten Ebene.
2. Text extrahieren (beim Hochladen). Die andere Methode besteht darin, die Textebene in Photoshop zu erstellen und als normale Textebene in der PSD zu speichern (anstatt sie als Bildebene zu rastern). Sie laden die Datei dann in Dynamic Media Classic hoch und verwenden die Option **Text extrahieren**. Dynamic Media Classic konvertiert jede Photoshop-Textebene mithilfe von RTF-Befehlen in eine Bildbereitstellungs-Textebene. Wenn Sie diese Methode verwenden, stellen Sie sicher, dass Sie Ihre Schriftarten zuerst in Dynamic Media Classic hochladen. Andernfalls ersetzt Dynamic Media Classic beim Hochladen eine Standardschriftart und es gibt keine einfache Möglichkeit, sie erneut durch die richtige Schriftart zu ersetzen.

### Der Texteditor

Sie geben Text mit dem Texteditor ein. Der Texteditor ist eine WYSIWYG-Benutzeroberfläche, mit der Sie Text mit Formatierungssteuerelementen eingeben und formatieren können, die denen in Photoshop oder Illustrator ähneln.

![Bild](assets/basic-templates/basic-templates-9.jpg)

_Texteditor für Vorlagengrundlagen_

Den größten Teil Ihrer Arbeit werden Sie auf der Registerkarte **Vorschau** erledigen, auf der Sie Text eingeben und ihn so sehen können, wie er in der Vorlage aussehen wird. Es gibt auch eine Registerkarte **Quelle**, auf der die RTF-Datei bei Bedarf manuell bearbeitet werden kann.

Der allgemeine Workflow besteht darin, die Registerkarte **Vorschau** zu verwenden, um einen Text einzugeben.

Wählen Sie dann den Text aus und wählen Sie mithilfe der Steuerelemente oben eine Formatierung wie Schriftfarbe, Schriftgrad oder Ausrichtung aus. Nachdem der Text nach Ihren Wünschen gestaltet wurde, klicken Sie auf **Anwenden**, um ihn in der Vorschau des Arbeitsbereichs zu aktualisieren. Schließen Sie dann den Texteditor, um zum Hauptfenster „Vorlagengrundlagen“ zurückzukehren.

#### Verwenden des Texteditors

Hier sind die Workflow-Schritte für das Hinzufügen von Text innerhalb der Seite für die Erstellung von Vorlagengrundlagen:

1. Klicken Sie auf die Tool-Schaltfläche **Text** am oberen Rand der Erstellungsseite.
2. Ziehen Sie aus ihr ein Textfeld heraus, in dem Text angezeigt werden soll. Das Fenster „Texteditor“ wird als modales Fenster geöffnet. Im Hintergrund wird die Vorlage angezeigt, sie kann jedoch erst bearbeitet werden, nachdem Sie die Bearbeitung abgeschlossen haben.
3. Geben Sie den Beispieltext ein, der beim ersten Laden der Vorlage angezeigt werden soll. Wenn Sie beispielsweise ein Textfeld für ein personalisiertes E-Mail-Bild erstellen, kann Ihr Text etwa lauten „Hi Name. Jetzt ist es an der Zeit zu sparen!“ Später würden Sie einen Textparameter hinzufügen, um den Namen durch einen Wert zu ersetzen, den Sie für die URL senden. Ihr Text wird erst dann in der Vorlage unter dem Fenster angezeigt, wenn Sie auf **Anwenden** klicken.
4. Um den Text zu formatieren, wählen Sie ihn aus, indem Sie ihn mit der Maus ziehen und ein Formatierungssteuerelement in der Benutzeroberfläche auswählen.

   - Es gibt viele Formatierungsoptionen. Einige der häufigsten sind Schriftart (Schriftbild), Schriftgrad und Schriftfarbe sowie linke/mittlere/rechte Ausrichtung.
   - Vergessen Sie nicht, zuerst den Text auszuwählen. Andernfalls können Sie keine Formatierung anwenden.
   - Um eine andere Schriftart auszuwählen, wählen Sie den Text aus und öffnen Sie das Menü „Schriftarten“. Der Editor zeigt eine Liste aller Schriftarten an, die in Dynamic Media Classic hochgeladen wurden. Wenn eine Schrift auch auf Ihrem Computer installiert ist, wird sie schwarz angezeigt. Wenn sie jedoch nicht auf Ihrem Computer installiert ist, wird sie rot angezeigt. Sie wird jedoch weiterhin im Vorschaufenster gerendert, wenn Sie auf **Anwenden** klicken. Sie müssen nur Schriftarten in Dynamic Media Classic hochladen, um sie allen Benutzenden mit Dynamic Media Classic zur Verfügung zu stellen. Nach der Veröffentlichung verwendet der Bild-Server diese Schriftarten, um den Text zu generieren. Ihre Benutzenden müssen also keine Schriftarten installieren, um den von Ihnen erstellten Text zu sehen, da er Teil eines Bildes ist.
   - Im Gegensatz zu Photoshop und Illustrator kann der Bild-Server den Text im Textfeld auch vertikal ausrichten. Die Standardeinstellung ist die obere Ausrichtung. Um dies zu ändern, markieren Sie Ihren Text und wählen Sie **Mitte** oder **Unten** im Menü **Vertikale Ausrichtung**.
   - Wenn Sie den Text zu groß für das Feld machen (oder Ihr Textfeld zu klein ist), wird er ganz oder teilweise abgeschnitten und ausgeblendet. Verkleinern Sie dann den Schriftgrad oder vergrößern Sie das Feld.

5. Klicken Sie auf **Anwenden**, damit Ihre Änderungen im Arbeitsbereichsfenster wirksam werden. Sie müssen auf **Anwenden** klicken, sonst gehen Ihre Änderungen verloren.
6. Klicken Sie abschließend auf **Schließen**. Wenn Sie in den Bearbeitungsmodus zurückkehren möchten, doppelklicken Sie auf die Textebene, um den Texteditor erneut zu öffnen.

Der Texteditor zeigt genau die Größe der Schrift an, wenn die Schrift lokal auf Ihrem System installiert ist.

### Über das Hinzufügen von Parametern zu Textebenen

Beim Hinzufügen von Textparametern gehen wir nun ähnlich vor wie bei den Ebenenparametern. Textebenen können auch Ebenenparameter für Größe, Position usw. verwenden. Sie können jedoch zusätzliche Parameter verwenden, mit denen Sie jeden Aspekt des RTF steuern können.

Im Gegensatz zu Ebenenparametern wählen Sie nur den Wert aus, den Sie ändern möchten, und fügen einen Parameter hinzu, anstatt einen Parameter zur gesamten Eigenschaft hinzuzufügen.

![Bild](assets/basic-templates/basic-templates-10.jpg)

Beispiel-RTF:

![Bild](assets/basic-templates/sample-rtf.png)

Bei der Untersuchung des RTF müssen Sie herausfinden, wo sich die einzelnen Einstellungen befinden, die Sie ändern möchten. In der obigen RTF-Datei macht einiges davon vielleicht Sinn und Sie können sehen, woher die Formatierung kommt.

Sie können die Formulierung „Chocolate Mint Sandal“ sehen – das ist der Text selbst.

- Es gibt einen Verweis auf die Schriftart „Poor Richard“ – hier werden Schriftarten ausgewählt.
- Sie können einen RGB-Wert sehen: \red56\green53\blue4 – dies ist die Textfarbe.
- Obwohl der Schriftgrad 20 ist, wird die Zahl 20 nicht angezeigt. Sie sehen jedoch den Befehl: \fs40 – aus irgendeinem seltsamen Grund misst RTF Schriftarten als Halbpunkte. So ist also \fs40 der Schriftgrad!

Sie verfügen über genügend Informationen, um Ihre Parameter zu erstellen. Es gibt jedoch auch eine vollständige Referenz aller RTF-Befehle in der Bildbereitstellungs-Dokumentation. Besuchen Sie die [Bildbereitstellungs-Dokumentation](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html?lang=de#concept-0d3136db7f6f49668274541cd4b6364c).

#### Hinzufügen von Parametern zu Textebenen

Im Folgenden finden Sie die Schritte zum Hinzufügen von Parametern zu Textebenen.

1. Klicken Sie auf die Schaltfläche **Parameter** (ein „P“) neben dem Namen der Textebene, für die Sie Parameter erstellen möchten. Der Bildschirm „Parameter“ wird geöffnet. Auf der Registerkarte **Allgemein** wird jede Eigenschaft der Ebene und ihr Wert aufgelistet. Hier können Sie reguläre Ebenenparameter hinzufügen.
1. Klicken Sie auf die Registerkarte **Text**. Hier sehen Sie das RTF oben, und die Parameter, die Sie hinzufügen, darunter.
1. Um einen Parameter hinzuzufügen, markieren Sie zunächst den zu ändernden Wert und klicken Sie auf die Schaltfläche **Parameter hinzufügen**. Stellen Sie sicher, dass Sie nur die Werte für Befehle und nicht den gesamten Befehl selbst auswählen. Um beispielsweise einen Parameter für den Namen der Schriftart im obigen Beispiel-RTF festzulegen, würde ich nur „Poor Richard“ markieren und einen Parameter für diese Schriftart hinzufügen, nicht aber den Teil „\f0“. Wenn Sie auf **Parameter hinzufügen** klicken, erscheint er in der Liste darunter, und der Parameterwert erscheint im RTF in roter Farbe, solange er noch ausgewählt ist. Wenn Sie einen Parameter entfernen möchten, klicken Sie auf das Kontrollkästchen neben **Ein**, um diesen Parameter zu deaktivieren. Er verschwindet dann.
1. Klicken Sie auf einen Parameter, um ihm einen aussagekräftigeren Namen zu geben.
1. Wenn Sie fertig sind, wird Ihr RTF dort grün hervorgehoben, wo Parameter vorhanden sind, und Ihre Parameternamen und Werte sind unten aufgeführt.
1. Klicken Sie auf **Schließen**, um den Bildschirm „Parameter“ zu verlassen. Klicken Sie dann auf **Speichern**, um die Vorlage zu speichern Wenn Sie mit der Bearbeitung fertig sind, klicken Sie auf **Schließen**, um die Seite mit den Vorlagengrundlagen zu verlassen.
1. Klicken Sie auf **Vorschau**, um Ihre Vorlage in Dynamic Media Classic zu testen. Um Ihre Textparameter zu testen, geben Sie neuen Text oder neue Werte in das Vorschaufenster ein. Um die Schriftart zu ändern, müssen Sie den genauen RTF-Namen der Schriftart eingeben.

>[!TIP]
>
>Um Parameter zur Textfarbe hinzuzufügen, fügen Sie Parameter für Rot, Grün und Blau separat hinzu. Wenn das RTF z. B. `\red56\green53\blue46` lautet, würden Sie separate Rot-, Grün- und Blau-Parameter für die Werte 56, 53 und 46 hinzufügen. In der URL würden Sie die Farbe ändern, indem Sie alle drei aufrufen: `&$red=56&$green=53&$blue=46`.

Erfahren Sie, wie man [dynamische Textparameter erstellt](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html?lang=de#creating-dynamic-text-parameters).

## Veröffentlichen und Erstellen von Vorlagen-URLs

### Erstellen von Bildvorgaben

Das Erstellen einer Vorgabe für Ihre Vorlage ist kein erforderlicher Schritt. Wir empfehlen dies aber als Best Practice, sodass die Vorlage immer in einer Größe von 1:1 aufgerufen wird und auch, um Scharfzeichnung zu allen großen Bildebenen hinzuzufügen, deren Größe an die Vorlage angepasst wird. Wenn Sie ein Bild ohne Vorgabe aufrufen, kann der Bild-Server die Größe des Bildes beliebig auf die Standardgröße (etwa 400 Pixel) ändern, ohne standardmäßig Scharfzeichnung anzuwenden.

An einer Bildvorgabe für eine Vorlage ist nichts Besonderes. Wenn Sie bereits über eine Vorgabe für ein statisches Bild mit derselben Größe verfügen, können Sie diese stattdessen verwenden.

### Veröffentlichen 

Sie müssen eine Veröffentlichung durchführen, damit Ihre Änderungen live auf den Bild-Server übertragen werden. Denken Sie daran, was alles veröffentlicht werden muss: die verschiedenen Bild-Asset-Ebenen, die Schriftarten für dynamischen Text und die Vorlage selbst. Ähnlich wie bei anderen Rich Media Assets in Dynamic Media Classic, z. B. Bild- und Rotations-Sets, handelt es sich bei einer grundlegenden Vorlage um eine künstliche Konstruktion. Sie ist ein Zeileneintrag in der Datenbank, der mithilfe einer Reihe von Befehlen zur Bildbereitstellung auf die Bilder und Schriftarten verweist. Wenn Sie also die Vorlage veröffentlichen, aktualisieren Sie lediglich die Daten auf dem Bild-Server.

Weitere Informationen finden Sie unter [Veröffentlichen der Vorlage](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/publishing-templates.html?lang=de).

### Erstellen der Vorlagen-URL

Eine grundlegende Vorlage besitzt dieselbe wesentliche URL-Syntax wie ein normaler Bildaufruf, wie zuvor erläutert. Eine Vorlage verfügt in der Regel über mehr Modifikatoren (durch ein Ampersand (&amp;) getrennte Befehle), z. B. Parameter mit Werten. Der Hauptunterschied besteht jedoch darin, dass Sie als Hauptbild die Vorlage aufrufen und nicht ein statisches Bild.

![Bild](assets/basic-templates/basic-templates-11.jpg)

Im Gegensatz zu Bildvorgaben, die auf beiden Seiten des Vorgabenamens ein Dollarzeichen ($) aufweisen, steht bei Parametern nur ein einzelnes Dollarzeichen am Anfang. Die Platzierung dieser Dollarzeichen ist wichtig.

**Richtig:**

`$text=46-inch LCD HDTV`

**Falsch:**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Wie bereits erwähnt, werden Parameter zum Ändern der Vorlage verwendet. Wenn Sie die Vorlage ohne Parameter aufrufen, wird sie wieder auf die Standardeinstellungen zurückgesetzt, die im Inhaltserstellungs-Tool „Vorlagengrundlagen“ festgelegt wurden. Wenn eine Eigenschaft nicht geändert werden muss, brauchen Sie keinen Parameter festzulegen.

![Bild](assets/basic-templates/sandals-without-with-parameters.png)
_Beispiele für eine Vorlage ohne Parameter (oben) und mit Parametern (unten)_
