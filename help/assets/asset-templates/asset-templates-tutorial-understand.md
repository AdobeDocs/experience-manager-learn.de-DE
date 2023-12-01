---
title: Grundlegendes zu InDesign-Dateien und Asset-Vorlagen in AEM Assets
description: In diesem Video-Tutorial werden das Definieren einer InDesign-Datei sowie alle zugehörigen Aspekte zur Verwendung in der AEM Assets-Funktion „Asset-Vorlagen“ erläutert.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 100%

---

# Grundlegendes zu InDesign-Dateien und Asset-Vorlagen in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

In diesem Video-Tutorial werden das Definieren einer InDesign-Datei sowie alle zugehörigen Aspekte zur Verwendung in der AEM Assets-Funktion „Asset-Vorlagen“ erläutert.

## Erstellen der InDesign-Vorlagendatei {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Laden Sie die [**InDesign-Dateivorlage**](assets/asset-templates-tutorial-video--supporting-files.zip) herunter und öffnen Sie diese.
2. **Öffnen Sie das Bedienfeld „Tags“** und überprüfen Sie die Tag-Namenskonvention. Sie werden feststellen, dass die in der InDesign-Datei vorhandenen bearbeitbaren Elemente bereits mit Tags versehen sind. Denken Sie daran, dass in AEM nur getaggte Elemente bearbeitbar sind.

   * **Fenster > Dienstprogramme > Tags**

3. Fügen Sie auf der Seite ein neues Textelement hinzu, geben Sie den Text „Header“ ein und wenden Sie das Absatzformat **Überschrift** an.

   * **Fenster > Formatvorlagen > Absatzformate**

   Erstellen Sie dann ein neues Tag mit dem Namen **ÜberschriftSeite2** und wenden Sie es an.

4. Fügen Sie das FPO-Logobild ([bereitgestellt in der ZIP-Datei](assets/asset-templates-tutorial-video--supporting-files.zip)) zum Logoelement auf der Primärseite hinzu.

   * **Rechtsklick** und Auswahl von **Einpassen > Rahmenanpassungsoptionen… > Inhalt anpassen> Rahmen proportional füllen**

   [Erfahren Sie mehr über die Rahmenanpassungsoptionen](https://helpx.adobe.com/de/indesign/using/frames-objects.html#fitting_objects_to_frames) und finden Sie heraus, welche für Ihren Anwendungsfall geeignet ist.

5. Kopieren Sie den Header (Logo und Unternehmensname) aus der Primärvorlage unter „Seite und Seite“ über „An Originalposition einfügen“.

   * Klicken Sie bei gedrückter Umschalt- und Befehlstaste unter macOS bzw. bei gedrückter Umschalt- und Alt-Taste unter Windows auf Seite 1, um die Kopfzeile auszuwählen, die von der Primärseite angezeigt wird, und sie zu löschen.
   * Kopieren Sie die Kopfzeile von der Primärseite über „An Originalposition einfügen“ in Seite 1.
   * Wiederholen Sie die Schritte für Seite 2.

6. Öffnen Sie das Bedienfeld „Struktur“, indem Sie jeweils doppelklicken. Stellen Sie sicher, dass alle Strukturelemente den realen Elementen in der InDesign-Datei entsprechen. Entfernen Sie alle nicht verwendeten oder nicht benötigten Elemente. Stellen Sie sicher, dass das gesamte Tagging semantisch ist und die Elemente ordnungsgemäß mit Tags versehen sind.

   >[!NOTE]
   >
   >Beachten Sie, dass eine schlecht aufgebaute InDesign-Datei die häufigste Ursache für Probleme mit AEM-Asset-Vorlagen ist. Achten Sie daher darauf, dass Tagging und Struktur sauber und korrekt sind.

## Erstellen und Bearbeiten einer Asset-Vorlage in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **Starten Sie InDesign Server** auf Port 8080.
2. Stellen Sie sicher, dass die **AEM-Autoreninstanz für die Interaktion mit Ihrem InDesign Server konfiguriert ist** (und umgekehrt).

   * [IDS-Worker-Cloud-Service-Konfiguration](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Cloud-Proxy-Cloud-Service-Konfiguration](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM-Externalizer-OSGi-Konfiguration](http://localhost:4502/system/console/configMgr)

3. **Laden Sie die InDesign-Datei in AEM Assets hoch** und lassen Sie eine vollständige Verarbeitung der Assets durch AEM Workflow und InDesign Server zu.
4. **Erstellen Sie** unter **Assets > Vorlagen** eine neue Vorlage und wählen Sie die InDesign-Datei aus, die in Schritt 4 in AEM hochgeladen wurde.
5. **Bearbeiten Sie die Asset-Vorlage**, die in Schritt 5 erstellt wurden, und die bearbeitbaren Felder.
6. Klicken Sie auf **Fertig**, um die endgültigen Ausgabedarstellungen der Asset-Vorlage mit hoher Wiedergabetreue zu generieren.
7. Klicken Sie auf die Karte „Asset-Vorlage“, um die Asset-Ausgabedarstellungen zu öffnen, zu überprüfen und die Ausgabedarstellungen mit hoher Wiedergabetreue herunterzuladen.

## Zusätzliche Ressourcen {#additional-resources}

InDesign-Vorlagendatei und unterstützende Bilder

Laden Sie die [InDesign-Vorlagendatei und unterstützende Bilder](assets/asset-templates-tutorial-video--supporting-files-1.zip) herunter.

* [Laden Sie die InDesign CC-Testversion herunter.](https://creative.adobe.com/products/download/indesign)
* Die InDesign Server-Testversion kann über die [Adobe-Website für Vorabversionen](https://www.adobeprerelease.com/) heruntergeladen werden. [CC Enterprise-Kundinnen und -Kunden können sich auch an die Kundenbetreuung wenden, um eine InDesign Server-Testlizenz anzufordern.](https://www.adobe.com/de/products/indesignserver/faq.html)
