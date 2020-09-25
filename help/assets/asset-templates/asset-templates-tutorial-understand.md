---
title: 'InDesign von Dateien und Asset-Vorlagen in AEM Assets '
description: Diese Videoschulung erläutert die Definition einer InDesign-Datei und alle zugehörigen Überlegungen zur Verwendung in der Asset-Vorlagenfunktion von AEM Assets.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# InDesign von Dateien und Asset-Vorlagen in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Diese Videoschulung erläutert die Definition einer InDesign-Datei und alle zugehörigen Überlegungen zur Verwendung in der Asset-Vorlagenfunktion von AEM Assets.

## Erstellen der InDesign-Vorlagendatei {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Herunterladen und Öffnen der [**InDesign-Dateivorlage**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Öffnen Sie das Tagsbedienfeld,** überprüfen Sie die Tag-Benennungskonvention und beachten Sie, dass die Autorenelemente in der InDesign-Datei bereits mit Tags versehen sind. Beachten Sie, dass nur markierte Elemente in AEM bearbeitet werden können.

   * **Fenster > Hilfsprogramme > Tags**

3. Geben Sie auf der Seite Hinzufügen ein neues Textelement den Text &quot;Kopfzeile&quot;ein und wenden Sie das Absatzformat &quot; **Überschrift** &quot;an.

   * **Fenster > Stile > Absatzformate**

   Erstellen Sie dann ein neues Tag mit dem Namen **Page2Heading und wenden Sie es an.**

4. hinzufügen Sie das Bild für das FPO-Logo (in der ZIP-Datei[angegeben](assets/asset-templates-tutorial-video--supporting-files.zip)) auf das Logo-Element auf der Seite Übergeordnet.

   * **Klicken Sie mit der rechten Maustaste** und wählen Sie **&quot;Anpassen&quot;> &quot;Rahmeneinpassungsoptionen&quot;... > Inhaltsanpassung > Rahmen proportional füllen**
   [Erfahren Sie mehr über die Optionen](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)für die Rahmeneinpassung und welche für Sie geeignet sind.

5. Kopieren Sie die Kopfzeile (Logos und Firmen) aus der Übergeordnet-Vorlage auf Seite und Seite über &quot;An Originalposition einfügen&quot;.

   * Klicken Sie auf Seite 1 bei gedrückter Umschalttaste auf macOS oder bei gedrückter Umschalttaste und Alt-Taste auf Windows, um die angezeigte Kopfzeile auf der Seite &quot;Übergeordnet&quot;auszuwählen und sie zu löschen.
   * Kopieren Sie auf der Seite &quot;Übergeordnet&quot;die Kopfzeile über &quot;An Position einfügen&quot;in Seite 1
   * Wiederholen Sie die Schritte für Seite 2

6. Öffnen Sie das Strukturbedienfeld, indem Sie mit der Dublette auf die einzelnen Elemente klicken, um sicherzustellen, dass alle Strukturelemente den tatsächlichen Elementen in der InDesign-Datei entsprechen. Entfernen Sie alle nicht verwendeten oder nicht benötigten Elemente. Vergewissern Sie sich, dass alle Tags semantisch sind und Elemente ordnungsgemäß getaggt werden.

   >[!NOTE]
   >
   >Denken Sie daran, dass eine schlecht erstellte InDesign-Datei die häufigste Ursache für Probleme mit AEM Asset-Vorlagen ist. Achten Sie daher darauf, dass Tagging und Struktur sauber und korrekt sind.

## Erstellen und Bearbeiten einer Asset-Vorlage in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Beginn InDesign Server** an Port 8080.
2. Vergewissern Sie sich, dass die **AEM Author-Instanz für die Interaktion mit Ihrer InDesign Server** konfiguriert ist (und umgekehrt).

   * [IDS Worker Cloud Service-Konfiguration](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Cloud Proxy Cloud Service-Konfiguration](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi-Konfiguration](http://localhost:4502/system/console/configMgr)

3. **Die InDesign-Datei wurde nach AEM Assets** hochgeladen und ermöglicht es AEM Arbeitsablauf und InDesign Server, die Assets vollständig zu verarbeiten.
4. **Erstellen Sie eine neue Vorlage** unter &quot; **Assets&quot;> &quot;Vorlagen** &quot;und wählen Sie die InDesign-Datei aus, die Sie in Schritt 4 hochgeladen haben.
5. **Bearbeiten Sie die in Schritt 5 erstellte Asset-Vorlage** und erstellen Sie die bearbeitbaren Felder.
6. Klicken Sie auf **Fertig** , um die endgültigen Darstellungen mit hoher Originaltreue der Asset-Vorlage zu generieren.
7. Klicken Sie auf die Asset-Vorlagenkarte, um sie zu öffnen, und überprüfen Sie die Asset-Darstellungen, um die hochauflösenden Darstellungen herunterzuladen.

## Zusätzliche Ressourcen {#additional-resources}

InDesign der Vorlagendatei und unterstützende Bilder

Herunterladen der [InDesign-Vorlagendatei und unterstützender Bilder](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesign CC-Testdownload](https://creative.adobe.com/products/download/indesign)
* [InDesign Server Testdownload](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
