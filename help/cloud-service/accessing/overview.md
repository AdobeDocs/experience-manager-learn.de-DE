---
title: Konfigurieren des Zugriffs auf AEM as a Cloud Service
description: 'AEM as a Cloud Service ist die Cloud-native Möglichkeit, die AEM-Anwendungen zu nutzen, und nutzt daher die Adobe IMS (Identity Management System), um die Anmeldung von Benutzern, sowohl Administratoren als auch reguläre Benutzer, beim AEM-Autorendienst zu erleichtern. Erfahren Sie, wie Adobe IMS-Benutzer, Benutzergruppen und Produktprofile gemeinsam mit AEM und Berechtigungen verwendet werden, um bestimmten Zugriff auf AEM Author zu gewähren.  '
feature: 'Benutzer und Gruppen '
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: Administration, Sicherheit
role: Admin
level: Beginner
source-git-commit: 36346a8a45fc20b5e6d71d6d00345d74b6b04c2a
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 7%

---


# Konfigurieren des Zugriffs auf AEM as a Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Einführung in Adobe IMS"
>abstract="AEM as a Cloud Service nutzt die Adobe IMS (Identity Management System), um die Anmeldung seiner Benutzer, sowohl Administratoren als auch reguläre Benutzer, beim AEM-Autorendienst zu erleichtern. Erfahren Sie, wie Adobe IMS-Benutzer, -Gruppen und -Produktprofile gemeinsam mit AEM und -Berechtigungen verwendet werden, um einen differenzierten Zugriff auf den AEM-Autorendienst zu ermöglichen."

AEM as a Cloud Service ist die Cloud-native Möglichkeit, die AEM-Anwendungen zu nutzen, und nutzt daher die Adobe IMS (Identity Management System), um die Anmeldung von Benutzern, sowohl Administratoren als auch reguläre Benutzer, beim AEM-Autorendienst zu erleichtern.

![Adobe Admin Console](./assets/hero.png)

Erfahren Sie, wie Adobe IMS-Benutzer, -Gruppen und -Produktprofile gemeinsam mit AEM und -Berechtigungen verwendet werden, um einen differenzierten Zugriff auf den AEM-Autorendienst zu ermöglichen.

## Adobe IMS-Benutzer

Benutzer, die Zugriff auf den AEM-Autorendienst benötigen, werden als [Adobe IMS-Benutzer](https://helpx.adobe.com/de/enterprise/using/set-up-identity.html) in der AdminConsole der Adobe](https://adminconsole.adobe.com) verwaltet. [ Erfahren Sie, welche Adobe IMS-Benutzer sind und wie sie in Admin Console aufgerufen und verwaltet werden.

[Erfahren Sie mehr über Adobe IMS-Benutzer](./adobe-ims-users.md)

## Adobe IMS-Benutzergruppen

Benutzer, die auf den AEM-Autorendienst zugreifen, sollten mithilfe von [Adobe IMS-Benutzergruppen](https://helpx.adobe.com/enterprise/using/user-groups.html) in [AdminConsole der Adobe](https://adminconsole.adobe.com) in logische Gruppen unterteilt werden. Adobe IMS-Benutzergruppen bieten keine direkten Berechtigungen oder Zugriff auf AEM (dies ist der Auftrag von [Adobe IMS-Produktprofilen](#adobe-ims-product-profiles)). Sie eignen sich jedoch hervorragend, um logische Benutzergruppen zu definieren, die wiederum mithilfe AEM Gruppen und Berechtigungen in bestimmte Zugriffsebenen des AEM-Autorendienstes übersetzt werden können.

[Informationen zu Adobe IMS-Benutzergruppen](./adobe-ims-user-groups.md)

## Adobe IMS-Produktprofile

[Adobe IMS-Produktprofile](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), die in der  [Adobe-AdminConsole](https://adminconsole.adobe.com) verwaltet werden, sind der Mechanismus, der  [Adobe-IMS-](#adobe-ims-users) Benutzern Zugriff auf die Anmeldung beim AEM-Autorendienst mit Basiszugriff bietet.

+ Das Produktprofil __AEM Benutzer__ bietet Benutzern Lesezugriff auf AEM über die Mitgliedschaft in AEM Gruppe Mitwirkende .
+ Das Produktprofil __AEM Administratoren__ bietet Benutzern vollständigen, administrativen Zugriff auf AEM.

[Informationen zu Adobe IMS-Produktprofilen](./adobe-ims-product-profiles.md)

## AEM Benutzergruppen und Berechtigungen

Adobe Experience Manager baut auf Adobe-IMS-Benutzern, Benutzergruppen und Produktprofilen auf, um Benutzern benutzerdefinierten Zugriff auf AEM zu ermöglichen. Erfahren Sie, wie Sie AEM Gruppen und Berechtigungen erstellen und wie diese in Abstimmung mit Adobe IMS-Abstraktionen funktionieren, um nahtlosen und anpassbaren Zugriff auf AEM zu ermöglichen.

[Erfahren Sie mehr über AEM Benutzer, Gruppen und Berechtigungen](./aem-users-groups-and-permissions.md)

## schrittweise Einführung in Zugriff und Berechtigungen

Eine kurze Anleitung zum Konfigurieren von Adobe IMS-Benutzern, Benutzergruppen und Produktprofilen in Adobe Admin Console und dazu, wie Sie diese Adobe IMS-Abstraktionen in der AEM-Autoreninstanz nutzen können, um spezifische gruppenbasierte Berechtigungen zu definieren und zu verwalten.

[AEM und Berechtigungen - schrittweise](./walk-through.md)

## Zusätzliche Adobe Admin Console-Ressourcen

Die folgende Dokumentation behandelt [Adobe Admin Console](https://adminconsole.adobe.com)-spezifische Details und Bedenken, die zu einem besseren Verständnis der Adobe Admin Console und zur Verwendung dieser Funktion für die Verwaltung von Benutzern und den Zugriff über Experience Cloud-Produkte hinweg beitragen können.

+ [Adobe Admin Console Identity - Übersicht](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console-Administratorrollen](https://helpx.adobe.com/de/enterprise/using/admin-roles.html)
+ [Adobe Admin Console-Entwicklerrollen](https://helpx.adobe.com/de/enterprise/using/manage-developers.html)