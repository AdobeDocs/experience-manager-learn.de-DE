---
title: Multi-Serie Charts in AEM Forms
seo-title: Multi-Serie Charts in AEM Forms
description: Erstellen Sie ein geeignetes Formulardatenmodell, um mehrere Seriendiagramme in Print- und Web-Kanal-Dokumenten zu erstellen.
seo-description: Erstellen Sie ein geeignetes Formulardatenmodell, um mehrere Seriendiagramme in Print- und Web-Kanal-Dokumenten zu erstellen.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 1%

---


# Diagramme mit mehreren Reihen

AEM Forms 6.5 bietet jetzt die Möglichkeit, Diagramme für mehrere Serien zu erstellen und zu konfigurieren. Die Diagramme mit mehreren Reihen werden in der Regel zusammen mit dem Diagrammtyp &quot;Linie&quot;, &quot;Bar&quot;, &quot;Spalte&quot;verwendet. Das folgende Diagramm ist ein gutes Beispiel für ein Multi-Serie-Diagramm. Die Tabelle zeigt das Wachstum von 10.000 USD in drei verschiedenen Fonds auf Gegenseitigkeit über einen bestimmten Zeitraum. Um solche Diagramme in AEM Forms erstellen und verwenden zu können, müssen Sie das entsprechende Formulardatenmodell erstellen.

![multiseries](assets/seriescharts.jfif)

Zum Erstellen von Diagrammen mit mehreren Reihen in AEM Forms müssen Sie ein geeignetes Formulardatenmodell mit den erforderlichen Entitäten und Verbindungen zwischen den Entitäten erstellen. Im folgenden Screenshot werden die Entitäten und die Verbindungen zwischen den drei Entitäten hervorgehoben. Auf der obersten Ebene haben wir eine Organisation namens &quot;Organisation&quot;, die eine Eins-zu-viele-Verbindung mit der Fondsorganisation hat. Die Fondsorganisation wiederum hat eine Eins-zu-viele-Verbindung mit der Performance-Einheit.

![formdatamodel](assets/formdatamodel.jfif)


## Formulardatenmodell für Diagramme mit mehreren Reihen erstellen

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Liniendiagramme konfigurieren

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Gehen Sie wie folgt vor, um dies auf Ihrem System zu testen

* [Laden Sie die Datei MutualFundFactSheet.zip mit AEM Package Manager herunter und importieren Sie sie.](assets/mutualfundfactsheet.zip)
* [Laden Sie SeriesChartSampleData.json auf Ihre Festplatte herunter.](assets/serieschartsampledata.json) Dies sind die Musterdaten, die zum Ausfüllen des Diagramms verwendet werden.
* [Navigieren Sie zu Forms und Dokumenten.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Wählen Sie behutsam die interaktive Kommunikationsvorlage &quot;MutualFundGrowthFactSheet&quot;.
* Auf Vorschau klicken | Beispieldaten hochladen.
* Navigieren Sie zu der Beispieldatendatei, die in diesem Artikel bereitgestellt wird.
* Vorschau des Kanals der interaktiven Kommunikation &quot;MutualFundGrowthFactSheet&quot; mit den im vorherigen Schritt heruntergeladenen Beispieldaten.
