---
title: Grundlegendes über die PDF Formular- und Dokumenten-Typen.
description: PDF ist eigentlich eine Familie von Dateiformaten. In diesem Artikel werden die PDF-Typen beschrieben, die für Entwicklerinnen und Entwickler von Formularen wichtig und relevant sind.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
jira: KT-7071
topic: Development
last-substantial-update: 2020-07-07T00:00:00Z
duration: 297
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 98%

---

# PDF

Portable Document Format (PDF) ist eigentlich eine Familie von Dateiformaten. In diesem Artikel werden die Dateiformate beschrieben, die für Entwicklerinnen und Entwickler von Formularen am relevantesten sind. Viele der technischen Details und Standards verschiedener PDF-Typen entwickeln sich weiter und ändern sich. Einige dieser Formate und Spezifikationen sind ISO-Normen (International Organization for Standardization), andere sind spezifisches geistiges Eigentum, das Adobe gehört.

In diesem Artikel erfahren Sie, wie Sie verschiedene Typen von PDF erstellen. Es hilft Ihnen zu verstehen, wie und warum Sie die einzelnen verwenden sollten. Alle diese Typen funktionieren am besten im führenden Client-Tool zum Anzeigen und Arbeiten mit PDFs – Adobe Acrobat DC.

Im Folgenden finden Sie ein Beispiel für eine PDF/A-Datei in Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Beispieldateien können [hier heruntergeladen werden](assets/pdf-file-types.zip)

## XML Forms Architecture PDF (XFA-PDF)

Adobe verwendet den Begriff XFA-PDF-Formular, um auf das interaktive und dynamische Formular zu verweisen, das Sie mit AEM Forms Designer erstellen. Die Formulare und Dateien, die Sie mit Designer erstellen, basieren auf der XML Forms Architecture (XFA) von Adobe. In vielerlei Hinsicht ist das XFA-PDF-Dateiformat näher an einer HTML-Datei als an einer herkömmlichen PDF-Datei. Der folgende Code zeigt beispielsweise, wie ein einfaches Textobjekt in einer XFA-PDF-Datei aussieht.

![Text-field](assets/text-field.JPG)

XFA Formulare sind XML-basiert. Dieses gut strukturierte und flexible Format ermöglicht es einem AEM Forms-Server, Ihre Designer-Dateien in verschiedene Formate umzuwandeln, darunter herkömmliches PDF, PDF/A und HTML. Sie können die vollständige XML-Struktur Ihrer Formulare in Designer anzeigen, indem Sie im Layout-Editor die Registerkarte „XML-Quelle“ auswählen. Sie können in AEM Forms Designer sowohl statische als auch dynamische XFA-Formulare erstellen.

## Statische PDF

Das Layout von statischen XFA-PDF-Formularen ändert sich zur Laufzeit nie, sie können jedoch für die Benutzenden interaktiv sein. Im Folgenden finden Sie einige Vorteile statischer XFA-PDF-Formulare:

* Das Layout von statischen XFA-PDF-Formularen ändert sich zur Laufzeit nie, sie können jedoch für die Benutzenden interaktiv sein.
* Statische Formulare unterstützen die Kommentar- und Markup-Tools von Acrobat.
* Mit einem statischen Formular können Sie Acrobat-Kommentare importieren und exportieren.
* Statische Formulare unterstützen Untereinstellungen für Schriften, die auf einem AEM Forms-Server vorgenommen werden können.
* Statische Formulare können mit dem integrierten PDF-Viewer gerendert werden, der in modernen Browsern enthalten ist.

>[!NOTE]
>
> Sie können statische PDFs mit AEM Forms Designer erstellen, indem Sie die XDP als statisches Adobe PDF-Formular speichern



### Dynamische Formulare

Dynamische XFA-PDFs können ihr Layout zur Laufzeit ändern, weswegen die Kommentar- und Markup-Funktionen nicht unterstützt werden. Dynamische XFA-PDFs bieten jedoch die folgenden Vorteile:

* Dynamische Formulare unterstützen Client-seitige Skripte, die das Layout und die Paginierung des Formulars ändern. Beispielsweise wird die Datei „Purchase Order.xdp“ erweitert und paginiert, um eine beliebige Datenmenge aufzunehmen, wenn Sie sie als dynamisches Formular speichern.
* Dynamische Formulare unterstützen alle Eigenschaften Ihres Formulars zur Laufzeit, während statische Formulare nur eine Teilmenge unterstützen.

