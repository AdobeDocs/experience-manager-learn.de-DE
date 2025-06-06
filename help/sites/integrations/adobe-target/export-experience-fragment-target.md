---
title: Exportieren von Experience Fragments nach Adobe Target
description: Erfahren Sie, wie Sie AEM Experience Fragments als Adobe Target-Angebote veröffentlichen und exportieren.
feature: Experience Fragments
version: Experience Manager as a Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '180'
ht-degree: 100%

---

# Exportieren von Experience Fragments nach Adobe Target {#experience-fragment-target}

Erfahren Sie, wie Sie AEM Experience Fragments als Adobe Target-Angebote exportieren.

>[!VIDEO](https://video.tv.adobe.com/v/328971?quality=12&learn=on&captions=ger)

## Nächste Schritte

+ [Erstellen einer Target-Aktivität mit Experience Fragment-Angeboten](./create-target-activity.md)

## Fehlerbehebung

### Fehler beim Exportieren von Experience Fragments nach Target

#### Fehler

Der Export von Experience Fragments nach Adobe Target ohne die korrekten Berechtigungen in Adobe Admin Console führt beim AEM-Author-Service zu folgendem Fehler:

![Target-API-Benutzeroberflächen-Fehler](assets/error-target-offer.png)

Außerdem werden die folgenden Protokollmeldungen im `aemerror`-Protokoll erstellt:

![Target-API-Konsolenfehler](assets/target-console-error.png)

#### Auflösung

1. Melden Sie sich mit Administratorrechten für das Adobe Target-Produktprofil, außer der AEM-Integration, bei [Admin Console](https://adminconsole.adobe.com/) an.
2. Wählen Sie __Produkte > Adobe Target > Produktprofil__ aus.
3. Wählen Sie auf der Registerkarte __Integrationen__ die Integration für Ihre AEM as a Cloud Service-Umgebung aus (gleicher Name wie das Adobe Developer-Projekt).
4. Weisen Sie die Rolle für __Editor__ oder __Approver__ zu.

   ![Target-API-Fehler](assets/target-permissions.png)

Durch Hinzufügen der korrekten Berechtigung zur Adobe Target-Integration sollte dieser Fehler behoben werden.

## Unterstützende Links

+ [Adobe Experience Cloud Debugger – Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
