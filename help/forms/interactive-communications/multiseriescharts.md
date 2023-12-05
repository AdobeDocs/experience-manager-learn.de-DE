---
title: Diagramme mit mehreren Reihen in AEM Forms
description: Erstellen Sie ein geeignetes Formulardatenmodell, um Diagramme mit mehreren Reihen in Druck- und Web-Kanaldokumenten zu erstellen.
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
duration: 454
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 100%

---

# Diagramme mit mehreren Reihen

AEM Forms 6.5 bietet die Möglichkeit, Diagramme mit mehreren Reihen zu erstellen und zu konfigurieren. Die Diagramme mit mehreren Reihen werden normalerweise zusammen mit dem Diagrammtyp Linie, Balken, Spalten verwendet. Die folgende Abbildung ist ein gutes Beispiel für ein Diagramm mit mehreren Reihen. Es zeigt, wie sich eine Investition in Höhe von 10.000 $ in drei verschiedene Anlagefonds über einen bestimmten Zeitraum entwickelt. Um solche Diagramme in AEM Forms erstellen und verwenden zu können, müssen Sie ein entsprechendes Formulardatenmodell erstellen.

![Diagramm mit mehreren Reihen](assets/seriescharts.jfif)

Um in AEM Forms Diagramme mit mehreren Reihen zu erstellen, müssen Sie ein geeignetes Formulardatenmodell mit den erforderlichen Entitäten und Zuordnungen zwischen den Entitäten erstellen. Im folgenden Screenshot werden die Entitäten und die Zuordnungen zwischen den 3 Entitäten hervorgehoben. Auf der obersten Ebene gibt es eine Organisationsentität, die eine 1:n-Zuordnung mit der Fonds-Entität unterhält. Die Fonds-Entität wiederum hat eine 1:n-Zuordnung mit der Leistungsentität.

![Formulardatenmodell](assets/formdatamodel.jfif)

## Erstellen eines Formulardatenmodells für Diagramme mit mehreren Reihen

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### Konfigurieren von Diagrammen mit Linienreihen

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

Führen Sie die folgenden Schritte aus, um dies auf Ihrem System zu testen:

* [Laden Sie die Datei „MutualFundFactSheet.zip“ herunter und importieren Sie sie mit AEM Package Manager.](assets/mutualfundfactsheet.zip)
* [Laden Sie „SeriesChartSampleData.json“ auf Ihre Festplatte herunter.](assets/serieschartsampledata.json) Hierbei handelt es sich um die Beispieldaten, mit denen das Diagramm aufgefüllt wird.
* [Navigieren Sie zu „Formulare und Dokumente“.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Wählen Sie die Vorlage zur interaktiven Kommunikation „MutualFundGrowthFactSheet“ aus.
* Klicken Sie auf „Vorschau“ > „Druckkanal“ > „Beispieldaten hochladen“.
* Navigieren Sie zu der Beispieldatendatei, die im Rahmen dieses Artikels bereitgestellt wird.
* Sehen Sie sich den Druckkanal der interaktiven Kommunikation „MutualFundGrowthFactSheet“ mit den im vorherigen Schritt heruntergeladenen Beispieldaten in einer Vorschau an.
