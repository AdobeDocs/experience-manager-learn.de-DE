---
title: Exportieren von Experience Fragments nach Adobe Target
description: Erfahren Sie, wie Sie AEM Experience Fragment als Adobe Target-Angebote veröffentlichen und exportieren.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 6%

---

# Experience Fragment nach Adobe Target exportieren {#experience-fragment-target}

Erfahren Sie, wie Sie AEM Experience Fragment als Adobe Target-Angebote exportieren.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Nächste Schritte

+ [Erstellen einer Target-Aktivität mit Experience Fragment-Angeboten](./create-target-activity.md)

## Fehlerbehebung

### Exportieren von Experience Fragments in Target schlägt fehl

#### Fehler

Der Export von Experience Fragment in Adobe Target ohne die richtigen Berechtigungen in Adobe Admin Console führt zum folgenden Fehler beim AEM-Autorendienst:

    ![Target API UI Error](assets/error-target-offer.png)

... und die folgenden Protokollmeldungen im `aemerror` log:

    ![Target API Console Error](assets/target-console-error.png)

#### Auflösung

1. Anmelden bei [Admin Console](https://adminconsole.adobe.com/) mit Administratorrechten für das verwendete Adobe Target-Produktprofil, aber AEM Integration
2. Auswählen __Produkte > Adobe Target > Produktprofil__
3. under __Integrationen__ auf, wählen Sie die Integration für Ihre AEM as a Cloud Service Umgebung aus (gleicher Name wie das Adobe I/O-Projekt).
4. Zuweisen __Editor__ oder __Genehmiger__ Rolle

   ![Target-API-Fehler](assets/target-permissions.png)

Durch Hinzufügen der richtigen Berechtigung zur Adobe Target-Integration sollte dieser Fehler behoben werden.

## Unterstützende Links

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
