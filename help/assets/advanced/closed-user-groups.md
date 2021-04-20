---
title: Geschlossene Benutzergruppen in AEM Assets
description: Closed User Groups (CUGs) ist eine Funktion, mit der der Zugriff auf Inhalte auf eine bestimmte Benutzergruppe auf einer veröffentlichten Site eingeschränkt wird. In diesem Video wird gezeigt, wie mit Adobe Experience Manager Assets geschlossene Benutzergruppen verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken.
version: 6.3, 6.4, 6.5, cloud-service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 2%

---


# Geschlossene Benutzergruppen{#using-closed-user-groups-with-aem-assets}

Closed User Groups (CUGs) ist eine Funktion, mit der der Zugriff auf Inhalte auf eine bestimmte Benutzergruppe auf einer veröffentlichten Site eingeschränkt wird. In diesem Video wird gezeigt, wie mit Adobe Experience Manager Assets geschlossene Benutzergruppen verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken. Die Unterstützung für geschlossene Benutzergruppen mit AEM Assets wurde erstmals in AEM 6.4 eingeführt.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Closed User Group (CUG) mit AEM Assets

* Entwickelt, um den Zugriff auf Assets in einer AEM Publish-Instanz zu beschränken.
* Gewährt Lesezugriff auf eine Gruppe von Benutzern/Gruppen.
* CUG kann nur auf Ordnerebene konfiguriert werden. CUG kann nicht für einzelne Assets festgelegt werden.
* CUG-Richtlinien werden automatisch von allen Unterordnern und angewendeten Assets geerbt.
* CUG-Richtlinien können durch Unterordner überschrieben werden, indem eine neue CUG-Richtlinie festgelegt wird. Dies sollte nur sparsam eingesetzt werden und wird nicht als bewährte Praxis betrachtet.

## Geschlossene Benutzergruppen im Vergleich zu Listen der Zugriffskontrolle {#closed-user-groups-vs-access-control-lists}

Sowohl Closed User Groups (CUG) als auch Zugriffskontrolle Listen (ACL) werden verwendet, um den Zugriff auf Inhalte in AEM zu steuern, und zwar auf der Grundlage AEM Sicherheitsbenutzer und -gruppen. Die Anwendung und Implementierung dieser Funktionen ist jedoch sehr unterschiedlich. Die folgende Tabelle fasst die Unterschiede zwischen den beiden Funktionen zusammen.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Vorgesehene Verwendung | Konfigurieren und wenden Sie Berechtigungen für Inhalte auf der AEM **current**-Instanz an. | Konfigurieren Sie CUG-Richtlinien für Inhalte in AEM **author**-Instanz. Wenden Sie CUG-Richtlinien für Inhalte auf AEM **publish**-Instanz(en) an. |
| Berechtigungsstufen | Definiert zugeteilte/verweigerte Berechtigungen für Benutzer/Gruppen für alle Ebenen: Lesen, Ändern, Erstellen, Löschen, Lesen, ACL bearbeiten, Replizieren. | Gewährt Lesezugriff auf eine Gruppe von Benutzern/Gruppen. Lese-Zugriff auf *alle anderen* Benutzer/Gruppen verweigert. |
| Veröffentlichung | ACLs sind *nicht* veröffentlicht mit Inhalt. | CUG-Richtlinien *werden mit Inhalt veröffentlicht.* |

## Unterstützende Links {#supporting-links}

* [Verwalten von Assets und geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Erstellen von geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Dokumentation zu geschlossenen Benutzergruppen von Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
