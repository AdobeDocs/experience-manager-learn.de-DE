---
title: Geschlossene Benutzergruppen in AEM Assets
description: 'Closed User Groups (CUGs) ist eine Funktion, mit der der Zugriff auf Inhalte auf eine bestimmte Benutzergruppe auf einer veröffentlichten Site eingeschränkt wird. In diesem Video wird gezeigt, wie mit Adobe Experience Manager Assets geschlossene Benutzergruppen verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken. Die Unterstützung für geschlossene Benutzergruppen mit AEM Assets wurde erstmals in AEM 6.4 eingeführt. '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 1%

---


# Closed User Groups{#using-closed-user-groups-with-aem-assets}

Closed User Groups (CUGs) ist eine Funktion, mit der der Zugriff auf Inhalte auf eine bestimmte Benutzergruppe auf einer veröffentlichten Site eingeschränkt wird. In diesem Video wird gezeigt, wie mit Adobe Experience Manager Assets geschlossene Benutzergruppen verwendet werden können, um den Zugriff auf einen bestimmten Asset-Ordner zu beschränken. Die Unterstützung für geschlossene Benutzergruppen mit AEM Assets wurde erstmals in AEM 6.4 eingeführt.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## Closed User Group (CUG) mit AEM Assets

* Entwickelt, um den Zugriff auf Assets in einer AEM Publish-Instanz zu beschränken.
* Gewährt Lesezugriff auf eine Gruppe von Benutzern/Gruppen.
* CUG kann nur auf Ordnerebene konfiguriert werden. CUG kann nicht für einzelne Assets festgelegt werden.
* CUG-Richtlinien werden automatisch von allen Unterordnern und angewendeten Assets geerbt.
* CUG-Richtlinien können durch Unterordner überschrieben werden, indem eine neue CUG-Richtlinie festgelegt wird. Dies sollte nur sparsam eingesetzt werden und wird nicht als bewährte Praxis betrachtet.

## CUG-Vertretung im JCR {#cug-representation-in-the-jcr}

![CUG-Vertretung im JCR](assets/closed-user-groups/folder-properties-closed-user-groups.png)

Wir.Retail-Mitglieder Gruppe wurde als geschlossene Benutzergruppe zu Ordner hinzugefügt: /content/dam/we-retail/en/beta-products

Eine Mischung aus **rep:CugMixin** wird auf den Ordner **/content/dam/we-retail/en/beta-products** angewendet. Unter dem Ordner wird ein Knoten **rep:cugPolicy** hinzugefügt, und wir-retail-Mitglieder werden als Prinzipal angegeben. Eine andere Mischung aus **granite:AuthenticationRequired** wird auf den Ordner beta-products angewendet, und die Eigenschaft** granite:loginPath** gibt die Anmeldeseite an, die verwendet werden soll, wenn ein Benutzer nicht authentifiziert ist, und versucht, ein Asset unterhalb des Ordners **beta-products** anzufordern.

JCR-Beschreibung unten:

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## Listen für geschlossene Benutzergruppen und Zugriffskontrollen {#closed-user-groups-vs-access-control-lists}

Sowohl Closed User Groups (CUG) als auch Zugriffskontrolle Listen (ACL) werden verwendet, um den Zugriff auf Inhalte in AEM zu steuern, und zwar auf der Grundlage AEM Sicherheitsbenutzer und -gruppen. Die Anwendung und Implementierung dieser Funktionen ist jedoch sehr unterschiedlich. Die folgende Tabelle fasst die Unterschiede zwischen den beiden Funktionen zusammen.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Vorgesehene Verwendung | Konfigurieren und wenden Sie Berechtigungen für Inhalte auf der **aktuellen** AEM an. | Konfigurieren Sie CUG-Richtlinien für Inhalte in AEM **Autoreninstanz** . Wenden Sie CUG-Richtlinien für Inhalte auf AEM **Veröffentlichungsinstanz** an. |
| Berechtigungsstufen | Definiert zugeteilte/verweigerte Berechtigungen für Benutzer/Gruppen für alle Ebenen: Lesen, Ändern, Erstellen, Löschen, Lesen, ACL bearbeiten, Replizieren. | Gewährt Lesezugriff auf eine Gruppe von Benutzern/Gruppen. Lese-Zugriff auf alle anderen Benutzer/Gruppen verweigert. |
| Replikation | ACLs werden nicht mit Inhalten repliziert. | CUG-Richtlinien werden mit Inhalten repliziert. |

## Unterstützende Links {#supporting-links}

* [Verwalten von Assets und geschlossenen Benutzergruppen](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [Erstellen von geschlossenen Benutzergruppen](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Dokumentation zu geschlossenen Benutzergruppen von Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
