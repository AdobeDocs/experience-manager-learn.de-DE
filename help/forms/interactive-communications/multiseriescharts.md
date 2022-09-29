---
title: Diagramme mit mehreren Serien in AEM Forms
description: Erstellen Sie ein geeignetes Formulardatenmodell, um mehrere Serien-Diagramme in Druck- und Webkanaldokumenten zu erstellen.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 3%

---

# Diagramme mit mehreren Reihen

AEM Forms 6.5 bietet jetzt die Möglichkeit, mehrere Seriendiagramme zu erstellen und zu konfigurieren. Die Diagramme mit mehreren Reihen werden normalerweise zusammen mit dem Diagrammtyp Linie, Balken, Spalten verwendet. Die folgende Grafik ist ein gutes Beispiel für ein Diagramm mit mehreren Reihen. Die Grafik zeigt das Wachstum von 10.000 USD in drei verschiedenen Fonds auf Gegenseitigkeit über einen bestimmten Zeitraum. Um solche Diagramme in AEM Forms erstellen und verwenden zu können, müssen Sie das entsprechende Formulardatenmodell erstellen.

![Mehrreihen-Diagramm](assets/seriescharts.jfif)

Um in AEM Forms Diagramme mit mehreren Reihen zu erstellen, müssen Sie ein geeignetes Formulardatenmodell mit den erforderlichen Entitäten und Zuordnungen zwischen den Entitäten erstellen. Im folgenden Screenshot werden die Entitäten und die Verbindungen zwischen den 3 Entitäten hervorgehoben. Auf der obersten Ebene haben wir eine Organisation, die eine Eins-zu-viele-Verbindung mit der Fondsorganisation hat. Die Fondseinrichtung wiederum hat eine Eins-zu-viele-Verbindung mit der Leistungsentität.

![Formulardatenmodell](assets/formdatamodel.jfif)

## Formulardatenmodell für Diagramme mit mehreren Reihen erstellen

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)

### Liniendiagramme konfigurieren

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)

Um dies auf Ihrem System zu testen, führen Sie die folgenden Schritte aus

* [Laden Sie die Datei MutualFundFactSheet.zip mit AEM Package Manager herunter und importieren Sie sie.](assets/mutualfundfactsheet.zip)
* [Laden Sie SeriesChartSampleData.json auf Ihre Festplatte herunter.](assets/serieschartsampledata.json) Dies sind die Beispieldaten, die zum Ausfüllen des Diagramms verwendet werden.
* [Navigieren Sie zu Forms und Dokumente.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Wählen Sie die Vorlage für interaktive Kommunikation &quot;MutualFundGrowthFactSheet&quot;.
* Klicken Sie auf Vorschau | Druckkanal | Laden Sie Beispieldaten hoch.
* Navigieren Sie zu der Beispieldatendatei, die als Teil dieses Artikels bereitgestellt wird.
* Sehen Sie sich den Druckkanal der interaktiven Kommunikation &quot;MutualFundGrowthFactSheet&quot;mit den im vorherigen Schritt heruntergeladenen Beispieldaten an.
