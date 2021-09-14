---
title: Einrichten von Adobe Project Firefly für Asset compute-Erweiterbarkeit
description: asset compute-Projekte sind speziell definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---

# Einrichten von Adobe Project Firefly

asset compute-Projekte sind speziell definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.

## Erstellen und Einrichten von Adobe Project Firefly in der Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Clickthrough zur Einrichtung von Adobe Project Firefly (kein Audio)_

1. Melden Sie sich bei [Adobe Developer Console](https://console.adobe.io) an, indem Sie die Adobe ID verwenden, die mit den bereitgestellten [Konten und Services](./accounts-and-services.md) verknüpft ist. Stellen Sie sicher, dass Sie ein __Systemadministrator__ oder in der __Entwicklerrolle__ sind, um die richtige Adobe zu finden.
1. Erstellen Sie ein Firefly-Projekt, indem Sie auf __Neues Projekt erstellen > Projekt aus Vorlage > Projekt Firefly__ tippen.

   _Wenn die Schaltfläche__ Neues Projekt erstellen __oder der__ Projektarchiv __nicht verfügbar ist, bedeutet dies, dass Ihre Adobe-Organisation nicht  [mit Project Firefly](#request-adobe-project-firefly) bereitgestellt wird._

   + __Projekttitel__:  `WKND AEM Asset Compute`
   + __App name__:  `wkndAemAssetCompute<YourName>`
      + Der __App-Name__ muss für alle Firefly-Projekte eindeutig sein und kann später nicht mehr geändert werden. Das Präfix des Namens Ihres Unternehmens oder Ihrer Organisation und das Postfixieren mit einem aussagekräftigen Suffix ist ein guter Ansatz, z. B.: `wkndAemAssetCompute`.
      + Für die Selbstaktivierung ist es oft am besten, Ihren Namen dem __App-Namen__ hinzuzufügen, z. B. `wkndAemAssetComputeJaneDoe`, um Konflikte mit anderen Projekt-Firefly-Projekten zu vermeiden.
   + Fügen Sie unter __Workspaces__ eine neue Umgebung mit dem Namen `Development` hinzu.
   + Stellen Sie unter __Adobe I/O Runtime__ sicher, dass __Laufzeiteinschluss für jeden Arbeitsbereich__ ausgewählt ist.
   + Tippen Sie auf __Speichern__, um das Projekt zu speichern.
1. Wählen Sie im Adobe Firefly-Projekt `Development` aus der Workspace-Auswahl aus.
1. Tippen Sie auf __+ Service hinzufügen > API__ , um den Assistenten __API__ hinzufügen zu öffnen. Verwenden Sie diesen Ansatz, um die folgenden APIs hinzuzufügen:

   + __Experience Cloud > Asset compute__
      + Wählen Sie __Generieren Sie ein Schlüsselpaar__ und tippen Sie auf die Schaltfläche __Generate keypair__ und speichern Sie die heruntergeladene `config.zip` an einem sicheren Speicherort für [spätere Verwendung](#private-key).
      + Tippen Sie auf __Weiter__
      + Wählen Sie das Produktprofil __Integrationen - Cloud Service__ aus und tippen Sie auf __Konfigurierte API speichern__
   + __Adobe Services > I/O-__ Ereignisse und tippen Sie auf Konfigurierte API  __speichern__
   + __Adobe Services > I/O Management-__ APIs und tippen Sie auf Konfigurierte API  __speichern__

## Zugriff auf private.key{#private-key}

Beim Einrichten der [Asset compute-API-Integration](#set-up) wurde ein neues Schlüsselpaar generiert und eine `config.zip`-Datei automatisch heruntergeladen. Dieses `config.zip` enthält das generierte öffentliche Zertifikat und die zugehörige `private.key`-Datei.

1. Entpacken Sie `config.zip` an eine sichere Stelle in Ihrem Dateisystem, da `private.key` [später verwendet wird](../develop/environment-variables.md)
   + Geheimnisse und private Schlüssel sollten niemals aus Sicherheitsgründen zu Git hinzugefügt werden.

## Überprüfen Sie die Anmeldedaten für das Dienstkonto (JWT).

Die Anmeldeinformationen dieses Adobe I/O-Projekts werden vom lokalen [Asset compute Development Tool](../develop/development-tool.md) verwendet, um mit Adobe I/O Runtime zu interagieren, und müssen in das Asset compute-Projekt integriert werden. Machen Sie sich mit den Anmeldedaten für das Dienstkonto (JWT) vertraut.

![Kontoanmeldeinformationen für Adobe Developer Service](./assets/firefly/service-account.png)

1. Stellen Sie im Projekt Adobe I/O Project Firefly sicher, dass der Arbeitsbereich `Development` ausgewählt ist.
1. Tippen Sie auf __Dienstkonto (JWT)__ unter __Anmeldedaten__
1. Überprüfen der angezeigten Adobe I/O-Anmeldedaten
   + Der unten aufgelistete __öffentliche Schlüssel__ hat den __private.key__-Gegenstück im `config.zip`, der heruntergeladen wurde, als die __Asset compute-API__ zu diesem Projekt hinzugefügt wurde.
      + Wenn der private Schlüssel verloren geht oder kompromittiert ist, kann der entsprechende öffentliche Schlüssel entfernt und ein neues Schlüsselpaar generiert oder über diese Benutzeroberfläche in Adobe I/O hochgeladen werden.
