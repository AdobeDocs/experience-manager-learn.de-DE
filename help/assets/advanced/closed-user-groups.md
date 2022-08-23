---
title: Geschlossene Benutzergruppen in AEM Assets
description: Geschlossene Benutzergruppen (CUGs) sind eine Funktion, mit der der Zugriff auf Inhalte auf eine ausgewählte Benutzergruppe auf einer veröffentlichten Site beschränkt wird. In diesem Video wird gezeigt, wie geschlossene Benutzergruppen mit Adobe Experience Manager Assets verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 1%

---

# Geschlossene Benutzergruppen{#using-closed-user-groups-with-aem-assets}

Geschlossene Benutzergruppen (CUGs) sind eine Funktion, mit der der Zugriff auf Inhalte auf eine ausgewählte Benutzergruppe auf einer veröffentlichten Site beschränkt wird. In diesem Video wird gezeigt, wie geschlossene Benutzergruppen mit Adobe Experience Manager Assets verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken. Die Unterstützung für geschlossene Benutzergruppen mit AEM Assets wurde erstmals in AEM 6.4 eingeführt.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Geschlossene Benutzergruppe (CUG) mit AEM Assets

* Wurde entwickelt, um den Zugriff auf Assets in einer AEM-Veröffentlichungsinstanz zu beschränken.
* Gewährt Lesezugriff für eine Reihe von Benutzern/Gruppen.
* CUG kann nur auf Ordnerebene konfiguriert werden. CUG kann nicht für einzelne Assets festgelegt werden.
* CUG-Richtlinien werden automatisch von allen Unterordnern und angewendeten Assets übernommen.
* CUG-Richtlinien können durch Unterordner überschrieben werden, indem eine neue CUG-Richtlinie festgelegt wird. Dies sollte sparsam verwendet werden und wird nicht als Best Practice betrachtet.

## Geschlossene Benutzergruppen und Zugriffssteuerungslisten {#closed-user-groups-vs-access-control-lists}

Sowohl geschlossene Benutzergruppen (CUG) als auch Zugriffssteuerungslisten (ACL) werden verwendet, um den Zugriff auf Inhalte in AEM zu steuern, und basieren auf AEM Sicherheitsbenutzern und -gruppen. Die Anwendung und Implementierung dieser Funktionen ist jedoch sehr unterschiedlich. Die folgende Tabelle fasst die Unterscheidungen zwischen den beiden Funktionen zusammen.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Vorgesehene Verwendung | Konfigurieren und Anwenden von Berechtigungen für Inhalte auf **current** AEM Instanz. | Konfigurieren von CUG-Richtlinien für Inhalte in AEM **author** -Instanz. Anwenden von CUG-Richtlinien auf Inhalte in AEM **publish** Instanz(en). |
| Berechtigungsebenen | Definiert erteilte/abgelehnte Berechtigungen für Benutzer/Gruppen für alle Ebenen: Lesen, Ändern, Erstellen, Löschen, Lesen, ACL, ACL bearbeiten, Replizieren. | Gewährt Lesezugriff für eine Reihe von Benutzern/Gruppen. Lese-Zugriff auf verweigert *alle anderen* Benutzer/Gruppen. |
| Veröffentlichung | ACLs sind *not* mit Inhalten veröffentlicht. | CUG-Richtlinien *are* mit Inhalten veröffentlicht. |

## Unterstützende Links {#supporting-links}

* [Verwalten von Assets und geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Erstellen von geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Dokumentation zu geschlossenen Oak-Benutzergruppen](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
