---
title: Machen Sie sich mit den verschiedenen Typen von PDF forms und Dokumenten vertraut.
description: PDF ist eigentlich eine Familie von Dateiformaten. In diesem Artikel werden die Typen von PDFs beschrieben, die für Formularentwickler wichtig und relevant sind.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner,Intermediate
version: 6.3,6.4,6.5
feature: Document Services
kt: 7071
topic: Entwicklung
source-git-commit: 1b4512fdb047bec15d72a8278fd0ce5dfafa309f
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 0%

---


# PDF

Portable Document Format (PDF) ist eigentlich eine Familie von Dateiformaten. In diesem Artikel werden die Formformate beschrieben, die für Formularentwickler am relevantesten sind. Viele der technischen Details und Standards verschiedener PDF-Typen entwickeln sich weiter und ändern sich. Einige dieser Formate und Spezifikationen sind ISO-Normen (International Organization for Standardization) und einige sind spezifisches geistiges Eigentum, das der Adobe gehört.

In diesem Artikel erfahren Sie, wie Sie verschiedene Arten von PDFs erstellen. Es hilft Ihnen zu verstehen, wie und warum Sie die einzelnen verwenden. Alle diese Typen funktionieren am besten im führenden Client-Tool für die Anzeige und Verwendung von PDFs - Adobe Acrobat DC.

Im Folgenden finden Sie ein Beispiel für eine PDF/A-Datei in Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Beispieldateien können [von hier heruntergeladen werden](assets/pdf-file-types.zip)

## XML Forms Architecture PDF

Adobe verwendet den Begriff &quot;PDF-Formular&quot;, um auf die interaktive und dynamische Forms zu verweisen, die Sie mit AEM Forms Designer erstellen. Die Forms und die Dateien, die Sie mit Designer erstellen, basieren auf der XML Forms Architecture (XFA) von Adobe. In vielerlei Hinsicht ist das XFA PDF-Dateiformat näher an einer HTML-Datei als an einer herkömmlichen PDF-Datei. Der folgende Code zeigt beispielsweise, wie ein einfaches Textobjekt in einer XFA-PDF-Datei aussieht.

![Textfeld](assets/text-field.JPG)

XFA Forms sind XML-basiert. Dieses gut strukturierte und flexible Format ermöglicht es einem AEM Forms-Server, Ihre Designer-Dateien in verschiedene Formate umzuwandeln, darunter herkömmliches PDF, PDF/A und HTML. Sie können die vollständige XML-Struktur Ihrer Forms in Designer anzeigen, indem Sie im Layout-Editor die Registerkarte &quot;XML-Quelle&quot;auswählen. Sie können in AEM Forms Designer sowohl statische als auch dynamische XFA-Forms erstellen.

## Statische PDF

Das Layout von statischen XFA-PDF forms ändert sich zur Laufzeit nie, kann jedoch für den Benutzer interaktiv sein. Im Folgenden finden Sie einige Vorteile statischer XFA-PDF forms:

* Das Layout von statischen XFA-PDF forms ändert sich zur Laufzeit nie, kann jedoch für den Benutzer interaktiv sein.
* Statische Forms unterstützen die Kommentar- und Markup-Tools von Acrobat.
* Mit dem statischen Forms können Sie Acrobat-Kommentare importieren und exportieren.
* Statische Forms unterstützen Schriftuntereinstellungen, die auf einem AEM Forms-Server vorgenommen werden können.
* Statische Forms können mit dem integrierten PDF-Viewer gerendert werden, der mit modernen Browsern geliefert wird.

>[!NOTE]
>
> Sie können statische PDF-Dateien mit AEM Forms Designer erstellen, indem Sie die XDP als statische Adobe PDF-Formulare speichern

## PDF-Formate

Portable Document Format (PDF) ist eigentlich eine Familie von Dateiformaten. In diesem Artikel werden die Formformate beschrieben, die für Formularentwickler am relevantesten sind. Viele der technischen Details und Standards verschiedener PDF-Typen entwickeln sich weiter und ändern sich. Einige dieser Formate und Spezifikationen sind ISO-Normen (International Organization for Standardization) und einige sind spezifisches geistiges Eigentum, das der Adobe gehört.

In diesem Artikel erfahren Sie, wie Sie verschiedene Arten von PDFs erstellen. Es hilft Ihnen, zu verstehen, wie und warum Sie jede einzelne verwenden. Alle diese Typen funktionieren am besten im führenden Client-Tool für die Anzeige und Verwendung von PDFs - Adobe Acrobat DC.

Dies ist ein Beispiel für eine PDF/A-Datei in Acrobat DC.

![pdfa](assets/pdfa-file-in-acrobat.png)

Beispieldateien können [von hier heruntergeladen werden](assets/pdf-file-types.zip)

### XFA PDF

