---
title: Geschlossene Benutzergruppen in AEM Assets
description: „Geschlossene Benutzergruppe“ (Closed User Group, CUG) ist eine Funktion, mit der der Zugriff auf Inhalte auf eine ausgewählte Benutzergruppe auf einer veröffentlichten Site beschränkt wird. In diesem Video wird gezeigt, wie geschlossene Benutzergruppen mit Adobe Experience Manager Assets verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 340
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 100%

---

# Geschlossene Benutzergruppen{#using-closed-user-groups-with-aem-assets}

„Geschlossene Benutzergruppe“ (Closed User Group, CUG) ist eine Funktion, mit der der Zugriff auf Inhalte auf eine ausgewählte Benutzergruppe auf einer veröffentlichten Site beschränkt wird. In diesem Video wird gezeigt, wie geschlossene Benutzergruppen mit Adobe Experience Manager Assets verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken. Die Unterstützung für geschlossene Benutzergruppen mit AEM Assets wurde erstmals in AEM 6.4 eingeführt.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Geschlossene Benutzergruppe mit AEM Assets

* Die CUG-Funktion wurde entwickelt, um den Zugriff auf Assets in einer AEM-Veröffentlichungsinstanz zu beschränken.
* Gewährt Lesezugriff für eine Reihe von Benutzenden/Gruppen.
* Eine CUG kann nur auf Ordnerebene konfiguriert werden. Eine CUG kann nicht für einzelne Assets festgelegt werden.
* CUG-Richtlinien werden automatisch von allen Unterordnern und angewendeten Assets übernommen.
* CUG-Richtlinien können durch Unterordner außer Kraft gesetzt werden, indem eine neue CUG-Richtlinie festgelegt wird. Dies sollte sparsam genutzt werden und gilt nicht als Best Practice.

## Geschlossene Benutzergruppen und Zugriffssteuerungslisten {#closed-user-groups-vs-access-control-lists}

Sowohl geschlossene Benutzergruppen als auch Zugriffssteuerungslisten (Access Control Lists, ACLs) werden verwendet, um den Zugriff auf Inhalte in AEM zu steuern, und basieren auf AEM-Sicherheitsbenutzenden und -gruppen. Anwendung und Implementierung dieser Funktionen sind jedoch sehr unterschiedlich. In der folgenden Tabelle werden die Unterschiede zwischen den beiden Funktionen zusammengefasst.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Verwendungszweck | Konfigurieren und Anwenden von Berechtigungen für Inhalte in der **aktuellen** AEM-Instanz. | Konfigurieren von CUG-Richtlinien für Inhalte in der AEM-**Autoreninstanz**. Anwenden von CUG-Richtlinien auf Inhalte in AEM-**Veröffentlichungsinstanzen**. |
| Berechtigungsebenen | Definiert gewährte/verweigerte Berechtigungen für Benutzende/Gruppen für alle Ebenen: Lesen, Ändern, Erstellen, Löschen, ACL lesen, ACL bearbeiten, Replizieren. | Gewährt Lesezugriff für eine Reihe von Benutzenden/Gruppen. Verweigert den Lesezugriff für *alle anderen* Benutzenden/Gruppen. |
| Veröffentlichung | ACLs werden *nicht* mit Inhalten veröffentlicht. | CUG-Richtlinien *werden* mit Inhalten veröffentlicht. |

## Unterstützende Links {#supporting-links}

* [Verwalten von Assets und geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=de#closed-user-group)
* [Erstellen von geschlossenen Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html?lang=de)
* [Dokumentation zu geschlossenen Oak-Benutzergruppen](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
