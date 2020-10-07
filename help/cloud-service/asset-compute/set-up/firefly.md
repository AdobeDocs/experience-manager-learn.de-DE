---
title: Adobe Project Firefly für Asset Compute-Erweiterbarkeit einrichten
description: Asset Compute-Projekte sind eigens definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Adobe-Projekt einrichten

Asset Compute-Projekte sind eigens definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.

## Adobe Project Firefly in der Adobe Developer Console erstellen und einrichten{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)
_Clickthrough der Einrichtung des Adobe Project Firefly (Kein Audio)_

1. Melden Sie sich bei der [Adobe Developer Console](https://console.adobe.io) mit dem Adobe ID an, das mit den bereitgestellten [Konten und Diensten](./accounts-and-services.md)verknüpft ist. Stellen Sie sicher, dass Sie für die richtige Adobe ein __Systemadministrator__ oder in der __Entwicklerrolle__ sind.
1. Erstellen Sie ein Firefly-Projekt, indem Sie auf Neues Projekt __erstellen > Projekt aus Vorlage > Projekt Firefly tippen__

   _Wenn entweder die Schaltfläche Neues Projekt____erstellen oder der__ Projektarchiv __nicht verfügbar ist, bedeutet dies, dass Ihr Adobe-Org nicht mit Project Firefly[bereitgestellt](#request-adobe-project-firefly)wird._

   + __Titel__ des Projekts: `WKND AEM Asset Compute`
   + __App-Name__: `wkndAemAssetCompute<YourName>`
      + Der __App-Name__ muss für alle Firefly-Projekte eindeutig sein und kann später nicht geändert werden. Das Präzisieren des Namens Ihrer Firma oder Organisation und das Präfix mit einem aussagekräftigen Suffix ist ein guter Ansatz, z. B.: `wkndAemAssetCompute`.
      + Für die Selbstaktivierung ist es oft am besten, Ihren Namen an den __App-Namen__ anzuhängen, z. B. um Kollisionen mit anderen Projekt-Firefly-Projekten `wkndAemAssetComputeJaneDoe` zu vermeiden.
   + Fügen Sie unter __Arbeitsflächen__ eine neue Umgebung mit dem Namen `Development`
   + Stellen Sie sicher, dass unter __Adobe I/O Runtime__ die Option &quot; __Laufzeitumgebung mit jedem Arbeitsbereich__ einschließen&quot;ausgewählt ist.
   + Tap __Save__ to save the project
1. Wählen Sie im Projekt Adobe Firefly `Development` aus der Arbeitsflächenauswahl
1. Tippen Sie auf __+ Hinzufügen Dienst > API__ , um den __Hinzufügen API__ -Assistenten zu öffnen. Verwenden Sie diese Methode, um die folgenden APIs hinzuzufügen:

   + __Experience Cloud > Asset-Berechnung__
      + Wählen Sie __Generate a key pair__ , tippen Sie auf die Schaltfläche __Generate keypair__ , und speichern Sie die heruntergeladene `config.zip` Datei für die [spätere Verwendung an einem sicheren Speicherort](#private-key)
      + Tippen Sie auf __Weiter__
      + Wählen Sie Product Profil __Integrations - Cloud Service__ und tippen Sie auf Konfigurierte API __speichern__
   + __Adobe Services > I/O-Ereignis__ und tippen Sie auf Konfigurierte API __speichern__
   + __Adobe Services > I/O-Management-API__ und tippen Sie auf Konfigurierte API __speichern__

## Zugriff auf private.key{#private-key}

Beim Einrichten der [Asset Compute API-Integration](#set-up) wurde ein neues Schlüsselpaar generiert und eine `config.zip` Datei automatisch heruntergeladen. Dies `config.zip` enthält das generierte öffentliche Zertifikat und die zugehörige `private.key` Datei.

1. Entpacken Sie `config.zip` die ZIP-Datei an eine sichere Stelle in Ihrem Dateisystem, da die `private.key` später [verwendet wird.](../develop/environment-variables.md)
   + Geheimnisse und private Schlüssel sollten niemals aus Sicherheitsgründen zu Git hinzugefügt werden.

## Überprüfen Sie die Berechtigungen für das Dienstkonto (JWT).

Die Anmeldeinformationen dieses Adoben-E/A-Projekts werden vom lokalen [Asset Compute Development Tool](../develop/development-tool.md) für die Interaktion mit Adobe I/O Runtime verwendet und müssen in das Asset Compute-Projekt integriert werden. Machen Sie sich mit den JWT-Anmeldeinformationen vertraut.

![Adobe Developer Service-Kontoanmeldeinformationen](./assets/firefly/service-account.png)

1. Vergewissern Sie sich, dass der `Development` Arbeitsbereich im Projekt &quot;I/O-Projekt - Firefly&quot;ausgewählt ist.
1. Tippen Sie auf __Dienstkonto (JWT)__ unter __Anmeldeinformationen__
1. Überprüfen Sie die angezeigten E/A-Anmeldeinformationen der Adobe.
   + Der unten aufgelistete __öffentliche Schlüssel__ hat das __private.key__ -Gegenstück im `config.zip` heruntergeladenen Dokument, wenn diesem Projekt die __Asset Compute-API__ hinzugefügt wurde.
      + Wenn der private Schlüssel verloren geht oder beschädigt wird, kann der entsprechende öffentliche Schlüssel entfernt und ein neues Schlüsselpaar generiert oder über diese Schnittstelle in die Adobe-E/A hochgeladen werden.