Adobe verwendet den Begriff &quot;PDF-Formular&quot;, um auf die interaktiven und dynamischen Formulare zu verweisen, die Sie mit AEM Forms Designer erstellen. Es ist wichtig zu beachten, dass es einen anderen PDF-Formulartyp namens &quot;Acroform&quot;gibt, der sich von den PDF forms unterscheidet, die Sie in AEM Forms Designer erstellen. Die mit Designer erstellten Formulare und Dateien basieren auf der XML Forms Architecture (XFA) von Adobe. In vielerlei Hinsicht ist das XFA PDF-Dateiformat näher an einer HTML-Datei als an einer herkömmlichen PDF-Datei. Der folgende Code zeigt beispielsweise, wie ein einfaches Textobjekt in einer XFA-PDF-Datei aussieht.

![Textfeld](assets/text-field.JPG)

Wie Sie sehen können, sind XFA-Formulare XML-basiert. Dieses gut strukturierte und flexible Format ermöglicht es einem AEM Forms-Server, Ihre Designer-Dateien in verschiedene Formate umzuwandeln, darunter herkömmliches PDF, PDF/A und HTML. Sie können die vollständige XML-Struktur Ihrer Formulare in Designer anzeigen, indem Sie im Layout-Editor die Registerkarte &quot;XML-Quelle&quot;auswählen. Sie können sowohl statische als auch dynamische XFA-Formulare in AEM Forms Designer erstellen.

### Statische PDF

Statische XFA-PDF forms ändern ihr Layout zur Laufzeit nicht, können jedoch interaktiv sein. Im Folgenden finden Sie einige Vorteile statischer XFA-PDF forms:

* Statische XFA-PDF forms ändern ihr Layout zur Laufzeit nicht, können jedoch interaktiv sein.
* Statische Formulare unterstützen die Kommentar- und Markup-Tools von Acrobat.
* Statische Formulare ermöglichen den Import und Export von Acrobat-Kommentaren.
* Statische Formulare unterstützen die Schriftunterteilung. Hierbei handelt es sich um eine Technik, die auf einem AEM Forms-Server ausgeführt werden kann.
* Statische Formulare können mit dem integrierten PDF-Viewer wiedergegeben werden, der mit modernen Browsern geliefert wird.

>[!NOTE]
> Sie können statische PDF-Dateien mit AEM Forms Designer erstellen, indem Sie die XDP als Adobe Static PDF Form speichern

### Dynamische Forms

Dynamische XFA-PDFs können ihr Layout zur Laufzeit ändern, sodass die Kommentar- und Markup-Funktionen nicht unterstützt werden. Dynamische XFA-PDFs bieten jedoch die folgenden Vorteile:

* Dynamische Formulare unterstützen clientseitige Skripte, die das Layout und die Paginierung des Formulars ändern. Beispielsweise wird die Datei &quot;Purchase Order.xdp&quot;erweitert und paginiert, um eine unendliche Datenmenge aufzunehmen, wenn Sie sie als dynamisches Formular speichern.
* Dynamische Formulare unterstützen alle Eigenschaften Ihres Formulars zur Laufzeit, während statische Formulare nur eine Teilmenge unterstützen

>[!NOTE]
>
> Sie können dynamische PDF-Dateien mit AEM Forms Designer erstellen, indem Sie die XDP als Adobe Dynamic XML Form speichern

>[!NOTE]
>
> Dynamische Formulare können nicht mit den integrierten PDF-Viewern der modernen Browser gerendert werden.

### PDF-Datei (Herkömmliches PDF)

Ein zertifiziertes Dokument bietet PDF-Dokumenten und Forms-Empfängern zusätzliche Garantien hinsichtlich ihrer Authentizität und Integrität.

Das beliebteste und verbreitetste PDF-Format ist die herkömmliche PDF-Datei. Es gibt viele Möglichkeiten, eine herkömmliche PDF-Datei zu erstellen, einschließlich der Verwendung von Acrobat und vielen Tools von Drittanbietern. Acrobat bietet alle folgenden Möglichkeiten zum Erstellen herkömmlicher PDF-Dateien. Wenn Sie Acrobat nicht installiert haben, werden diese Optionen möglicherweise nicht auf Ihrem Computer angezeigt.

* Erfassen Sie den Druckstrom eines Desktop-Programms: Wählen Sie den Druckbefehl einer Authoring-Anwendung und dann das Adobe PDF-Druckersymbol. Anstelle einer gedruckten Kopie Ihres Dokuments haben Sie eine PDF-Datei Ihres Dokuments erstellt.
* Durch Verwendung des Acrobat PDFMaker-Plug-ins mit Microsoft Office-Anwendungen: Wenn Sie Acrobat installieren, wird Microsoft Office-Anwendungen ein Adobe PDF-Menü und dem Office-Band ein Symbol hinzugefügt. Sie können diese hinzugefügten Funktionen verwenden, um PDF-Dateien direkt in Microsoft Office zu erstellen
* Durch die Verwendung von Acrobat Distiller zum Konvertieren von PostScript- und Encapsulated PostScript (EPS)-Dateien in PDFs: Distiller wird in der Regel bei der Druckveröffentlichung und anderen Workflows verwendet, für die eine Konvertierung vom Postscript-Format in das PDF-Format erforderlich ist
* Im Hintergrund unterscheidet sich ein herkömmliches PDF-Dokument stark von einem XFA-PDF. Sie weist nicht dieselbe XML-Struktur auf. Da sie durch Erfassen des Druckstreams einer Datei erstellt wird, ist eine herkömmliche PDF eine statische und schreibgeschützte Datei.

