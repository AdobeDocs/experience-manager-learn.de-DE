---
title: Verwenden von Metadaten-Import und -Export in AEM Assets
seo-title: Verwenden von Metadaten-Import und -Export in AEM Assets
description: Die Import- und Exportfunktionen von AEM Assets-Metadaten ermöglichen es Inhaltserstellern, Asset-Metadaten einfach in AEM zu verschieben und aus der zu entfernen. Zudem können Sie die Leistungsfähigkeit von Microsoft Excel nutzen, um Metadaten im Maßstab zu bearbeiten und so die Massenaktualisierungsmetadaten für vorhandene Assets in AEM zu vereinfachen.
seo-description: Die Import- und Exportfunktionen von AEM Assets-Metadaten ermöglichen es Inhaltserstellern, Asset-Metadaten einfach in AEM zu verschieben und aus der zu entfernen. Zudem können Sie die Leistungsfähigkeit von Microsoft Excel nutzen, um Metadaten im Maßstab zu bearbeiten und so die Massenaktualisierungsmetadaten für vorhandene Assets in AEM zu vereinfachen.
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 3%

---


# Verwenden von Metadaten-Import und -Export in AEM Assets{#using-metadata-import-and-export-in-aem-assets}

Die Import- und Exportfunktionen von AEM Assets-Metadaten ermöglichen es Inhaltserstellern, Asset-Metadaten einfach in AEM zu verschieben und aus der zu entfernen. Zudem können Sie die Leistungsfähigkeit von Microsoft Excel nutzen, um Metadaten im Maßstab zu bearbeiten und so die Massenaktualisierungsmetadaten für vorhandene Assets in AEM zu vereinfachen.

## Metadaten-Export {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## Metadatenimport {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

[WeRetail-Sportordner](assets/we-retail-sports.zip)

[Asset-Metadatenpaket](assets/we-retail-sports-asset-metadata.zip) herunterladen

## Metadaten-Dateiformat {#metadata-file-format}

### CSV-Dateiformat

#### Erste Zeile

* Die erste Zeile der CSV-Datei definiert das Metadaten-Schema.
* Die erste Spalte ist standardmäßig auf `assetPath` eingestellt, was den absoluten JCR-Pfad für ein Asset enthält.

* Nachfolgende Spalten in der ersten Zeile verweisen auf andere Metadateneigenschaften eines Assets.

   * Beispiel : `dc:title, dc:description, jcr:title`

* Einzelwerteigenschaftsformat

   * `<metadata property name> {{<property type}}`
   * Wenn der Eigenschaftstyp nicht angegeben ist, wird standardmäßig &quot;String&quot;verwendet.
   * Beispiel: `dc:title {{String}}`

* Eigenschaftsname unterscheidet zwischen Groß- und Kleinschreibung
   * Richtig : `dc:title {{String}}`
   * Falsch: `Dc:Ttle {{String}}`

* Bei Eigenschaftstyp wird nicht zwischen Groß- und Kleinschreibung unterschieden
* Alle gültigen [JCR-Eigenschaftstypen](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) werden unterstützt

* Format der Eigenschaft mit mehreren Werten - `<metadata property name> {{<property type : MULTI }}`

#### Zweite Zeile in N Zeilen

* Die erste Spalte enthält den absoluten JCR-Pfad für ein Asset. Beispiel: /content/dam/asset1.jpg
* Die Metadateneigenschaft für ein Asset könnte fehlende Werte in der CSV-Datei enthalten. Fehlende Metadateneigenschaft für das betreffende Asset wird nicht aktualisiert.