---
title: Verwenden von Metadatenimport und -export in AEM Assets
description: Erfahren Sie, wie Sie die Import- und Export-Metadatenfunktionen von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Inhaltsautoren Metadaten für vorhandene Assets stapelweise aktualisieren.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 3%

---

# Verwenden von Metadatenimport und -export in AEM Assets {#metadata-import-and-export}

Erfahren Sie, wie Sie die Import- und Export-Metadatenfunktionen von Adobe Experience Manager Assets verwenden. Mit den Import- und Exportfunktionen können Inhaltsautoren Metadaten für vorhandene Assets stapelweise aktualisieren.

## Metadaten-Export {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## Metadatenimport {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> Beim Vorbereiten einer zu importierenden CSV-Datei ist es einfacher, mithilfe der Funktion &quot;Metadatenexport&quot;eine CSV-Datei mit der Asset-Liste zu erstellen. Anschließend können Sie die generierte CSV-Datei ändern und mithilfe der Importfunktion importieren.

## Metadaten-CSV-Dateiformat {#metadata-file-format}

### Erste Zeile

* Die erste Zeile der CSV-Datei definiert das Metadatenschema.
* Die Standardeinstellung der ersten Spalte ist `assetPath`, der den absoluten JCR-Pfad für ein Asset enthält.

* Nachfolgende Spalten in der ersten Zeile verweisen auf andere Metadateneigenschaften eines Assets.
   * Beispiel : `dc:title, dc:description, jcr:title`

* Einzelwert-Eigenschaftenformat

   * `<metadata property name> {{<property type}}`
   * Wenn kein Eigenschaftstyp angegeben ist, wird standardmäßig &quot;String&quot;verwendet.
   * Beispiel: `dc:title {{String}}`

* Beim Eigenschaftsnamen wird zwischen Groß- und Kleinschreibung unterschieden
   * Richtig : `dc:title {{String}}`
   * Falsch: `Dc:Title {{String}}`

* Beim Eigenschaftstyp wird nicht zwischen Groß- und Kleinschreibung unterschieden
* Alle gültig [JCR-Eigenschaftstypen](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) werden unterstützt

* Mehrwert-Eigenschaftenformat - `<metadata property name> {{<property type : MULTI }}`

### Zweite Zeile in N Zeilen

* Die erste Spalte enthält den absoluten JCR-Pfad für ein Asset. Beispiel: /content/dam/asset1.jpg
* Die Metadateneigenschaft für ein Asset kann fehlende Werte in der CSV-Datei enthalten. Fehlende Metadateneigenschaften für dieses bestimmte Asset werden nicht aktualisiert.
