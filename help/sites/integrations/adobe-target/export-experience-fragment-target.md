---
title: Exportieren von Experience Fragments nach Adobe Target
description: Erfahren Sie, wie Sie AEM Erlebnisfragment als Adobe Target-Angebot veröffentlichen und exportieren.
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 6%

---


# Erlebnisfragment nach Adobe Target exportieren {#experience-fragment-target}

Erfahren Sie, wie Sie AEM Erlebnisfragment als Adobe Target-Angebot exportieren.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Nächste Schritte

+ [Erstellen einer Zielgruppe-Aktivität mit Experience Fragment-Angeboten](./create-target-activity.md)

## Fehlerbehebung

### Exportieren von Erlebnisfragmenten in die Zielgruppe schlägt fehl

#### Fehler

Beim Exportieren von Erlebnisfragment nach Adobe Target ohne die richtigen Berechtigungen in Adobe Admin Console wird der folgende Fehler beim AEM Author-Dienst angezeigt:

    ![UI-Fehler der Zielgruppe-API](assets/error-target-offer.png)

... und die folgenden Protokollmeldungen im `aemerror`-Protokoll:

    ![Zielgruppe API Console Error](assets/target-console-error.png)

#### Auflösung

1. Melden Sie sich bei [Admin Console](https://adminconsole.adobe.com/) mit Administratorrechten für das verwendete Adobe Target Product Profil an, aber die AEM Integration
2. Wählen Sie __Produkte > Adobe Target > Produkt-Profil__
3. Wählen Sie auf der Registerkarte __Integrationen__ die Integration für Ihren AEM als Cloud Service-Umgebung (gleicher Name wie das Adobe I/O-Projekt) aus.
4. Rolle __Editor__ oder __Genehmigende Person__ zuweisen

   ![Zielgruppen-API-Fehler](assets/target-permissions.png)

Durch Hinzufügen der richtigen Berechtigung zur Adobe Target-Integration sollte dieser Fehler behoben werden.

## Unterstützende Links

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)