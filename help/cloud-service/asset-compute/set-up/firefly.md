---
title: Adobe Project Firefly für Asset compute-Erweiterbarkeit einrichten
description: asset compute-Projekte sind speziell definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrationen, Entwicklung
role: Entwickler
level: Vermittelt, erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---


# Adobe-Projekt einrichten

asset compute-Projekte sind speziell definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.

## Adobe Project Firefly in Adobe Developer Console {#set-up} erstellen und einrichten

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Clickthrough der Einrichtung des Adobe Project Firefly (Kein Audio)_

1. Melden Sie sich bei [Adobe Developer Console](https://console.adobe.io) mit dem Adobe ID an, das den bereitgestellten [Konten und Diensten](./accounts-and-services.md) zugeordnet ist. Vergewissern Sie sich, dass Sie ein __Systemadministrator__ oder __Entwicklerrolle__ für die richtige Adobe-Reihenfolge verwenden.
1. Erstellen Sie ein Firefly-Projekt, indem Sie auf __Neues Projekt erstellen > Projekt aus Vorlage > Projekt Firefly__ tippen

   _Wenn entweder__ Create new __projectButton oder der__ Project __Firefytype nicht verfügbar ist, bedeutet dies, dass Ihre Adobe Org nicht  [mit Project Firefly](#request-adobe-project-firefly) bereitgestellt wird._

   + __Titel__ des Projekts:  `WKND AEM Asset Compute`
   + __App-Name__:  `wkndAemAssetCompute<YourName>`
      + Der __App-Name__ muss für alle Firefly-Projekte eindeutig sein und kann später nicht geändert werden. Das Präzisieren des Namens Ihrer Firma oder Organisation und das Präfix mit einem aussagekräftigen Suffix ist ein guter Ansatz, z. B.: `wkndAemAssetCompute`.
      + Für die Selbstaktivierung ist es oft am besten, Ihren Namen dem __App-Namen__ hinzuzufügen, z. B. `wkndAemAssetComputeJaneDoe`, um Kollisionen mit anderen Project Firefly-Projekten zu vermeiden.
   + Fügen Sie unter __Workspaces__ eine neue Umgebung mit dem Namen `Development` hinzu.
   + Stellen Sie sicher, dass unter __Adobe I/O Runtime__ __Laufzeitumgebung mit jedem Arbeitsbereich__ einschließen ausgewählt ist.
   + Tippen Sie auf __Speichern__, um das Projekt zu speichern
1. Wählen Sie im Projekt &quot;Adobe Firefly&quot;aus der Arbeitsbereichsauswahl die Option `Development`
1. Tippen Sie auf __+ Hinzufügen Service > API__, um den Assistenten __Hinzufügen API__ zu öffnen. Verwenden Sie diesen Ansatz, um die folgenden APIs hinzuzufügen:

   + __Experience Cloud > Asset compute__
      + Wählen Sie __Generieren Sie ein Schlüsselpaar__ und tippen Sie auf die Schaltfläche __Generate keypair__ und speichern Sie die heruntergeladene `config.zip` an einem sicheren Speicherort für [spätere Verwendung](#private-key)
      + Tippen Sie auf __Weiter__
      + Wählen Sie das Profil __Integrationen - Cloud Service__ und tippen Sie auf __Konfigurierte API speichern__
   + __Adobe Services > I/O-__ Ereignisse und tippen Sie auf Konfigurierte API  __speichern__
   + __Adobe Services > I/O-Management-__ APIs und tippen Sie auf Konfigurierte API  __speichern__

## Zugriff auf private.key{#private-key}

Beim Einrichten der [Asset compute-API-Integration](#set-up) wurde ein neues Schlüsselpaar generiert und eine `config.zip`-Datei wurde automatisch heruntergeladen. Dieses `config.zip` enthält das generierte öffentliche Zertifikat und die zugehörige `private.key`-Datei.

1. Dekomprimieren Sie `config.zip` an eine sichere Stelle im Dateisystem, da `private.key` [später ](../develop/environment-variables.md) verwendet wird.
   + Geheimnisse und private Schlüssel sollten niemals aus Sicherheitsgründen zu Git hinzugefügt werden.

## Überprüfen Sie die Berechtigungen für das Dienstkonto (JWT).

Die Anmeldeinformationen dieses Adobe I/O-Projekts werden vom lokalen [Asset compute Development Tool](../develop/development-tool.md) verwendet, um mit Adobe I/O Runtime zu interagieren, und müssen in das Asset compute-Projekt integriert werden. Machen Sie sich mit den JWT-Anmeldeinformationen vertraut.

![Adobe Developer Service-Kontoanmeldeinformationen](./assets/firefly/service-account.png)

1. Vergewissern Sie sich, dass im Projekt &quot;Adobe I/O-Projekt - Firefly&quot;der Arbeitsbereich `Development` ausgewählt ist.
1. Tippen Sie auf __Dienstkonto (JWT)__ unter __Berechtigungen__
1. Überprüfen Sie die angezeigten Adoben I/O-Anmeldeinformationen.
   + Der unten aufgeführte __öffentliche Schlüssel__ hat das Gegenstück __private.key__ im heruntergeladenen `config.zip`, wenn die __Asset compute API__ zu diesem Projekt hinzugefügt wurde.
      + Wenn der private Schlüssel verloren geht oder beschädigt wird, kann der entsprechende öffentliche Schlüssel entfernt und ein neues Schlüsselpaar generiert oder mit dieser Schnittstelle in die Adobe I/O hochgeladen werden.
