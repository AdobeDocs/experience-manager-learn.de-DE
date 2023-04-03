---
title: App Builder für Asset compute-Erweiterbarkeit einrichten
description: asset compute-Projekte sind speziell definierte App Builder-Projekte und erfordern daher Zugriff auf App Builder in der Adobe Developer-Konsole, um sie einzurichten und bereitzustellen.
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# App Builder einrichten

asset compute-Projekte sind speziell definierte App Builder-Projekte und erfordern daher Zugriff auf App Builder in der Adobe Developer-Konsole, um sie einzurichten und bereitzustellen.

## App Builder in der Adobe Developer Console erstellen und einrichten{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Clickthrough der Einrichtung von App Builder (kein Audio)_

1. Anmelden bei [Adobe Developer-Konsole](https://console.adobe.io) mithilfe der Adobe ID, die mit dem bereitgestellten verknüpft ist [Konten und Dienstleistungen](./accounts-and-services.md). Stellen Sie sicher, dass __Systemadministrator__ oder in __Entwicklerrolle__ für die richtige Adobe Org.
1. Erstellen eines App Builder-Projekts durch Tippen auf __Neues Projekt erstellen > Projekt aus Vorlage > App Builder__

   _Wenn__ Neues Projekt erstellen __oder__ App Builder __Der Typ ist nicht verfügbar. Das bedeutet, dass Ihre Adobe-Org nicht verfügbar ist. [mit App Builder bereitgestellt wurde](#request-adobe-project-app-builder)._

   + __Projekttitel__: `WKND AEM Asset Compute`
   + __App-Name__: `wkndAemAssetCompute<YourName>`
      + Die __App-Name__ muss für alle FApp Builderirefly-Projekte eindeutig sein und kann später nicht geändert werden. Das Präfix des Namens Ihres Unternehmens oder Ihrer Organisation und das Postfixieren mit einem aussagekräftigen Suffix ist ein guter Ansatz, z. B.: `wkndAemAssetCompute`.
      + Für die Selbstaktivierung ist es oft am besten, Ihren Namen dem __App-Name__, z. B. `wkndAemAssetComputeJaneDoe` um Kollisionen mit anderen App Builder-Projekten zu vermeiden.
   + under __Arbeitsbereiche__ eine neue Umgebung mit dem Namen `Development`
   + under __Adobe I/O Runtime__ sicherstellen __Laufzeitumgebung in jeden Arbeitsbereich einbeziehen__ ausgewählt ist
   + Tippen __Speichern__ zum Speichern des Projekts
1. Wählen Sie im App Builder-Projekt `Development` über die Workspace-Auswahl
1. Tippen __+ Dienst hinzufügen > API__ , um __API hinzufügen__ -Assistenten verwenden Sie diesen Ansatz, um die folgenden APIs hinzuzufügen:

   + __Experience Cloud > Asset compute__
      + Auswählen __Generieren eines Schlüsselpaars__ und tippen Sie auf __Generieren von keypair__ und speichern Sie die heruntergeladene `config.zip` an einen sicheren Ort für [spätere Verwendung](#private-key)
      + Tippen Sie auf __Weiter__
      + Produktprofil auswählen __Integrationen - Cloud Service__ und tippen __Konfigurierte API speichern__
   + __Adobe Services > I/O-Ereignisse__ und tippen __Konfigurierte API speichern__
   + __Adobe Services > I/O-Management-API__ und tippen __Konfigurierte API speichern__

## Zugriff auf private.key{#private-key}

Beim Einrichten der [asset compute-API-Integration](#set-up) ein neues Schlüsselpaar generiert und ein `config.zip` -Datei automatisch heruntergeladen wurde. Diese `config.zip` enthält das generierte öffentliche Zertifikat und stimmt `private.key` -Datei.

1. Entpacken `config.zip` auf eine sichere Stelle in Ihrem Dateisystem als `private.key` is [verwendet später](../develop/environment-variables.md)
   + Geheimnisse und private Schlüssel sollten niemals aus Sicherheitsgründen zu Git hinzugefügt werden.

## Überprüfen Sie die Anmeldedaten für das Dienstkonto (JWT).

Die Anmeldeinformationen dieses Adobe I/O-Projekts werden von der lokalen [asset compute-Entwicklungstool](../develop/development-tool.md) , um mit Adobe I/O Runtime zu interagieren, und muss in das Asset compute-Projekt integriert werden. Machen Sie sich mit den Anmeldedaten für das Dienstkonto (JWT) vertraut.

![Kontoanmeldeinformationen für Adobe Developer Service](./assets/app-builder/service-account.png)

1. Stellen Sie im Adobe I/O Project App Builder -Projekt sicher, dass die `Development` Arbeitsbereich ist ausgewählt
1. Tippen Sie auf __Dienstkonto (JWT)__ under __Anmeldeinformationen__
1. Überprüfen der angezeigten Adobe I/O-Anmeldedaten
   + Die __öffentlicher Schlüssel__ unten aufgelistet ist, hat es __private.key__ -Gegenstück in der `config.zip` heruntergeladen wurde, wenn __asset compute-API__ wurde diesem Projekt hinzugefügt.
      + Wenn der private Schlüssel verloren geht oder kompromittiert ist, kann der entsprechende öffentliche Schlüssel entfernt und ein neues Schlüsselpaar generiert oder über diese Benutzeroberfläche in Adobe I/O hochgeladen werden.
