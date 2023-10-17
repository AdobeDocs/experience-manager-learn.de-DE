---
title: Verwenden von Metadatenimport und -export in AEM Assets
description: Erfahren Sie, wie Sie die Funktionen zum Metadatenimport und -export von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Inhaltsautorinnen und Inhaltsautoren eine Massenaktualisierung der Metadaten für vorhandene Assets durchführen.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '260'
ht-degree: 100%

---

# Verwenden von Metadatenimport und -export in AEM Assets {#metadata-import-and-export}

Erfahren Sie, wie Sie die Funktionen zum Metadatenimport und -export von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Inhaltsautorinnen und Inhaltsautoren eine Massenaktualisierung der Metadaten für vorhandene Assets durchführen.

## Metadatenexport {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

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
