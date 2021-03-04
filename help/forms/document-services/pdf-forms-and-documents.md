---
title: Verstehen der verschiedenen Typen von PDF forms und Dokumenten
description: PDF ist eigentlich eine Familie von Dateiformaten. Dieser Artikel beschreibt die Typen von PDFs, die für Formularentwickler wichtig und relevant sind.
solution: Experience Manager Forms
product: aem
type: Dokumentation
role: Entwickler
level: Anfänger,Zwischenraum
version: 6.3,6.4,6.5
feature: Document Services
topic: Entwicklung
kt: 7071
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---


# PDF-Formate

Das Portable Dokument Format (PDF) ist eine Produktfamilie von Dateiformaten. In diesem Artikel werden die für Formularentwickler am relevantesten Formate beschrieben. Viele technische Details und Standards der verschiedenen PDF-Typen entwickeln sich weiter und ändern sich. Einige dieser Formate und Spezifikationen sind ISO-Normen (International Organization for Standardization) und einige sind spezifisches geistiges Eigentum der Adobe.

Dieser Artikel zeigt Ihnen, wie Sie verschiedene Arten von PDFs erstellen. Es wird Ihnen helfen, zu verstehen, wie und warum Sie jedes verwenden. Diese Typen eignen sich am besten für das erstklassige Client-Tool zum Anzeigen und Arbeiten mit PDFs - Adobe Acrobat DC.

Dies ist ein Beispiel für eine PDF/A-Datei in Acrobat DC.
![pdfa](assets/pdfa-file-in-acrobat.png)

Beispieldateien können [von hier heruntergeladen werden](assets/pdf-file-types.zip)

## XFA-PDF

Adobe verwendet den Begriff &quot;PDF-Formular&quot;, um auf die interaktiven und dynamischen Formulare zu verweisen, die Sie mit AEM Forms Designer erstellen. Beachten Sie, dass es einen anderen PDF-Formulartyp gibt, den Acroform-Typ, der sich von den PDF forms unterscheidet, die Sie in AEM Forms Designer erstellen. Die mit Designer erstellten Formulare und Dateien basieren auf der XML Forms Architecture (XFA) der Adobe. In vielerlei Hinsicht ist das XFA PDF-Dateiformat einer HTML-Datei näher als einer herkömmlichen PDF-Datei. Der folgende Code zeigt beispielsweise, wie ein einfaches Textobjekt in einer XFA-PDF-Datei aussieht.

![Textfeld](assets/text-field.JPG)

Wie Sie sehen können, sind XFA-Formulare XML-basiert. Dieses gut strukturierte und flexible Format ermöglicht es einem AEM Forms-Server, Ihre Designer-Dateien in verschiedene Formate umzuwandeln, einschließlich herkömmlicher PDF-, PDF/A- und HTML-Formate. Sie können die vollständige XML-Struktur Ihrer Formulare in Designer anzeigen, indem Sie im Layout-Editor auf der Registerkarte &quot;XML-Quelle&quot;klicken. Sie können statische und dynamische XFA-Formulare in AEM Forms Designer erstellen.

## Statische PDF

Statische XFA-PDF forms ändern ihr Layout zur Laufzeit nicht, können aber interaktiv sein. Die folgenden Vorteile bieten statische XFA-PDF forms:

* Statische XFA-PDF forms ändern ihr Layout zur Laufzeit nicht, können aber interaktiv sein.
* Statische Formulare unterstützen die Werkzeuge für Kommentare und Markierungen von Acrobat.
* Mit statischen Formularen können Sie Acrobat-Kommentare importieren und exportieren.
* Statische Formulare unterstützen die Untereinstellung von Schriftarten. Dies ist eine Technik, die auf einem AEM Forms-Server ausgeführt werden kann.
* Statische Formulare können mit dem integrierten PDF-Viewer wiedergegeben werden, der mit modernen Browsern geliefert wird.

>[!NOTE]
> Sie können statische PDF-Dateien mit AEM Forms Designer erstellen, indem Sie die XDP als statisches PDF-Formular der Adobe speichern

## Dynamisches Forms

Dynamische XFA-PDFs können ihr Layout zur Laufzeit ändern, sodass die Kommentarfunktionen und Markierungen nicht unterstützt werden. Dynamische XFA-PDFs bieten jedoch folgende Vorteile:

* Dynamische Formulare unterstützen clientseitige Skripten, die das Layout und die Paginierung des Formulars ändern. Beispielsweise wird die Datei &quot;Purchase Order.xdp&quot;erweitert und paginiert, um eine unendliche Datenmenge aufzunehmen, wenn Sie sie als dynamisches Formular speichern
* Dynamische Formulare unterstützen alle Eigenschaften Ihres Formulars zur Laufzeit, während statische Formulare nur eine Teilmenge unterstützen


>[!NOTE]
> Sie können dynamische PDFs mit AEM Forms Designer erstellen, indem Sie die XDP als dynamisches XML-Formular der Adobe speichern

>[!NOTE]
> Dynamische Formulare können nicht mit den integrierten PDF-Viewern der modernen Browser gerendert werden.


## PDF-Datei (herkömmliche PDF)

Das beliebteste und am weitesten verbreitete PDF-Format ist die herkömmliche PDF-Datei. Es gibt viele Möglichkeiten, eine herkömmliche PDF-Datei zu erstellen, einschließlich der Verwendung von Acrobat und vielen Drittanbieterwerkzeugen. Acrobat bietet alle folgenden Möglichkeiten, herkömmliche PDF-Dateien zu erstellen. Wenn Acrobat nicht installiert ist, werden diese Optionen auf Ihrem Computer möglicherweise nicht angezeigt.

