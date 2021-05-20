---
title: Geschlossene Benutzergruppen in AEM Assets
description: Geschlossene Benutzergruppen (CUGs) sind eine Funktion, mit der der Zugriff auf Inhalte auf eine ausgewählte Benutzergruppe auf einer veröffentlichten Site beschränkt wird. In diesem Video wird gezeigt, wie geschlossene Benutzergruppen mit Adobe Experience Manager Assets verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken.
version: 6.3, 6.4, 6.5, cloud-service
topic: Administration, Sicherheit
feature: Benutzer und Gruppen
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '382'
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

## Geschlossene Benutzergruppen vs. Zugriffssteuerungslisten {#closed-user-groups-vs-access-control-lists}

Sowohl geschlossene Benutzergruppen (CUG) als auch Zugriffssteuerungslisten (ACL) werden verwendet, um den Zugriff auf Inhalte in AEM zu steuern, und basieren auf AEM Sicherheitsbenutzern und -gruppen. Die Anwendung und Implementierung dieser Funktionen ist jedoch sehr unterschiedlich. Die folgende Tabelle fasst die Unterscheidungen zwischen den beiden Funktionen zusammen.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Vorgesehene Verwendung | Konfigurieren und wenden Sie Berechtigungen für Inhalte auf der AEM **current** an. | Konfigurieren Sie CUG-Richtlinien für Inhalte in AEM **author** -Instanz. Wenden Sie CUG-Richtlinien für Inhalte auf AEM **publish**-Instanz(en) an. |
| Berechtigungsebenen | Definiert erteilte/abgelehnte Berechtigungen für Benutzer/Gruppen für alle Ebenen: Lesen, Ändern, Erstellen, Löschen, Lesen, ACL, ACL bearbeiten, Replizieren. | Gewährt Lesezugriff für eine Reihe von Benutzern/Gruppen. Verweigert Lesezugriff auf *alle anderen* Benutzer/Gruppen. |
| Veröffentlichung | ACLs sind *nicht*, die mit Inhalten veröffentlicht werden. | CUG-Richtlinien *sind* mit Inhalt veröffentlicht. |

## Unterstützende Links {#supporting-links}

* [Verwalten von Assets und geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Erstellen von geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Dokumentation zu geschlossenen Oak-Benutzergruppen](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
