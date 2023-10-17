---
title: Generieren von Druckkanaldokumenten mithilfe überwachter Ordner
seo-title: Generating Print Channel Documents Using Watched Folder
description: Dies ist Teil 10 des mehrstufigen Tutorials zum Erstellen Ihres ersten Dokuments zur interaktiven Kommunikation für den Druckkanal. In diesem Teil generieren wir Druckkanaldokumente mithilfe des Mechanismus für überwachte Ordner.
seo-description: This is part 10 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will generate print channel documents using the watched folder mechanism.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '348'
ht-degree: 100%

---

# Generieren von Druckkanaldokumenten mithilfe überwachter Ordner

In diesem Teil generieren wir Druckkanaldokumente mithilfe des Mechanismus für überwachte Ordner.

Nach dem Erstellen und Testen Ihres Druckkanaldokuments benötigen wir einen Mechanismus, um dieses Dokument im Batch-Modus oder nach Bedarf zu generieren. In der Regel werden diese Arten von Dokumenten im Batch-Modus generiert. Der gängigste Mechanismus ist dabei die Verwendung von überwachten Ordnern.

Wenn Sie einen überwachten Ordner in AEM konfigurieren, verknüpfen Sie ein ECMA-Skript oder einen Java-Code, das bzw. der ausgeführt wird, wenn eine Datei im überwachten Ordner abgelegt wird. In diesem Artikel konzentrieren wir uns auf das ECMA-Skript, über das Druckkanaldokumente generiert und im Dateisystem speichert werden.

Die Konfiguration des überwachten Ordners und das ECMA-Skript gehören zu den Assets, die Sie zu [Beginn dieses Tutorials](introduction.md) importiert haben.

Die Eingabedatei, die im überwachten Ordner abgelegt wird, weist die folgende Struktur auf. Das ECMA-Skript liest die Kontonummern aus und generiert ein Druckkanaldokument für jedes dieser Konten.

Weitere Informationen zum ECMA-Skript zum Generieren von Dokumenten finden Sie [in diesem Artikel](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md).

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

Gehen Sie wie folgt vor, um ein Druckkanaldokument mithilfe des Mechanismus für überwachte Ordner zu generieren:

* [Führen Sie die in diesem Dokument beschriebenen Schritte aus.](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Melden Sie sich bei crx an und navigieren Sie zu „/etc/fd/watchfolder/scripts/PrintPDF.ecma“.

* Stellen Sie sicher, dass der Pfad zu „interactiveCommunicationsDocument“ auf das richtige Dokument verweist, das gedruckt werden soll(Zeile 1).
* Notieren Sie sich den Speicherort (Zeile 2). Sie können diesen nach Bedarf ändern.
* Stellen Sie sicher, dass der Eingabeparameter für das Formulardatenmodell an das Anfrageattribut gebunden ist und der Bindungswert auf „accountnumber“ festgelegt ist. Siehe folgenden Screenshot:
  ![Anfrage](assets/requestattributeprintchannel.gif)

* Erstellen Sie eine Datei „accountnumbers.xml“ mit folgendem Inhalt:

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* Legen Sie die XML-Datei unter „C:\RenderPrintChannel\input“ ab.

* Überprüfen Sie die PDF-Dateien am Speicherort, wie im ECMA-Skript angegeben.

## Nächste Schritte

[Öffnen der Agent-Benutzeroberfläche bei der Formularübermittlung](./opening-agent-ui-on-form-submission.md)