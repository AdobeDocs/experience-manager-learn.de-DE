---
title: Konfigurieren des Zugriffs auf AEM as a Cloud Service
description: AEM as a Cloud Service ist die Cloud-native Methode zur Nutzung der AEM-Anwendungen und nutzt daher Adobe IMS (Identity Management System), um die Anmeldung von Benutzenden, sowohl Administrierenden als auch regulären Benutzenden, beim AEM-Autoren-Service zu erleichtern. Erfahren Sie, wie Adobe IMS-Benutzende, Benutzergruppen und Produktprofile gemeinsam mit AEM-Gruppen und -Berechtigungen verwendet werden, um bestimmten Zugriff auf die AEM-Autoreninstanz zu gewähren.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 143
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 100%

---

# Konfigurieren des Zugriffs auf AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Einführung in Adobe IMS"
>abstract="AEM as a Cloud Service nutzt Adobe IMS (Identity Management System), um die Anmeldung von Benutzenden, sowohl Administrierenden als auch normalen Benutzenden, beim AEM-Autoren-Service zu erleichtern. Erfahren Sie, wie Adobe IMS-Benutzende, -Gruppen und -Produktprofile gemeinsam mit AEM-Gruppen und -Berechtigungen verwendet werden, um einen spezifischen Zugriff auf den AEM-Autoren-Service zu ermöglichen."

AEM as a Cloud Service ist die Cloud-native Methode zur Nutzung der AEM-Anwendungen und nutzt daher Adobe IMS (Identity Management System), um die Anmeldung von Benutzenden, sowohl Administrierenden als auch regulären Benutzenden, beim AEM-Autoren-Service zu erleichtern.

![Adobe Admin Console](./assets/hero.png)

Erfahren Sie, wie Adobe IMS-Benutzende, -Gruppen und -Produktprofile gemeinsam mit AEM-Gruppen und -Berechtigungen verwendet werden, um einen spezifischen Zugriff auf den AEM-Autoren-Service zu ermöglichen.

## Adobe IMS-Benutzende

Benutzende, die Zugriff auf den AEM-Autoren-Service benötigen, werden als [Adobe IMS-Benutzende](https://helpx.adobe.com/de/enterprise/using/set-up-identity.html) in der [Adobe Admin Console](https://adminconsole.adobe.com) verwaltet. Erfahren Sie, was Adobe IMS-Benutzende sind und wie sie in Admin Console aufgerufen und verwaltet werden.

>[!NOTE]
>
>Wenn IMS-Benutzende aus der Admin Console gelöscht werden, werden sie nicht automatisch aus AEM gelöscht, aber sobald die AEM-Sitzung (d. h. deren Token) abgelaufen ist, können sie sich NICHT bei AEM anmelden.


[Informationen zu Adobe IMS-Benutzenden](./adobe-ims-users.md)

## Adobe IMS-Benutzergruppen

Benutzende, die auf den AEM-Autoren-Service zugreifen, sollten mithilfe von [Adobe IMS-Benutzergruppen](https://helpx.adobe.com/de/enterprise/using/user-groups.html) in der [Adobe Admin Console](https://adminconsole.adobe.com) in logische Gruppen kategorisiert werden. Adobe IMS-Benutzergruppen gewähren keine direkten Berechtigungen oder Zugriff auf AEM (dies ist die Aufgabe der [Adobe IMS-Produktprofile](#adobe-ims-product-profiles)), sie sind jedoch eine hervorragende Möglichkeit, logische Benutzergruppen zu definieren, die mithilfe von AEM-Gruppen und -Berechtigungen in bestimmte Zugriffsebenen des AEM-Autoren-Service übersetzt werden können.

[Informationen zu Adobe IMS-Benutzergruppen](./adobe-ims-user-groups.md)

## Adobe IMS-Produktprofile

In der [Adobe Admin Console](https://adminconsole.adobe.com) verwaltete [Adobe IMS-Produktprofile](https://helpx.adobe.com/de/enterprise/using/manage-permissions-and-roles.html) dienen als Mechanismus, der [Adobe IMS-Benutzenden](#adobe-ims-users) Zugriff zur Anmeldung beim AEM-Autoren-Service auf einem Basiszugrifflevel erteilt.

+ Das Produktprofil __AEM-Benutzer__ ermöglicht Benutzenden schreibgeschützten Zugriff auf AEM über die Mitgliedschaft in der Gruppe der Mitwirkenden von AEM.
+ Das Produktprofil __AEM-Administratoren__ bietet Benutzenden vollständigen, administrativen Zugriff auf AEM.

[Informationen zu Adobe IMS-Produktprofilen](./adobe-ims-product-profiles.md)

## AEM-Benutzergruppen und -Berechtigungen

Adobe Experience Manager basiert auf Adobe IMS-Benutzenden, Benutzergruppen und Produktprofilen, um Benutzenden anpassbaren Zugriff auf AEM zu ermöglichen. Erfahren Sie, wie Sie AEM-Gruppen und -Berechtigungen erstellen und wie diese in Abstimmung mit Adobe IMS-Abstraktionen funktionieren, um einen nahtlosen und anpassbaren Zugriff auf AEM zu ermöglichen.

[Informationen zu AEM-Benutzenden, -Gruppen und -Berechtigungen](./aem-users-groups-and-permissions.md)

## Anleitung zu Zugriff und Berechtigungen

Eine kurze Anleitung zur Konfiguration von Adobe IMS-Benutzenden, -Benutzergruppen und -Produktprofilen in der Adobe Admin Console und zur Verwendung dieser Adobe IMS-Abstraktionen im AEM Author, um bestimmte gruppenbasierte Berechtigungen zu definieren und zu verwalten.

[Anleitung zu AEM-Zugriff und -Berechtigungen](./walk-through.md)

## Zusätzliche Adobe Admin Console-Ressourcen

Die folgende Dokumentation behandelt spezifische Details und Überlegungen zur [Adobe Admin Console](https://adminconsole.adobe.com), die zu einem besseren Verständnis der Adobe Admin Console und deren Verwendung bei der Verwaltung der Benutzenden und des Zugriffs über Experience Cloud-Produkte hinweg beitragen können.

+ [Identitätsüberblick für die Adobe Admin Console](https://helpx.adobe.com/de/enterprise/using/identity.html)
+ [Administratorrollen der Adobe Admin Console](https://helpx.adobe.com/de/enterprise/using/admin-roles.html)
+ [Entwicklerrollen der Adobe Admin Console](https://helpx.adobe.com/de/enterprise/using/manage-developers.html)
