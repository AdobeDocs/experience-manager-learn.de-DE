---
title: Responsive Breakpoints
description: Erfahren Sie, wie Sie neue responsive Breakpoints für den responsiven Seiteneditor von AEM konfigurieren.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# Responsive Breakpoints

Erfahren Sie, wie Sie neue responsive Breakpoints für den responsiven Seiteneditor von AEM konfigurieren.

## Erstellen von CSS-Breakpoints

Erstellen Sie zunächst Medien-Breakpoints in der CSS für das responsive Raster von AEM, an die die responsive AEM-Site gebunden ist.

Erstellen Sie in der Datei `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` Ihre Breakpoints, die zusammen mit dem mobilen Emulator verwendet werden sollen.  Achten Sie auf die `max-width` für jeden Breakpoint, da dies die CSS-Breakpoints den responsiven Seiteneditor-Breakpoints von AEM zuordnet.

![Erstellen neuer responsiver Breakpoints](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Anpassen der Breakpoints der Vorlage

Öffnen Sie die Datei `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` und aktualisieren Sie `cq:responsive/breakpoints` mit Ihren Definitionen der neuen Breakpoint-Knoten.  Jeder [CSS-Breakpoint](#create-new-css-breakpoints) sollte einen entsprechenden Knoten unter `breakpoints` haben, dessen `width`-Eigenschaft auf die `max-width` des CSS-Breakpoints gesetzt ist.

![Anpassen der responsiven Breakpoints der Vorlage](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Erstellen von Emulatoren

AEM-Emulatoren müssen definiert sein, damit Autorinnen und Autoren die responsive Ansicht auswählen können, die im Seiteneditor bearbeitet werden soll.

Erstellen Sie Emulator-Knoten unter `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Beispiel: `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Kopieren Sie einen Referenz-Emulator-Knoten von `/libs/wcm/mobile/components/emulators` in CRXDE Lite und aktualisieren Sie die Kopie, um die Knotendefinition zu beschleunigen.

![Erstellen neuer Emulatoren](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Erstellen einer Gerätegruppe

Gruppieren Sie die Emulatoren, um [sie im AEM-Seiteneditor verfügbar zu machen](#update-the-templates-device-group).

Erstellen Sie die Knotenstruktur `/apps/settings/mobile/groups/<name of device group>` unter `/ui.apps/src/main/content/jcr_root`.

![Erstellen einer neuen Gerätegruppe](./assets/responsive-breakpoints/create-new-device-group.jpg)

Erstellen Sie eine Datei `.content.xml` in `/apps/settings/mobile/groups/<device group name>` und definieren Sie 
die neuen Emulatoren mit einem Code ähnlich dem untenstehenden:

![Erstellen eines neuen Geräts](./assets/responsive-breakpoints/create-new-device.jpg)

## Aktualisieren der Gerätegruppe der Vorlage

Ordnen Sie die Gerätegruppe schließlich wieder der Seitenvorlage zu, damit die Emulatoren im Seiteneditor für Seiten verfügbar sind, die aus dieser Vorlage erstellt wurden.

Öffnen Sie die Datei `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` und aktualisieren Sie die Eigenschaft `cq:deviceGroups`, um auf die neue mobile Gruppe zu verweisen (zum Beispiel `cq:deviceGroups="[mobile/groups/customdevices]"`).
