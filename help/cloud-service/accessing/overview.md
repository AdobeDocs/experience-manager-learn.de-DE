---
title: Zugriff auf AEM als Cloud Service konfigurieren
description: 'AEM als Cloud Service ist die Cloud-native Methode zur Nutzung der AEM-Anwendungen und nutzt daher die Adobe IMS (Identity Management System), um die Anmeldung von Benutzern, sowohl Administratoren als auch regulären Benutzern, beim AEM Author-Dienst zu erleichtern. Erfahren Sie, wie Adobe-IMS-Benutzer, -Benutzergruppen und -Produkt-Profil zusammen mit AEM Gruppen und Berechtigungen für den Zugriff auf AEM Author verwendet werden.  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 5%

---


# Zugriff auf AEM als Cloud Service konfigurieren

AEM als Cloud Service ist die Cloud-native Methode zur Nutzung der AEM-Anwendungen und nutzt daher die Adobe IMS (Identity Management System), um die Anmeldung der Benutzer, sowohl der Administratoren als auch der normalen Benutzer, beim AEM Author-Dienst zu erleichtern. Erfahren Sie, wie Adobe-IMS-Profil, -Gruppen und -Produktgruppen gemeinsam mit AEM und Berechtigungen für einen detaillierten Zugriff auf den AEM Author-Dienst verwendet werden.

## Adobe IMS-Benutzer

Benutzer, die Zugriff auf den AEM Author-Dienst benötigen, werden als IMS- [Adoben](https://helpx.adobe.com/de/enterprise/using/set-up-identity.html) in der AdminConsole [der](https://adminconsole.adobe.com)Adobe verwaltet. Erfahren Sie, welche Adobe IMS-Benutzer sind und wie sie in der Admin Console aufgerufen und verwaltet werden.

[Informationen zu Adobe IMS-Benutzern](./adobe-ims-users.md)

## Adobe IMS-Benutzergruppen

Benutzer, die auf den AEM Author-Dienst zugreifen, sollten mithilfe der [Adobe IMS-Benutzergruppen](https://helpx.adobe.com/enterprise/using/user-groups.html) in der AdminConsole [der](https://adminconsole.adobe.com)Adobe in logische Gruppen organisiert werden. Adobe IMS-Benutzergruppen bieten keine direkten Berechtigungen oder Zugriff auf AEM (dies ist die Aufgabe von [Adobe IMS-Produktgruppen](#adobe-ims-product-profiles)). Sie bieten jedoch eine hervorragende Möglichkeit, logische Benutzergruppen zu definieren, die wiederum mithilfe von AEM und Berechtigungen in bestimmte Zugriffsebenen des AEM Author-Dienstes übersetzt werden können.

[Informationen zu Adobe IMS-Benutzergruppen](./adobe-ims-user-groups.md)

## Adobe IMS-Profil

[Adobe-IMS-Profil](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), die in der AdminConsole [der](https://adminconsole.adobe.com)Adobe verwaltet werden, sind der Mechanismus, der [Adobe-IMS-Benutzern](#adobe-ims-users) Zugriff auf die Anmeldung beim AEM Author-Dienst mit Basiszugriff bietet.

+ Mit dem __AEM User__ -Profil erhalten Benutzer über die Mitgliedschaft in AEM Beitragsgruppe Lesezugriff auf AEM.
+ Das __AEM Profil Administratoren__ ermöglicht Benutzern den vollen, administrativen Zugriff auf AEM.

[Informationen zu Adobe IMS-Profilen](./adobe-ims-product-profiles.md)

## AEM Benutzergruppen und Berechtigungen

Adobe Experience Manager baut auf Adobe-IMS-Benutzern, Benutzergruppen und Produkt-Profilen auf, um Benutzern benutzerdefinierbaren Zugriff auf AEM zu ermöglichen. Erfahren Sie, wie Sie AEM Gruppen und Berechtigungen zusammenstellen und wie sie mit Adobe IMS-Abstraktionen zusammenarbeiten, um einen nahtlosen und anpassbaren Zugriff auf AEM zu ermöglichen.

[Informationen zu AEM Benutzern, Gruppen und Berechtigungen](./aem-users-groups-and-permissions.md)

## Durchlaufen von Zugriff und Berechtigungen

Eine Kurzanleitung zum Konfigurieren von Adobe IMS-Benutzern, -Benutzergruppen und -Produkt-Profilen in der AdminConsole und zum Verwenden dieser Adobe-IMS-Abstraktionen in AEM Author, um spezifische gruppenbasierte Berechtigungen zu definieren und zu verwalten.

[AEM und Zugriffsberechtigungen](./walk-through.md)

## Zusätzliche Adobe Admin Console-Ressourcen

Die folgende Dokumentation behandelt [Adobe Admin Console](https://adminconsole.adobe.com)-spezifische Details und Bedenken, die zu einem besseren Verständnis des Adobe Admin Console und seiner Verwendung für die Benutzerverwaltung und den Zugriff über verschiedene Experience Cloud-Produkte hinweg beitragen können.

+ [Adobe Admin Console-Identitätsübersicht](https://helpx.adobe.com/de/enterprise/using/identity.html)
+ [Adobe Admin Console Admin-Rollen](https://helpx.adobe.com/de/enterprise/using/admin-roles.html)
+ [Adobe Admin Console-Rollen für Entwickler](https://helpx.adobe.com/de/enterprise/using/manage-developers.html)