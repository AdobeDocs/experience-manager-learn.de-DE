---
title: Verwenden von Metadatenimport und -export in AEM Assets
description: Erfahren Sie, wie Sie die Funktionen zum Metadatenimport und -export von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Inhaltsautorinnen und Inhaltsautoren eine Massenaktualisierung der Metadaten für vorhandene Assets durchführen.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '378'
ht-degree: 100%

---

# Verwenden von Metadatenimport und -export in AEM Assets {#metadata-import-and-export}

Erfahren Sie, wie Sie die Funktionen zum Metadatenimport und -export von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Inhaltsautorinnen und Inhaltsautoren eine Massenaktualisierung der Metadaten für vorhandene Assets durchführen.

## Metadatenexport {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/39394?quality=12&learn=on&captions=ger)

>[!TIP]
>
> Verwenden Sie beim Öffnen einer Metadaten-Export-CSV-Datei in Excel den [Excel-Importer](https://support.microsoft.com/de-de/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) anstelle eines Doppelklicks auf die Datei, um Probleme mit UTF-8-codierten CSV-Dateien zu vermeiden.
>
> Gehen Sie wie folgt vor, um die CSV-Datei für den Metadatenexport in Excel zu öffnen:
> 
> 1. Öffnen Sie Microsoft Excel.
> 1. Wählen Sie __Datei > Neu__, um eine leere Tabelle zu erstellen.
> 1. Wenn die leere Tabelle geöffnet ist, wählen Sie __Datei > Importieren__.
> 1. Wählen Sie die Datei __Text__ aus und klicken Sie auf __Importieren__.
> 1. Wählen Sie die exportierte CSV-Datei aus dem Dateisystem aus und klicken Sie auf __Daten abrufen__.
> 1. In Schritt 1 des Import-Assistenten wählen Sie __Getrennt__, legen Sie __Dateiherkunft__ auf __Unicode (UTF-8)__ fest und klicken Sie auf __Weiter__.
> 1. In Schritt 2 legen Sie __Trennzeichen__ auf __Komma__ fest und klicken Sie auf __Weiter__.
> 1. In Schritt 3 lassen Sie das __Spaltendatenformat__ unverändert und klicken Sie auf __Beenden__.
> 1. Wählen Sie __Importieren__ aus, um die Daten zur Tabelle hinzuzufügen.

## Metadatenimport {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Beim Vorbereiten einer zu importierenden CSV-Datei ist es einfacher, mithilfe der Funktion für den Metadatenexport eine CSV-Datei mit der Asset-Liste zu erstellen. Anschließend können Sie die generierte CSV-Datei ändern und mithilfe der Importfunktion importieren.

## Metadaten-CSV-Dateiformat {#metadata-file-format}

### Erste Zeile

* Die erste Zeile der CSV-Datei definiert das Metadatenschema.
* Die erste Spalte ist standardmäßig auf `assetPath` mit dem absoluten JCR-Pfad für ein Asset eingestellt.

* Die nachfolgenden Spalten verweisen in der ersten Zeile auf andere Metadateneigenschaften eines Assets.
   * Beispiel: `dc:title, dc:description, jcr:title`

* Einzelwert-Eigenschaftsformat:

   * `<metadata property name> {{<property type}}`
   * Wenn kein Eigenschaftstyp angegeben ist, wird standardmäßig „String“ verwendet.
   * Zum Beispiel: `dc:title {{String}}`

* Beim Eigenschaftsnamen wird zwischen Groß- und Kleinschreibung unterschieden:
   * Richtig: `dc:title {{String}}`
   * Falsch: `Dc:Title {{String}}`

* Beim Eigenschaftstyp wird nicht zwischen Groß- und Kleinschreibung unterschieden.
* Alle gültigen [JCR-Eigenschaftstypen](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) werden unterstützt.

* Mehrfachwert-Eigenschaftsformat: `<metadata property name> {{<property type : MULTI }}`

### Zweite bis N-te Zeile

* Die erste Spalte enthält den absoluten JCR-Pfad für ein Asset, Zum Beispiel: „/content/dam/asset1.jpg“.
* Die Metadateneigenschaft für ein Asset kann fehlende Werte in der CSV-Datei enthalten. Fehlende Metadateneigenschaften für dieses bestimmte Asset werden nicht aktualisiert.