* Erfassen des Druckstreams einer Desktopanwendung: Wählen Sie den Befehl &quot;Drucken&quot;einer Authoring-Anwendung und dann das Adobe PDF-Druckersymbol. Anstelle einer gedruckten Kopie Ihres Dokuments haben Sie eine PDF-Datei Ihres Dokuments erstellt.
* Verwenden Sie das Acrobat PDFMaker-Plug-In mit Microsoft Office-Anwendungen: Wenn Sie Acrobat installieren, wird Microsoft Office-Anwendungen ein Adobe PDF-Menü und das Office-Menüband mit einem Symbol versehen. Sie können diese hinzugefügten Funktionen verwenden, um PDF-Dateien direkt in Microsoft Office zu erstellen.
* Verwenden von Acrobat Distiller zum Konvertieren von PostScript- und Encapsulated PostScript (EPS)-Dateien in PDFs: Distiller wird in der Regel für die Druckveröffentlichung und andere Workflows verwendet, bei denen eine Konvertierung vom PostScript-Format in das PDF-Format erforderlich ist
* Unter der Oberfläche unterscheidet sich eine herkömmliche PDF sehr von einer XFA-PDF. Es hat nicht dieselbe XML-Struktur. Da es durch Erfassen des Druckstreams einer Datei erstellt wird, ist eine herkömmliche PDF eine statische und schreibgeschützte Datei.

Ein zertifiziertes Dokument bietet PDF-Dokument- und Forms-Empfängern zusätzliche Garantien hinsichtlich Authentizität und Integrität.

## Acroforms

Acroforms sind die ältere interaktive Formulartechnologie der Adobe. sie gehen zurück auf Acrobat Version 3. Adobe bietet die Forms API-Referenz [Acrobat, datiert Mai 2003, um technische Details zu dieser Technologie anzugeben. ](assets/FormsAPIReference.pdf) Acroforms sind eine Kombination aus
Folgende Elemente:

* Eine herkömmliche PDF, die das statische Layout und die Grafik des Formulars definiert.
* Interaktive Formularfelder, die mit den Formularwerkzeugen des Adobe Acrobat-Programms überlagert werden. Beachten Sie, dass diese Formularwerkzeuge eine kleine Untergruppe der verfügbaren Funktionen von AEM Forms Designer sind.

## PDF/A (PDFs für Archiv)

PDF/A (PDF for Archives) baut auf den Vorteilen herkömmlicher PDFs für die Datenspeicherung von Dokumenten auf und enthält viele spezifische Details, die die langfristige Archivierung verbessern. Das herkömmliche PDF-Dateiformat bietet viele Vorteile für die Datenspeicherung langfristiger Dokumente. Der kompakte Charakter von PDF ermöglicht einen einfachen Transfer und spart Platz. Die gut strukturierte Form ermöglicht leistungsstarke Indizierungs- und Suchfunktionen. Die herkömmliche PDF-Datei unterstützt Metadaten und PDF unterstützt seit langem verschiedene Umgebung.

Wie PDF ist PDF/A eine ISO-Standardspezifikation. Es wurde von einer Aufgabe entwickelt, die AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) und das Verwaltungsbüro der US-Gerichte umfasste. Da das Ziel der PDF/A-Spezifikation darin besteht, ein langfristiges Archivierungsformat bereitzustellen, werden viele PDF-Funktionen weggelassen, damit die Dateien eigenständig sein können. Im Folgenden finden Sie einige wichtige Punkte zur Spezifikation, die die langfristige Reproduzierbarkeit der PDF/A-Datei verbessern:

* Der gesamte Inhalt muss in der Datei enthalten sein, und es kann keine Abhängigkeit von externen Quellen wie Hyperlinks, Schriftarten oder Software-Programmen geben.
* Alle Schriftarten müssen eingebettet sein, und es müssen Schriftarten sein, die über eine unbegrenzte Nutzungslizenz für elektronische Dokumente verfügen.
* JavaScript ist nicht zulässig
* Transparenz ist nicht zulässig
* Verschlüsselung ist nicht zulässig
* Audio- und Videoinhalte sind nicht zulässig
* Farbräume müssen geräteunabhängig definiert werden
* Alle Metadaten müssen bestimmte Standards erfüllen

## Anzeigen einer PDF/A-Datei

Zwei Dateien in den Beispieldateien wurden aus derselben Microsoft Word-Datei erstellt. Das eine wurde als herkömmliche PDF und das andere als PDF/A-Datei erstellt. Öffnen Sie diese beiden Dateien in Acrobat Professional:

simpleWordFile.pdf
simpleWordFilePDFA.pdf

Obwohl die Dokumente gleich aussehen, wird die PDF/A-Datei mit einem blauen Balken oben geöffnet, der angibt, dass Sie dieses Dokument im PDF/A-Modus anzeigen. Diese blaue Leiste ist die Acrobats Dokument-Meldungsleiste, die Sie sehen, wenn Sie bestimmte PDF-Dateitypen öffnen.

![pdf-img](assets/pdfa-message.png)

Die Dokument-Meldungsleiste enthält Anweisungen und ggf. Schaltflächen, mit denen Sie eine Aufgabe abschließen können. Es ist farbkodiert, und Sie sehen die blaue Farbe, wenn Sie bestimmte PDFs (wie diese PDF/A-Datei) sowie zertifizierte und digital signierte PDFs öffnen. Die Leiste wird für PDF forms violett und für PDF-Review gelb angezeigt.

>[!NOTE]
> Wenn Sie auf &quot;Bearbeitung aktivieren&quot;klicken, wird dieses Dokument aus der PDF/A-Kompatibilität entfernt.




