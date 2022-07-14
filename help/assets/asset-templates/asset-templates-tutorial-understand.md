---
title: Grundlegendes zu InDesign-Dateien und Asset-Vorlagen in AEM Assets
description: In diesem Video-Tutorial erfahren Sie, wie Sie eine InDesign-Datei definieren und alle zugehörigen Überlegungen zur Verwendung in der Funktion "Asset-Vorlagen"von AEM Assets aufrufen.
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: bf5b2fca04c09fd52df8ef8d9fca8b4b7bd2de2f
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# Grundlegendes zu InDesign-Dateien und Asset-Vorlagen in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

In diesem Video-Tutorial erfahren Sie, wie Sie eine InDesign-Datei definieren und alle zugehörigen Überlegungen zur Verwendung in der Funktion &quot;Asset-Vorlagen&quot;von AEM Assets aufrufen.

## Erstellen der InDesign-Vorlagendatei {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Herunterladen und Öffnen Sie die [**InDesign-Dateivorlage**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Öffnen Sie das Bedienfeld Tags ,** Überprüfen Sie die Tag-Benennungskonvention und beachten Sie, dass die in der InDesign-Datei vorhandenen Autorenelemente bereits mit Tags versehen sind. Beachten Sie, dass in AEM nur getaggte Elemente bearbeitbar sind.

   * **Fenster > Dienstprogramme > Tags**

3. Fügen Sie auf Seite ein neues Textelement hinzu, geben Sie den Text &quot;Header&quot;ein und wenden Sie die **Überschrift** Absatzformat.

   * **Fenster > Stile > Absatzformate**

   Erstellen Sie dann ein neues Tag mit dem Namen **Page2Heading.**

4. Fügen Sie das FPO-Logo-Bild hinzu ([bereitgestellt in der ZIP-Datei](assets/asset-templates-tutorial-video--supporting-files.zip)) zum Logo-Element auf der Übergeordneten Seite.

   * **Rechtsklick** und wählen Sie **&quot;Einpassen&quot;> &quot;Optionen für die Bildanpassung&quot;.. > &quot;Inhaltsanpassung&quot;> &quot;Frame proportional füllen&quot;**
   [Erfahren Sie mehr über die Optionen für die Frame-Anpassung.](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)und die für Ihren Anwendungsfall geeignet ist.

5. Kopieren Sie die Kopfzeile (Logo und Firmenname) aus der Übergeordneten Vorlage in Seite und Seite über Einfügen an Ort und Stelle.

   * Klicken Sie auf Seite 1 bei gedrückter Umschalt-Befehlstaste auf macOS oder bei gedrückter Umschalttaste-Alt-Taste auf Windows, um die Kopfzeile auszuwählen, die von der Übergeordneten Seite angezeigt wird, und sie zu löschen.
   * Kopieren Sie die Kopfzeile von der Übergeordneten Seite in Seite 1 über &quot;An Ort einfügen&quot;.
   * Wiederholen Sie die Schritte für Seite 2

6. Öffnen Sie das Bedienfeld Struktur , indem Sie auf die Elemente doppelklicken. Stellen Sie sicher, dass alle Strukturelemente den realen Elementen in der InDesign-Datei entsprechen. Entfernen Sie alle nicht verwendeten oder nicht benötigten Elemente. Stellen Sie sicher, dass das gesamte Tagging semantisch ist und Elemente ordnungsgemäß mit Tags versehen sind.

   >[!NOTE]
   >
   >Beachten Sie, dass eine schlecht erstellte InDesign-Datei die häufigste Ursache für Probleme mit AEM Asset-Vorlagen ist. Stellen Sie daher sicher, dass das Tagging und die Struktur sauber und korrekt sind.

## Erstellen und Bearbeiten einer Asset-Vorlage in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesign Server starten** auf Port 8080.
2. Stellen Sie sicher, dass **Die AEM-Autoreninstanz ist für die Interaktion mit Ihrer InDesign Server konfiguriert**(und umgekehrt).

   * [IDS Worker Cloud Service-Konfiguration](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Cloud Proxy Cloud Service-Konfiguration](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi-Konfiguration](http://localhost:4502/system/console/configMgr)

3. **InDesign-Datei in AEM Assets hochgeladen** und ermöglichen es AEM Workflow und InDesign Server, die Assets vollständig zu verarbeiten.
4. **Neue Vorlage erstellen** under **Assets > Vorlagen** und wählen Sie die InDesign-Datei aus, die in Schritt 4 in AEM hochgeladen wurde.
5. **Bearbeiten der Asset-Vorlage** erstellt in Schritt 5 und erstellen Sie die bearbeitbaren Felder.
6. Klicken **Fertig** , um die endgültigen Ausgabeformate mit hoher Wiedergabetreue der Asset-Vorlage zu generieren.
7. Klicken Sie auf die Karte &quot;Asset-Vorlage&quot;, um sie zu öffnen, und überprüfen Sie die Asset-Ausgabedarstellungen , um die Ausgabeformate mit hoher Wiedergabetreue herunterzuladen.

## Zusätzliche Ressourcen {#additional-resources}

InDesign der Vorlagendatei und unterstützende Bilder

Download [InDesign der Vorlagendatei und unterstützende Bilder](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesign CC-Testdownload](https://creative.adobe.com/products/download/indesign)
* InDesign Server-Testversion kann von heruntergeladen werden [Website zur Vorabversion der Adobe](https://www.adobeprerelease.com/) oder [CC Enterprise-Kunden können sich an ihren Kundenbetreuer wenden, um eine InDesign Server-Testlizenz anzufordern](https://www.adobe.com/products/indesignserver/faq.html)
