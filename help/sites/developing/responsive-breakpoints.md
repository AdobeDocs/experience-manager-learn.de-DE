---
title: Responsive Breakpoints
description: Erfahren Sie, wie Sie neue responsive Haltepunkte für AEM responsiven Seiten-Editor konfigurieren.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# Responsive Breakpoints

Erfahren Sie, wie Sie neue responsive Haltepunkte für AEM responsiven Seiten-Editor konfigurieren.

## Erstellen von CSS-Haltepunkten

Erstellen Sie zunächst Medienhaltepunkte in der CSS für AEM responsives Raster, an die die responsive AEM-Site gebunden ist.

In `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` erstellen Sie Ihre Haltepunkte, die zusammen mit dem mobilen Emulator verwendet werden sollen. Beachten Sie die `max-width` für jeden Haltepunkt, da dies die CSS-Haltepunkte den AEM responsiven Seiteneditor-Haltepunkten zuordnet.

![Erstellen neuer responsiver Haltepunkte](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Anpassen der Haltepunkte der Vorlage

Öffnen Sie die `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` Datei und Aktualisierung `cq:responsive/breakpoints` mit Ihren neuen Breakpoint-Knotendefinitionen. Jeder [CSS-Breakpoint](#create-new-css-breakpoints) sollte über einen entsprechenden Knoten unter verfügen. `breakpoints` mit `width` -Eigenschaft, die auf den CSS-Haltepunkt `max-width`.

![Responsive Breakpoints der Vorlage anpassen](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Erstellen von Emulatoren

AEM Emulatoren müssen definiert sein, damit Autoren die responsive Ansicht auswählen können, die im Seiteneditor bearbeitet werden soll.

Erstellen Sie Emulsionsknoten unter `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Beispiel: `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Kopieren Sie einen Referenzemulatorknoten aus `/libs/wcm/mobile/components/emulators` in der CRXDE Lite, um die Kopie zu aktualisieren und die Knotendefinition zu beschleunigen.

![Erstellen neuer Emulatoren](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Gerätegruppe erstellen

Gruppieren Sie die Emulatoren in [Bereitstellung im AEM Seiteneditor](#update-the-templates-device-group).

Erstellen `/apps/settings/mobile/groups/<name of device group>` Knotenstruktur unter `/ui.apps/src/main/content/jcr_root`.

![Neue Gerätegruppe erstellen](./assets/responsive-breakpoints/create-new-device-group.jpg)

Erstellen Sie eine `.content.xml` Datei in `/apps/settings/mobile/groups/<device group name>` und definieren Sie die neuen Emulatoren mithilfe des Codes ähnlich dem folgenden:

![Neues Gerät erstellen](./assets/responsive-breakpoints/create-new-device.jpg)

## Aktualisieren der Gerätegruppe der Vorlage

Ordnen Sie die Gerätegruppe schließlich wieder der Seitenvorlage zu, damit die Emulatoren im Seiteneditor für Seiten verfügbar sind, die aus dieser Vorlage erstellt wurden.

Öffnen Sie die `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` und aktualisieren Sie die `cq:deviceGroups` -Eigenschaft, um auf die neue mobile Gruppe zu verweisen (z. B. `cq:deviceGroups="[mobile/groups/customdevices]"`)
