---
title: Exportieren von Experience Fragments nach Adobe Target
description: Erfahren Sie, wie Sie AEM Experience Fragment als Adobe Target-Angebote veröffentlichen und exportieren.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integrationen
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 8%

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

... und die folgenden Protokollmeldungen im Protokoll `aemerror`:

    ![Target API Console Error](assets/target-console-error.png)

#### Auflösung

1. Melden Sie sich bei [Admin Console](https://adminconsole.adobe.com/) mit Administratorrechten für das verwendete Adobe Target-Produktprofil an, aber die AEM.
2. Wählen Sie __Produkte > Adobe Target > Produktprofil__
3. Wählen Sie auf der Registerkarte __Integrationen__ die Integration für Ihre AEM als Cloud Service-Umgebung aus (gleicher Name wie das Adobe I/O-Projekt).
4. Zuweisen der Rolle __Bearbeiter__ oder __Genehmiger__

   ![Target-API-Fehler](assets/target-permissions.png)

Durch Hinzufügen der richtigen Berechtigung zur Adobe Target-Integration sollte dieser Fehler behoben werden.

## Unterstützende Links

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)