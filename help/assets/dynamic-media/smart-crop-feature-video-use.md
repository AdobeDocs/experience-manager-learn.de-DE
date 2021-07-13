---
title: Verwenden von smartem Zuschneiden mit AEM Assets Dynamic Media
description: Smartes Zuschneiden nutzt Adobe Sensei, um die zeitaufwendigen und kostspieligen Aufgaben beim Zuschneiden von Inhalten für responsives Design zu eliminieren.
sub-product: dynamic-media
feature: Smartes Zuschneiden, Bildprofile
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 18%

---


# Verwenden von smartem Zuschneiden mit AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

Smartes Zuschneiden nutzt Adobe Sensei, um die zeitaufwendigen und kostspieligen Aufgaben beim Zuschneiden von Inhalten für responsives Design zu eliminieren.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>Video setzt voraus, dass Ihre AEM-Instanz im Dynamic Media S7-Modus ausgeführt wird. [Anweisungen zum Einrichten von AEM mit Dynamic Media finden Sie hier.](https://helpx.adobe.com/de/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Die Funktion für smartes Zuschneiden in Adobe Experience Manager umfasst

* AEM Asset-Administratoren können einfach Bildprofile für das smarte Zuschneiden basierend auf der Gerätebreite und -höhe erstellen.
* Smartes Zuschneiden kann für ein einzelnes Asset oder für alle Assets in einem Ordner durchgeführt werden.
* Die Größe des Layouts für die Bearbeitung von smartem Zuschneiden kann für eine bessere Sichtbarkeit geändert werden.
* Die Dynamic Media-Komponente von AEM Sites unterstützt das smarte Zuschneiden.
* Die veröffentlichte URL für das Asset mit smartem Zuschnitt ist für die Verwendung mit Anwendungen von Drittanbietern verfügbar, die gehostete Assets akzeptieren.

>[!NOTE]
>
>Die Koordinaten für das smarte Zuschneiden hängen vom Seitenverhältnis ab. Das heißt, dass für die verschiedenen Einstellungen für das smarte Zuschneiden in einem Bildprofil dasselbe Seitenverhältnis an Dynamic Media gesendet wird, wenn das Seitenverhältnis für die hinzugefügten Abmessungen im Bildprofil dasselbe ist. Aus diesem Grund wird im Editor für Smartes Zuschneiden derselbe Zuschnittbereich vorgeschlagen. Beispielsweise würde eine Zuschnitteinstellung von 100x100 und 200x200 dazu führen, dass vom System derselbe smarte Zuschnitt generiert wird.
