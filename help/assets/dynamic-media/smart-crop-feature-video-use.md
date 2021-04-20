---
title: Verwenden von Smart Crop mit AEM Assets Dynamic Media
description: Smart Crop nutzt Adobe Sensei, um zeitaufwendige und kostspielige Aufgaben beim Zuschneiden von Inhalten für reaktionsfähiges Design zu vermeiden.
sub-product: dynamic-media
feature: Smart Crop, Image Profiles
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 17%

---


# Verwenden von &quot;Smart Crop&quot;mit AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop nutzt Adobe Sensei, um zeitaufwendige und kostspielige Aufgaben beim Zuschneiden von Inhalten für reaktionsfähiges Design zu vermeiden.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>Video geht davon aus, dass Ihre AEM Instanz im Dynamic Media S7-Modus ausgeführt wird. [Anweisungen zur Einrichtung von AEM mit Dynamic Media finden Sie hier.](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Managers Dynamic Media Smart Crop-Funktion umfasst

* AEM Asset-Administratoren können auf einfache Weise Profile für das intelligente Beschneiden auf der Grundlage der Gerätegröße und -breite erstellen.
* Intelligente Beschneidung kann für ein einzelnes Asset oder für alle Assets in einem Ordner durchgeführt werden.
* Die Größe des Layouts für die Bearbeitung intelligenter Zuschnitte kann zur besseren Sichtbarkeit angepasst werden.
* Die Dynamic Media-Komponente von AEM Sites unterstützt Smart Crop.
* Die veröffentlichte URL für das Asset mit intelligenten Zuschnitten ist verfügbar und kann mit Drittanbieteranwendungen verwendet werden, die gehostete Assets akzeptieren.

>[!NOTE]
>
>Die Koordinaten der intelligenten Zuschneidung sind vom Seitenverhältnis abhängig. Das heißt, dass für die verschiedenen Einstellungen für das smarte Zuschneiden in einem Bildprofil dasselbe Seitenverhältnis an Dynamic Media gesendet wird, wenn das Seitenverhältnis für die hinzugefügten Abmessungen im Bildprofil dasselbe ist. Aus diesem Grund wird im Editor für intelligente Beschneidung der gleiche Bereich vorgeschlagen. Beispielsweise würde eine Einstellung für die Zuschneidung von 100 x 100 und 200 x 200 dazu führen, dass dieselbe intelligente Zuschneidung vom System generiert wird.