Ein zertifiziertes Dokument bietet PDF-Dokumenten- und Formularempfängern zusätzliche Garantien hinsichtlich ihrer Authentizität und Integrität.

### Acroforms

Acroforms ist die ältere interaktive Formulartechnologie der Adobe. sie gehen zurück zur Acrobat-Version 3. Adobe stellt die [Acrobat Forms API Reference](assets/FormsAPIReference.pdf) vom Mai 2003 zur Verfügung, um die technischen Details für diese Technologie anzugeben. Acroforms sind eine Kombination aus
folgende Elemente:

* Ein herkömmliches PDF-Dokument, das das statische Layout und die Grafiken des Formulars definiert.
* Interaktive Formularfelder, die mit den Formularbeitern des Adobe Acrobat-Programms zusätzlich hervorgehoben werden. Diese Formularwerkzeuge sind eine kleine Teilmenge der in AEM Forms Designer verfügbaren Funktionen.

### PDF/A (PDFs für Archiv)

PDF/A (PDF für Archive) baut auf den Vorteilen herkömmlicher PDFs für die Dokumentenspeicherung mit vielen spezifischen Details auf, die die langfristige Archivierung verbessern. Das herkömmliche PDF-Dateiformat bietet viele Vorteile für die langfristige Dokumentenspeicherung. Der kompakte Charakter von PDF ermöglicht eine einfache Übertragung und spart Platz, und seine gut strukturierte Natur ermöglicht leistungsstarke Indizierungs- und Suchfunktionen. Die herkömmliche PDF-Datei unterstützt Metadaten umfassend und PDF unterstützt seit langem verschiedene Computerumgebungen.

Wie PDF ist PDF/A eine ISO-Standardspezifikation. Es wurde von einer Task Force entwickelt, zu der die AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) und das Verwaltungsbüro der US-Gerichte gehörten. Da das Ziel der PDF/A-Spezifikation darin besteht, ein langfristiges Archivformat bereitzustellen, werden viele PDF-Funktionen weggelassen, sodass die Dateien eigenständig sein können. Im Folgenden finden Sie einige wichtige Punkte zur Spezifikation, die die langfristige Reproduzierbarkeit der PDF/A-Datei verbessern:

* Alle Inhalte müssen in der Datei enthalten sein, und es kann keine Abhängigkeiten von externen Quellen wie Hyperlinks, Schriftarten oder Software-Programmen geben.
* Alle Schriftarten müssen eingebettet sein, und es müssen Schriftarten sein, die über eine unbegrenzte Lizenz für elektronische Dokumente verfügen.
* JavaScript ist nicht zulässig
* Transparenz ist nicht zulässig
* Verschlüsselung ist nicht zulässig
* Audio- und Videoinhalte sind nicht zulässig
* Farbräume müssen geräteunabhängig definiert werden
* Alle Metadaten müssen bestimmten Standards entsprechen

### PDF/A-Datei anzeigen

Zwei Dateien in den Beispieldateien wurden aus derselben Microsoft Word-Datei erstellt. Das eine wurde als herkömmliches PDF und das andere als PDF/A-Datei erstellt. Öffnen Sie diese beiden Dateien in Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Obwohl die Dokumente gleich aussehen, wird die PDF/A-Datei mit einem blauen Balken oben geöffnet, der angibt, dass Sie dieses Dokument im PDF/A-Modus anzeigen. Diese blaue Leiste ist die Dokumentmeldungsleiste von Acrobat, die beim Öffnen bestimmter PDF-Dateitypen angezeigt wird.

![pdf-img](assets/pdfa-message.png)

Die Dokumentmeldungsleiste enthält Anweisungen und ggf. Schaltflächen, mit denen Sie eine Aufgabe abschließen können. Es ist farbcodiert und Sie sehen die blaue Farbe, wenn Sie bestimmte PDF-Typen (wie diese PDF/A-Datei) sowie zertifizierte und digital signierte PDFs öffnen. Die Leiste ändert sich für PDF forms in violett und für PDF-Überprüfungen in gelb.

>[!NOTE]
>
> Wenn Sie auf Bearbeiten aktivieren klicken, wird die PDF/A-Kompatibilität dieses Dokuments aufgehoben.