* [In diesem Dokument erfahren Sie mehr über die Unterschiede zwischen statischen und dynamischen PDF-Formularen.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/pdf-forms-and-documents.html#:~:text=Dynamic%20forms%20support%20all%20the,forms%20support%20only%20a%20subset)

>[!NOTE]
>
> Sie können dynamische PDF-Dateien mit AEM Forms Designer erstellen, indem Sie die XDP als dynamisches XML-Formular von Adobe speichern.

>[!NOTE]
>
> Dynamische Formulare können nicht mit den integrierten PDF-Viewern der modernen Browser gerendert werden.

### PDF-Datei (herkömmliche PDF)

Ein zertifiziertes Dokument bietet den Empfängerinnen und Empfängern von PDF-Dokumenten und -Formularen eine zusätzliche Garantie hinsichtlich seiner Authentizität und Integrität.

Das beliebteste und verbreitetste PDF-Format ist die herkömmliche PDF-Datei. Es gibt viele Möglichkeiten, eine herkömmliche PDF-Datei zu erstellen, einschließlich der Verwendung von Acrobat und vielen Tools von Drittanbietern. Acrobat bietet alle folgenden Möglichkeiten, herkömmliche PDF-Dateien zu erstellen. Wenn Sie Acrobat nicht installiert haben, werden diese Optionen möglicherweise nicht auf Ihrem Computer angezeigt.

* Durch Erfassen des Druck-Streams einer Desktop-Anwendung: Wählen Sie in einer Textverarbeitung den Befehl „Drucken“ und wählen Sie das Adobe PDF-Druckersymbol. Anstelle einer gedruckten Kopie Ihres Dokuments wird dann eine PDF-Datei Ihres Dokuments erstellt.
* Mithilfe des Plug-ins Acrobat PDFMaker für Microsoft Office-Anwendungen: Wenn Sie Acrobat installieren, wird den Microsoft Office-Anwendungen ein Adobe PDF-Menü und der Office-Menüleiste ein entsprechendes Symbol hinzugefügt. Sie können diese zusätzlichen Funktionen verwenden, um PDF-Dateien direkt in Microsoft Office zu erstellen
* Durch Verwenden von Acrobat Distiller zum Konvertieren von PostScript- und Encapsulated PostScript(EPS)-Dateien in PDFs: Distiller wird in der Regel bei der Druckveröffentlichung und anderen Workflows verwendet, für die eine Konvertierung vom PostScript-Format in das PDF-Format erforderlich ist
* Unter der Haube unterscheidet sich eine herkömmliche PDF-Datei deutlich von einer XFA-PDF-Datei. Sie weist nicht die gleiche XML-Struktur auf, und da sie durch Erfassen des Druck-Streams einer Datei erstellt wird, ist eine herkömmliche PDF eine statische und schreibgeschützte Datei.

Ein zertifiziertes Dokument bietet den Empfängerinnen und Empfängern von PDF-Dokumenten und -Formularen eine zusätzliche Garantie hinsichtlich seiner Authentizität und Integrität.

### Acroforms

Acroforms bezeichnen die ältere Adobe-Technologie für interaktive Formulare. Sie gehen auf die Acrobat-Version 3 zurück. Adobe stellt die [Acrobat Forms API-Referenz](assets/FormsAPIReference.pdf) vom Mai 2003 zur Verfügung, um die technischen Details für diese Technologie zu erläutern. Acroforms sind eine Kombination aus den
nachfolgenden Elementen:

* Eine herkömmliche PDF, die das statische Layout und die Grafiken des Formulars definiert.
* Interaktive Formularfelder, die mit den Formular-Tools des Adobe-Acrobat-Programms aufgesetzt werden. Diese Formular-Tools sind eine kleine Teilmenge der in AEM Forms Designer verfügbaren Funktionen.

### PDF/A (PDFs für Archive)

PDF/A (PDFs für Archive) baut auf den Vorteilen der Dokumentenspeicherung traditioneller PDFs mit vielen spezifischen Details auf, die die langfristige Archivierung verbessern. Das herkömmliche PDF-Dateiformat bietet viele Vorteile für die langfristige Dokumentenspeicherung. Die Kompaktheit von PDF erleichtert die Übertragung und spart Platz, und die gute Strukturierung ermöglicht leistungsstarke Indexierungs- und Suchfunktionen. Das herkömmliche PDF-Format bietet umfangreiche Unterstützung für Metadaten, und PDF unterstützt seit langem verschiedene Computer-Umgebungen.

Wie PDF ist auch PDF/A eine ISO-Standardspezifikation. Es wurde von einer Arbeitsgruppe entwickelt, zu der die AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) und das Verwaltungsbüro der US-Gerichte gehörten. Da das Ziel der PDF/A-Spezifikation darin besteht, ein langfristiges Archivformat bereitzustellen, werden viele PDF-Funktionen weggelassen, sodass die Dateien eigenständig sein können. Im Folgenden finden Sie einige wichtige Punkte zur Spezifikation, die die langfristige Reproduzierbarkeit der PDF/A-Datei verbessern:

* Alle Inhalte müssen in der Datei selbst enthalten sein, und es kann keine Abhängigkeiten von externen Quellen wie Hyperlinks, Schriftarten oder Software-Programmen geben.
* Alle Schriftarten müssen eingebettet sein, und es müssen Schriftarten sein, die über eine unbegrenzte Lizenz für elektronische Dokumente verfügen.
* JavaScript ist nicht zulässig
* Transparenz ist nicht zulässig
* Verschlüsselung ist nicht zulässig
* Audio- und Videoinhalte sind nicht zulässig
* Farbräume müssen geräteunabhängig definiert werden
* Alle Metadaten müssen bestimmten Standards entsprechen

### Anzeigen einer PDF/A-Datei

Zwei Dateien in den Beispieldateien wurden aus derselben Microsoft Word-Datei erstellt. Die eine wurde als herkömmliche PDF und die andere als PDF/A-Datei erstellt. Öffnen Sie diese beiden Dateien in Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Obwohl die Dokumente gleich aussehen, wird die PDF/A-Datei mit einem blauen Balken oben geöffnet, der angibt, dass Sie dieses Dokument im PDF/A-Modus anzeigen. Diese blaue Leiste ist die Dokumentmeldungsleiste von Acrobat, die beim Öffnen bestimmter PDF-Dateitypen angezeigt wird.

![Pdf-img](assets/pdfa-message.png)

Die Dokumentmeldungsleiste enthält Anweisungen und ggf. Schaltflächen, mit denen Sie eine Aufgabe abschließen können. Sie ist farbcodiert, und Sie sehen sie in blau, wenn Sie bestimmte PDF-Typen (wie diese PDF/A-Datei) sowie zertifizierte und digital signierte PDFs öffnen. Die Leiste ändert sich in violett für PDF Formulare und in gelb, wenn Sie an einem PDF-Review teilnehmen.

>[!NOTE]
>
> Wenn Sie auf „Bearbeitung aktivieren“ klicken, wird dieses Dokument nicht mehr PDF/A-konform sein.
