---
title: Konfigurieren des Investment-Mix-Panels
description: Dies ist Teil 11 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil werden wir Tortendiagramme hinzufügen, um den aktuellen und den Modell-Investment-Mix anzuzeigen.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 97
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 100%

---

# Konfigurieren des Investment-Mix-Panels

In diesem Teil werden wir Tortendiagramme hinzufügen, um den aktuellen und den Modell-Investment-Mix anzuzeigen.

* Melden Sie sich bei AEM Forms an und navigieren Sie zu „Adobe Experience Manager“ > „Formulare“ > „Formulare und Dokumente“.

* Öffnen Sie den Ordner „401KStatement“.

* Öffnen Sie das 401KStatement im Bearbeitungsmodus.

* Wir fügen zwei Tortendiagramme hinzu, die den aktuellen und den Modell-Investment-Mix der Kontoinhaberin bzw. des Kontoinhabers darstellen.

## Aktueller Asset-Mix {#current-asset-mix}

* Tippen Sie auf das Bedienfeld „CurrentAssetMix“ auf der rechten Seite, wählen Sie das Symbol „+“ aus und fügen Sie eine Textkomponente ein. Ändern Sie den Standardtext in „Aktueller Asset-Mix“.

* Tippen Sie auf das Bedienfeld „CurrentAssetMix“, wählen Sie das Symbol „+“ aus und fügen Sie die Diagrammkomponente ein. Tippen Sie auf die neu eingefügte Diagrammkomponente und klicken Sie auf das Schraubenschlüssel-Symbol, um das Konfigurationseigenschaftsblatt für das Diagramm zu öffnen.

* Legen Sie die Eigenschaften wie in der Abbildung unten dargestellt fest. Stellen Sie sicher, dass der Diagrammtyp „Tortendiagramm“ ist.

* Beachten Sie das Datenmodellobjekt, das an die x- und y-Achsen gebunden ist. Sie müssen das Stammelement des Formulardatenmodells auswählen und es dann aufschlüsseln, um das entsprechende Element auszuwählen.

* ![currentassetmix](assets/currentassetmixchart.png)

## Modell-Asset-Mix {#model-asset-mix}

* Tippen Sie auf das Bedienfeld „RecommendedAssetMix“ auf der rechten Seite, wählen Sie das Symbol „+“ aus und fügen Sie eine Textkomponente ein. Ändern Sie den Standardtext in „Modell-Asset-Mix“.

* Tippen Sie auf das Bedienfeld „RecommendedAssetMix“, wählen Sie das Symbol „+“ aus und fügen Sie die Diagrammkomponente ein. Tippen Sie auf die neu eingefügte Diagrammkomponente und klicken Sie auf das Schraubenschlüssel-Symbol, um das Konfigurationseigenschaftsblatt für das Diagramm zu öffnen.

* Legen Sie die Eigenschaften wie in der Abbildung unten dargestellt fest. Stellen Sie sicher, dass der Diagrammtyp „Tortendiagramm“ ist.

* Beachten Sie das Datenmodellobjekt, das an die x- und y-Achsen gebunden ist. Sie müssen das Stammelement des Formulardatenmodells auswählen und es dann aufschlüsseln, um das entsprechende Element auszuwählen.

* ![assettype](assets/modelassettypechart.png)

## Nächste Schritte

[Vorbereiten der Bereitstellung des Web-Kanaldokuments](./parttwelve.md)
