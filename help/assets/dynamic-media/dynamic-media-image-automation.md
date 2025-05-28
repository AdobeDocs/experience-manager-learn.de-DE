---
title: Dynamic Media für Transparenz und Inhaltsautomatisierung - Batch-Verarbeitung
description: Erfahren Sie, wie Sie mit Dynamic Media in AEM virtuelle Ausgabedarstellungen erstellen, die Transparenz verwalten und die Bildverarbeitung für die Wiederverwendung skalierbarer Inhalte automatisieren können.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 542
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: 2ffe4706856f0dbf63f2916af010f23bdb7b0045
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 6%

---


# Dynamic Media für Transparenz und Inhaltsautomatisierung - Batch-Verarbeitung

Erfahren Sie, wie Sie mit Dynamic Media in AEM virtuelle Ausgabedarstellungen erstellen, die Transparenz verwalten und die Bildverarbeitung für die Wiederverwendung skalierbarer Inhalte automatisieren können.

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Beispiel für Dynamic Media-Assets

Im Folgenden finden Sie Beispiele für Dynamic Media-Assets und ihre im Video verwendeten URLs.

>[!BEGINTABS]

>[!TAB Beispiele für Bildtransparenz]

Im Folgenden finden Sie die Beispiel-URLs für den Dynamic Media-Bildserver, die im Video verwendet werden.

| Vorschau | Beschreibung | Dynamic Media-URL |
|-----------|------------------|---------|
| ![Standard](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Standard | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![Zusammengesetzt mit nahtloser Hintergrundbildebene](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Zusammengesetzt mit nahtloser Hintergrundbildebene | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Roter Hintergrund](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Roter Hintergrund | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Auf ovalen Pfad abgeschnitten](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | Auf ovalen Pfad abgeschnitten | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB Beispiele für Bildpfade]

Im Folgenden finden Sie die Beispiel-URLs für den Dynamic Media-Bildserver, die im Video verwendet werden.

| Vorschau | Beschreibung | Dynamic Media-URL |
|-----------|------------------|---------|
| ![Auf 80 Pixel breit normalisiert (keine Transparenz)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Auf 80 Pixel breit normalisiert (keine Transparenz){width="250"} | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![Auf Pfad zuschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | Auf Pfad zuschneiden | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![Auf Pfad beschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | An Pfad anschneiden | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![Auf Pfad beschneiden und auf Pfad zuschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | Auf Pfad beschneiden und auf Pfad beschneiden | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![Auf einen anderen Pfad abschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | An einen anderen Pfad anschneiden | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![Auf einen anderen Pfad abschneiden und roten Hintergrund erstellen](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | An einen anderen Pfad anschneiden und roten Hintergrund erstellen | [Verknüpfung](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## Dynamic Media Image Server-APIs

* [Dynamic Media Image Serving and Rendering-API](https://experienceleague.adobe.com/en/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Vorschau von Dynamic Media-Momentaufnahmen](https://snapshot.scene7.com/)