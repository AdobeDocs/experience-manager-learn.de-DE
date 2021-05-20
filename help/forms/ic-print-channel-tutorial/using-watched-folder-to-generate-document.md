---
title: Generieren von Druckkanaldokumenten mit einem überwachten Ordner
seo-title: Generieren von Druckkanaldokumenten mit einem überwachten Ordner
description: Dies ist Teil 10 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments für den Druckkanal. In diesem Teil generieren wir Druckkanaldokumente mithilfe des Mechanismus für überwachte Ordner.
seo-description: Dies ist Teil 10 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments für den Druckkanal. In diesem Teil generieren wir Druckkanaldokumente mithilfe des Mechanismus für überwachte Ordner.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 1%

---


# Generieren von Druckkanaldokumenten mit einem überwachten Ordner

In diesem Teil generieren wir Druckkanaldokumente mithilfe des Mechanismus für überwachte Ordner.

Nach dem Erstellen und Testen Ihres Druckkanaldokuments benötigen wir einen Mechanismus, um dieses Dokument im Batch-Modus oder bei Bedarf zu generieren. In der Regel werden diese Arten von Dokumenten im Batch-Modus generiert und der gängigste Mechanismus ist die Verwendung von überwachten Ordnern.

Wenn Sie einen überwachten Ordner in AEM konfigurieren, verknüpfen Sie ein ECMA-Skript oder einen Java-Code, der ausgeführt wird, wenn eine Datei im überwachten Ordner abgelegt wird. In diesem Artikel werden wir uns auf das ECMA-Skript konzentrieren, das Druckkanaldokumente generiert und im Dateisystem speichert.

Die Konfiguration des überwachten Ordners und das ECMA-Skript sind Teil der Assets, die Sie am [Anfang dieses Tutorials](introduction.md) importiert haben

Die Eingabedatei, die im überwachten Ordner abgelegt wird, weist die folgende Struktur auf. Das ECMA-Skript liest die Kontonummern und generiert ein Druckkanaldokument für jedes dieser Konten.

Weitere Informationen zum ECMA-Skript zum Generieren von Dokumenten finden Sie in [diesem Artikel](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

Gehen Sie wie folgt vor, um Druckkanaldokumente mithilfe des Mechanismus für überwachte Ordner zu generieren:

* [Führen Sie die in diesem Dokument beschriebenen Schritte aus.](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Melden Sie sich bei crx an und navigieren Sie zu /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Stellen Sie sicher, dass der Pfad zu InteractiveCommunicationsDocument auf das richtige Dokument verweist, das Sie drucken möchten.( Zeile 1)
* Notieren Sie sich die saveLocation (Zeile 2). Sie können sie nach Bedarf ändern.
* Stellen Sie sicher, dass der Eingabeparameter für das Formulardatenmodell an das Anforderungsattribut gebunden ist und der Bindungswert auf &quot;Anschlussnummer&quot;eingestellt ist. Siehe Screenshot unten.
   ![request](assets/requestattributeprintchannel.gif)

* Erstellen Sie die Datei &quot;accountnumbers.xml&quot;mit folgendem Inhalt

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

* Legen Sie die XML-Datei unter C:\RenderPrintChannel\input ab.

* Überprüfen Sie die PDF-Dateien am Speicherort, wie im ECMA-Skript angegeben.




