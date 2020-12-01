---
title: Verwenden von Smart-Zuschneiden mit AEM Assets Dynamic Media
seo-title: Verwenden von Smart-Zuschneiden mit AEM Assets Dynamic Media
description: Smart Crop nutzt Adobe Sensei, um zeitaufwendige und kostspielige Aufgaben beim Zuschneiden von Inhalten für reaktionsfähiges Design zu vermeiden.
seo-description: Smart Crop nutzt Adobe Sensei, um zeitaufwendige und kostspielige Aufgaben beim Zuschneiden von Inhalten für reaktionsfähiges Design zu vermeiden.
uuid: 2cb27aa8-644d-4b17-8ffc-f6a99f95cfd2
discoiquuid: e4b8534c-fa64-491f-86ec-4dbe50cd6bf7
sub-product: dynamic-media
feature: smart-crop, image-profiles
topics: images, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 13%

---


# Verwenden von Smart-Zuschneiden mit AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop nutzt Adobe Sensei, um zeitaufwendige und kostspielige Aufgaben beim Zuschneiden von Inhalten für reaktionsfähiges Design zu vermeiden.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>Video geht davon aus, dass Ihre AEM-Instanz im Modus Dynamische Medien S7 ausgeführt wird. [Anweisungen zum Einrichten von AEM mit dynamischen Medien finden Sie hier.](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Managers Funktion zum Zuschneiden auf dynamische Medien umfasst

* AEM Asset-Administratoren können auf einfache Weise Profile für das intelligente Beschneiden auf der Grundlage der Gerätegröße und -breite erstellen.
* Intelligente Beschneidung kann für ein einzelnes Asset oder für alle Assets in einem Ordner durchgeführt werden.
* Die Größe des Layouts für die Bearbeitung intelligenter Zuschnitte kann zur besseren Sichtbarkeit angepasst werden.
* Die Komponente &quot;Dynamische Medien&quot;von AEM-Sites unterstützt Smart Crop.
* Die veröffentlichte URL für das Asset mit intelligenten Zuschnitten ist verfügbar und kann mit Drittanbieteranwendungen verwendet werden, die gehostete Assets akzeptieren.

>[!NOTE]
>
>Die Koordinaten der intelligenten Zuschneidung sind vom Seitenverhältnis abhängig. Das heißt, dass für die verschiedenen Einstellungen für das smarte Zuschneiden in einem Bildprofil dasselbe Seitenverhältnis an Dynamic Media gesendet wird, wenn das Seitenverhältnis für die hinzugefügten Abmessungen im Bildprofil dasselbe ist. Aus diesem Grund wird im Editor für intelligente Beschneidung der gleiche Bereich vorgeschlagen. Beispielsweise würde eine Einstellung für die Zuschneidung von 100 x 100 und 200 x 200 dazu führen, dass dieselbe intelligente Zuschneidung vom System generiert wird.
