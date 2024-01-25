---
title: Einrichten der App-Entwicklung zur Asset Compute-Erweiterung
description: Asset Compute-Projekte sind speziell definierte App Builder-Projekte und erfordern daher Zugriff auf App Builder in Adobe Developer Console, um sie einzurichten und bereitzustellen.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 216
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 100%

---

# Einrichten von App Builder

Asset Compute-Projekte sind speziell definierte App-Entwicklungs-Projekte und erfordern daher Zugriff auf die App-Entwicklung in Adobe Developer Console, um sie einzurichten und bereitzustellen.

## Erstellen und Einrichten der App-Entwicklung in Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Clickthrough beim Einrichten der App-Entwicklung (kein Audio)_

1. Melden Sie sich bei [Adobe Developer Console](https://console.adobe.io) über die Adobe ID an, die mit den bereitgestellten [Konten und Diensten](./accounts-and-services.md) verknüpft ist. Stellen Sie sicher, dass Sie über eine __Systemadmin__- oder __Entwicklerrolle__ für die richtige Adobe-Organisation verfügen.
1. Erstellen Sie ein App-Entwicklungsprojekt, indem Sie __Neues Projekt erstellen > Projekt aus Vorlage > App-Entwicklung__ auswählen.

   _Wenn die Schaltfläche__ Neues Projekt erstellen __oder der Typ__ App-Entwicklung __nicht verfügbar ist, bedeutet dies, dass die [App-Entwicklung nicht für Ihre Adobe-Organisation bereitgestellt wurde](#request-adobe-project-app-builder)._

   + __Projekttitel__: `WKND AEM Asset Compute`
   + __App-Name__: `wkndAemAssetCompute<YourName>`
      + Der __App-Name__ muss für alle App-Entwicklungsprojekte eindeutig sein und kann später nicht geändert werden. Es ist ein guter Ansatz, den Namen Ihres Unternehmens oder Ihrer Organisation als Präfix zu verwenden und ein aussagekräftige Suffix anzufügen (Beispiel: `wkndAemAssetCompute`).
      + Um es sich selbst einfacher zu machen, sollten Sie am besten Ihren eigenen Namen an den __App-Namen__ anhängen, z. B. `wkndAemAssetComputeJaneDoe`. So vermeiden Sie Konflikte mit anderen App-Entwicklungsprojekten.
   + Fügen Sie unter __Arbeitsbereiche__ eine neue Umgebung mit dem Namen `Development` hinzu.
   + Stellen Sie unter __Adobe I/O Runtime__ sicher, dass __Runtime bei jedem Arbeitsbereich einbeziehen__ ausgewählt ist.
   + Klicken Sie auf __Speichern__, um das Projekt zu speichern
1. Wählen Sie im App-Entwicklungsprojekt über die Arbeitsbereichsauswahl `Development` aus.
1. Klicken Sie auf __+ Dienst hinzufügen > API__, um den Assistenten __API hinzufügen__ zu öffnen. Verwenden Sie diesen Ansatz, um die folgenden APIs hinzuzufügen:

   + __Experience Cloud > Asset Compute__
      + Wählen Sie __Schlüsselpaar generieren__ aus, klicken Sie auf die Schaltfläche __Schlüsselpaar generieren__ und speichern Sie die heruntergeladene Datei `config.zip` zur [späteren Verwendung](#private-key) an einem sicheren Ort.
      + Tippen Sie auf __Weiter__
      + Wählen Sie das Produktprofil __Integrationen – Cloud Service__ aus und klicken Sie auf __Konfigurierte API speichern__.
   + Wählen Sie __Adobe Services > I/O-Ereignisse__ aus und klicken Sie auf __Konfigurierte API speichern__.
   + Wählen Sie __Adobe Services > I/O-Management API__ aus und klicken Sie auf __Konfigurierte API speichern__.

## Zugreifen auf private.key{#private-key}

Beim Einrichten der [Asset Compute-API-Integration](#set-up) wurde ein neues Schlüsselpaar generiert und die Datei `config.zip` automatisch heruntergeladen. Die Datei `config.zip` enthält das generierte öffentliche Zertifikat und die entsprechende Datei `private.key`.

1. Entpacken Sie `config.zip` an einem sicheren Ort in Ihrem Dateisystem, da `private.key` [später verwendet](../develop/environment-variables.md) wird.
   + Geheime und private Schlüssel sollten aus Sicherheitsgründen niemals zu Git hinzugefügt werden.

## Überprüfen der Anmeldeinformationen für das Dienstkonto (JWT)

Die Anmeldeinformationen für dieses Adobe I/O-Projekt werden vom lokalen [Asset Compute-Entwicklungs-Tool](../develop/development-tool.md) verwendet, um mit der Adobe I/O Runtime zu interagieren. Sie müssen daher in das Asset Compute-Projekt integriert werden. Machen Sie sich mit den Anmeldeinformationen für das Dienstkonto (JWT) vertraut.

![Anmeldeinformationen für das Adobe Developer-Dienstkonto](./assets/app-builder/service-account.png)

1. Stellen Sie im App-Entwicklungsprojekt des Adobe I/O-Projekts sicher, dass der Arbeitsbereich `Development` ausgewählt ist.
1. Klicken Sie unter __Anmeldeinformationen__ auf die Option __Dienstkonto (JWT)__.
1. Überprüfen Sie die angezeigten Adobe I/O-Anmeldeinformationen.
   + Für den unten aufgelisteten __öffentlichen Schlüssel__ ist das __private.key__-Gegenstück in der heruntergeladenen Datei `config.zip` vorhanden, wenn die __Asset Compute-API__ diesem Projekt hinzugefügt wurde.
      + Bei Verlust oder Kompromittierung des privaten Schlüssels kann über diese Benutzeroberfläche der entsprechende öffentliche Schlüssel entfernt und ein neues Schlüsselpaar in Adobe I/O generiert oder nach Adobe I/O hochgeladen werden.
