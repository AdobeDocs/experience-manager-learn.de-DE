---
title: Verwenden von Metadaten-Import und -Export in AEM Assets
description: Erfahren Sie, wie Sie die Import- und Exportfunktionen von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Autoren von Inhalten Metadaten für vorhandene Assets stapelweise aktualisieren.
version: 6.3, 6.4, 6.5, cloud-service
topics: Content Management
feature: 'Metadaten  '
role: Admin
level: Zwischenschaltung
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: 0d012d61b7740e461e641dddd6c5255a022305ea
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 4%

---


# Verwenden von Metadaten-Import und -Export in AEM Assets {#metadata-import-and-export}

Erfahren Sie, wie Sie die Import- und Exportfunktionen von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Autoren von Inhalten Metadaten für vorhandene Assets stapelweise aktualisieren.

## Metadaten-Export {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## Metadatenimport {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> Beim Vorbereiten einer CSV-Datei für den Import ist es einfacher, mithilfe der Funktion &quot;Metadatenexport&quot;eine CSV-Datei mit der Liste von Assets zu erstellen. Anschließend können Sie die generierte CSV-Datei ändern und mit der Importfunktion importieren.

## Metadaten-CSV-Dateiformat {#metadata-file-format}

### Erste Zeile

* Die erste Zeile der CSV-Datei definiert das Metadaten-Schema.
* Die erste Spalte ist standardmäßig auf `assetPath` eingestellt, was den absoluten JCR-Pfad für ein Asset enthält.

* Nachfolgende Spalten in der ersten Zeile verweisen auf andere Metadateneigenschaften eines Assets.
   * Beispiel : `dc:title, dc:description, jcr:title`

* Einzelwerteigenschaftsformat

   * `<metadata property name> {{<property type}}`
   * Wenn der Eigenschaftstyp nicht angegeben ist, wird standardmäßig &quot;String&quot;verwendet.
   * Beispiel: `dc:title {{String}}`

* Bei Eigenschaftsnamen wird zwischen Groß- und Kleinschreibung unterschieden.
   * Richtig : `dc:title {{String}}`
   * Falsch: `Dc:Title {{String}}`

* Bei Eigenschaftstyp wird nicht zwischen Groß- und Kleinschreibung unterschieden
* Alle gültigen [JCR-Eigenschaftstypen](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) werden unterstützt

* Format der Eigenschaft mit mehreren Werten - `<metadata property name> {{<property type : MULTI }}`

### Zweite Zeile in N Zeilen

* Die erste Spalte enthält den absoluten JCR-Pfad für ein Asset. Beispiel: /content/dam/asset1.jpg
* Die Metadateneigenschaft für ein Asset könnte fehlende Werte in der CSV-Datei enthalten. Fehlende Metadateneigenschaften für das betreffende Asset werden nicht aktualisiert.
