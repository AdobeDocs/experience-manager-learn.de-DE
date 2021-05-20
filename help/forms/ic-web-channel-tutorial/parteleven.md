---
title: Konfigurieren des Investment Mix-Bedienfelds
seo-title: Konfigurieren des Investment Mix-Bedienfelds
description: Dies ist Teil 11 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil werden wir Kreisdiagramme hinzufügen, um den aktuellen und modalen Investitionsmix anzuzeigen.
seo-description: Dies ist Teil 11 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil werden wir Kreisdiagramme hinzufügen, um den aktuellen und modalen Investitionsmix anzuzeigen.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---


# Konfigurieren des Investment Mix-Bedienfelds

In diesem Teil werden wir Kreisdiagramme hinzufügen, um den aktuellen und Modell-Investment-Mix anzuzeigen.

* Melden Sie sich bei AEM Forms an und navigieren Sie zu Adobe Experience Manager > Forms > Forms &amp; Documents.

* Öffnen Sie den Ordner 401KStatement .

* Öffnen Sie die 401KStatement im Bearbeitungsmodus.

* Wir fügen 2 Tortendiagramme hinzu, die den aktuellen und Modell-Investment-Mix des Kontoinhabers darstellen.

## Aktueller Asset-Mix {#current-asset-mix}

* Tippen Sie auf das Bedienfeld &quot;CurrentAssetMix&quot;auf der rechten Seite, wählen Sie das Symbol &quot;+&quot;aus und fügen Sie eine Textkomponente ein. Ändern Sie den Standardtext in &quot;Aktueller Asset-Mix&quot;.

* Tippen Sie auf das Bedienfeld &quot;CurrentAssetMix&quot;, wählen Sie das Symbol &quot;+&quot;aus und fügen Sie die Diagrammkomponente ein. Tippen Sie auf die neu eingefügte Diagrammkomponente und klicken Sie auf das Symbol &quot;Schraubenschlüssel&quot;, um das Konfigurationseigenschaftsblatt für das Diagramm zu öffnen.

* Legen Sie die Eigenschaften wie in der Abbildung unten dargestellt fest. Stellen Sie sicher, dass der Diagrammtyp Kreisdiagramm ist.

* Beachten Sie das Datenmodellobjekt, das an die X- und Y-Achsen gebunden ist. Sie müssen das Stammelement des Formulardatenmodells auswählen und dann im Drilldown-Verfahren das entsprechende Element auswählen.

* ![currentassetmix](assets/currentassetmixchart.png)

## Modell-Asset-Mix {#model-asset-mix}

* Tippen Sie auf das Bedienfeld &quot;RecommendedAssetMix&quot;auf der rechten Seite, wählen Sie das Symbol &quot;+&quot;aus und fügen Sie eine Textkomponente ein. Ändern Sie den Standardtext in &quot;Model Asset Mix&quot;.

* Tippen Sie auf das Bedienfeld &quot;RecommendedAssetMix&quot;, wählen Sie das Symbol &quot;+&quot;aus und fügen Sie die Diagrammkomponente ein. Tippen Sie auf die neu eingefügte Diagrammkomponente und klicken Sie auf das Symbol &quot;Schraubenschlüssel&quot;, um das Konfigurationseigenschaftsblatt für das Diagramm zu öffnen.

* Legen Sie die Eigenschaften wie in der Abbildung unten dargestellt fest. Stellen Sie sicher, dass der Diagrammtyp Kreisdiagramm ist.

* Beachten Sie das Datenmodellobjekt, das an die X- und Y-Achsen gebunden ist. Sie müssen das Stammelement des Formulardatenmodells auswählen und dann im Drilldown-Verfahren das entsprechende Element auswählen.

* ![assettype](assets/modelassettypechart.png)

