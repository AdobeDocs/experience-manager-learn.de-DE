---
title: Dynamic Media für Transparenz und Stapelverarbeitung der Inhaltsautomatisierung
description: Erfahren Sie, wie Sie mit Dynamic Media in AEM virtuelle Ausgabedarstellungen erstellen, die Transparenz verwalten und die Bildverarbeitung für die Wiederverwendung skalierbarer Inhalte automatisieren können.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
exl-id: 13a09ad3-bf55-4524-bf43-f1cdad368034
source-git-commit: 1061a13089e5891ee375c959a9361079c0f8ce20
workflow-type: ht
source-wordcount: '262'
ht-degree: 100%

---

# Dynamic Media für Transparenz und Stapelverarbeitung der Inhaltsautomatisierung

Erfahren Sie, wie Sie mit Dynamic Media in AEM virtuelle Ausgabedarstellungen erstellen, die Transparenz verwalten und die Bildverarbeitung für die Wiederverwendung skalierbarer Inhalte automatisieren können.

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Beispiel-Dynamic Media-Assets

Im Folgenden finden Sie Beispiele für Dynamic Media-Assets und ihre im Video verwendeten URLs.

>[!BEGINTABS]

>[!TAB Beispiele für Bildtransparenz]

Im Folgenden finden Sie die Beispiel-URLs für den Dynamic Media-Bild-Server, die im Video verwendet werden.

| Vorschau | Beschreibung | Dynamic Media-URL |
|-----------|------------------|---------|
| ![Standard](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Standard | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![Zusammengesetzt mit nahtloser Hintergrundbildebene](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&src=backdrop5-Camera&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans){width="250"} | Zusammengesetzt mit nahtloser Hintergrundbildebene | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&src=backdrop5-Camera&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans) |
| ![Roter Hintergrund](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&color=200,50,50&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans){width="250"} | Roter Hintergrund | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&color=200,50,50&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans) |
| ![Auf ovalen Pfad beschnitten](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&bgc=255,255,255){width="250"} | Auf ovalen Pfad beschnitten | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&bgc=255,255,255) |


>[!TAB Beispiele für Bildpfade]

Im Folgenden finden Sie die Beispiel-URLs für den Dynamic Media-Bild-Server, die im Video verwendet werden.

| Vorschau | Beschreibung | Dynamic Media-URL |
|-----------|------------------|---------|
| ![Auf 80 Pixel breit normalisiert (keine Transparenz)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Auf 80 Pixel breit normalisiert (keine Transparenz){width="250"} | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![Auf Pfad zuschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&wid=800){width="250"} | Auf Pfad zuschneiden | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&wid=800) |
| ![Auf Pfad beschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&wid=800){width="250"} | Auf Pfad beschneiden | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&wid=800) |
| ![Auf Pfad beschneiden und auf Pfad zuschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&cropPathE=Path%201&wid=800){width="250"} | Auf Pfad beschneiden und auf Pfad zuschneiden | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&cropPathE=Path%201&wid=800) |
| ![Auf einen anderen Pfad beschneiden](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&wid=800){width="250"} | Auf einen anderen Pfad beschneiden | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&wid=800) |
| ![Auf einen anderen Pfad beschneiden und roten Hintergrund erstellen](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&clipPathE=round&bgc=200,50,50&wid=800){width="250"} | Auf einen anderen Pfad beschneiden und roten Hintergrund erstellen | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&clipPathE=round&bgc=200,50,50&wid=800) |

>[!ENDTABS]


## Bild-Server-APIs für Dynamic Media

* [Bildbereitstellungs- und Rendering-API für Dynamic Media](https://experienceleague.adobe.com/de/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Dynamic Media-Snapshot-Vorschau](https://snapshot.scene7.com/)
