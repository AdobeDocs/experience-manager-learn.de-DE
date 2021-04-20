---
title: Zugriff auf AEM als Cloud Service konfigurieren
description: 'AEM als Cloud Service ist die Cloud-native Methode zur Nutzung der AEM-Anwendungen und nutzt daher die Adobe IMS (Identity Management System), um die Anmeldung von Benutzern, sowohl Administratoren als auch regulären Benutzern, beim AEM Author-Dienst zu erleichtern. Erfahren Sie, wie Adobe-IMS-Benutzer, -Benutzergruppen und -Produkt-Profil zusammen mit AEM Gruppen und Berechtigungen für den Zugriff auf AEM Author verwendet werden.  '
feature: Users and Groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: Administration, Security
role: Administrator
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 5%

---


# Zugriff auf AEM als Cloud Service konfigurieren

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Einführung in die Adobe IMS"
>abstract="AEM als Cloud Service nutzt die Adobe IMS (Identity Management System), um die Anmeldung von Benutzern, sowohl Administratoren als auch normale Benutzer, beim AEM Author-Dienst zu erleichtern. Erfahren Sie, wie Adobe-IMS-Profil, -Gruppen und -Produktgruppen gemeinsam mit AEM und Berechtigungen für einen detaillierten Zugriff auf den AEM Author-Dienst verwendet werden."

AEM als Cloud Service ist die Cloud-native Methode zur Nutzung der AEM-Anwendungen und nutzt daher die Adobe IMS (Identity Management System), um die Anmeldung der Benutzer, sowohl der Administratoren als auch der normalen Benutzer, beim AEM Author-Dienst zu erleichtern. Erfahren Sie, wie Adobe-IMS-Profil, -Gruppen und -Produktgruppen gemeinsam mit AEM und Berechtigungen für einen detaillierten Zugriff auf den AEM Author-Dienst verwendet werden.

## Adobe IMS-Benutzer

Benutzer, die Zugriff auf den AEM Author-Dienst benötigen, werden als [Adobe IMS-Benutzer](https://helpx.adobe.com/de/enterprise/using/set-up-identity.html) in [Adobe AdminConsole](https://adminconsole.adobe.com) verwaltet. Erfahren Sie, welche Adobe IMS-Benutzer sind und wie sie in der Admin Console aufgerufen und verwaltet werden.

[Informationen zu Adobe IMS-Benutzern](./adobe-ims-users.md)

## Adobe IMS-Benutzergruppen

Benutzer, die auf den AEM Author-Dienst zugreifen, sollten mithilfe von [Adobe IMS-Benutzergruppen](https://helpx.adobe.com/enterprise/using/user-groups.html) in [Adobe AdminConsole](https://adminconsole.adobe.com) in logische Gruppen unterteilt werden. Adobe IMS-Benutzergruppen bieten keine direkten Berechtigungen oder Zugriff auf AEM (dies ist die Aufgabe von [Adobe IMS-Produktgruppen](#adobe-ims-product-profiles)). Sie sind jedoch eine hervorragende Möglichkeit, logische Benutzergruppen zu definieren, die wiederum mithilfe von AEM und Berechtigungen in bestimmte Zugriffsebenen des AEM Author-Dienstes übersetzt werden können.

[Informationen zu Adobe IMS-Benutzergruppen](./adobe-ims-user-groups.md)

## Adobe IMS-Profil

[Adobe-IMS-Profil](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), die in der AdminConsole [ der ](https://adminconsole.adobe.com)Adobe verwaltet werden, sind der Mechanismus, der  [Adobe-IMS-](#adobe-ims-users) Benutzern Zugriff auf die Anmeldung beim AEM Author-Dienst mit Basiszugriff bietet.

+ Das __AEM Users__-Profil ermöglicht Benutzern schreibgeschützten Zugriff auf AEM über die Mitgliedschaft in AEM Beitragsgruppe.
+ Das Produkt-Profil __AEM Administratoren__ ermöglicht Benutzern den vollständigen, administrativen Zugriff auf AEM.

[Informationen zu Adobe IMS-Profilen](./adobe-ims-product-profiles.md)

## AEM Benutzergruppen und Berechtigungen

Adobe Experience Manager baut auf Adobe-IMS-Benutzern, Benutzergruppen und Produkt-Profilen auf, um Benutzern benutzerdefinierbaren Zugriff auf AEM zu ermöglichen. Erfahren Sie, wie Sie AEM Gruppen und Berechtigungen zusammenstellen und wie sie mit Adobe IMS-Abstraktionen zusammenarbeiten, um einen nahtlosen und anpassbaren Zugriff auf AEM zu ermöglichen.

[Informationen zu AEM Benutzern, Gruppen und Berechtigungen](./aem-users-groups-and-permissions.md)

## Durchlaufen von Zugriff und Berechtigungen

Eine Kurzanleitung zum Konfigurieren von Adobe IMS-Benutzern, -Benutzergruppen und -Produkt-Profilen in der AdminConsole und zum Verwenden dieser Adobe-IMS-Abstraktionen in AEM Author, um spezifische gruppenbasierte Berechtigungen zu definieren und zu verwalten.

[AEM und Zugriffsberechtigungen](./walk-through.md)

## Zusätzliche Adobe Admin Console-Ressourcen

Die folgende Dokumentation behandelt [Adobe Admin Console](https://adminconsole.adobe.com)-spezifische Details und Bedenken, die zu einem besseren Verständnis des Adobe Admin Console und dessen Verwendung bei der Benutzerverwaltung und dem Zugriff über verschiedene Experience Cloud-Produkte hinweg beitragen können.

+ [Adobe Admin Console-Identitätsübersicht](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console Admin-Rollen](https://helpx.adobe.com/de/enterprise/using/admin-roles.html)
+ [Adobe Admin Console-Rollen für Entwickler](https://helpx.adobe.com/de/enterprise/using/manage-developers.html)