---
title: AEM-Sicherheitsbenachrichtigung (November 2018)
seo-title: AEM Security Notification (November 2018)
description: AEM Experience Manager Sicherheits-Benachrichtigungs-Dispatcher
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: ht
source-wordcount: '428'
ht-degree: 100%

---

# AEM-Sicherheitsbenachrichtigung (November 2018)

## Zusammenfassung

In diesem Artikel werden einige Schwachstellen behandelt, die kürzlich in AEM gemeldet wurden. Beachten Sie, dass die am meisten identifizierten Sicherheitslücken bekannte Probleme für das AEM-Produkt waren und bereits erkannt wurden. Für die neuen Schwachstellen ist eine neue Dispatcher-Version verfügbar. Adobe fordert seine Kunden außerdem auf, die [AEM-Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administring/using/security-checklist.html) auszufüllen und die entsprechenden Richtlinien zu befolgen.

## Aktion erforderlich

* AEM-Bereitstellungen sollten mit der Verwendung der neuesten Dispatcher-Version beginnen.
* Die Dispatcher-Sicherheitsregeln müssen gemäß der empfohlenen Konfiguration angewendet werden.
* Die [AEM-Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administring/using/security-checklist.html) sollte für AEM-Bereitstellungen ausgefüllt werden.

## Schwachstellen und Lösungen

| Problem | Auflösung | Links |
|-------|------------|-------|
| Umgehen der AEM Dispatcher-Regeln | Installieren Sie die neueste Version des Dispatchers (4.3.1) und befolgen Sie die empfohlene Dispatcher-Konfiguration. | Siehe [AEM Dispatcher-Versionshinweise](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) und [Konfigurieren des Dispatchers](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| URL-Filter umgehen Sicherheitslücken, die zur Umgehung von Dispatcher-Regeln verwendet werden könnten – CVE-2016-0957 | Dies wurde in einer älteren Version des Dispatchers behoben. Jetzt wird jedoch empfohlen, die neueste Version des Dispatchers (4.3.1) zu installieren und die empfohlene Dispatcher-Konfiguration zu befolgen. | Siehe [AEM Dispatcher-Versionshinweise](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) und [Konfigurieren des Dispatchers](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| XSS-Verwundbarkeit im Zusammenhang mit gespeicherten SWF-Dateien | Dies wurde mit Sicherheitskorrekturen behoben, die zuvor veröffentlicht wurden. | Siehe [AEM-Sicherheitsbulletin APSB18-10](https://helpx.adobe.com/de/security/products/experience-manager/apsb18-10.html). |
| Kennwortbezogene Exploits | Befolgen Sie die Empfehlungen in der Sicherheitscheckliste für sichere Kennwörter. | Siehe [AEM-Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administring/using/security-checklist.html) |
| Festplattenauslastung für anonyme Benutzende | Dieses Problem wurde für AEM 6.1 und höher behoben. Für AEM 6.0 können die nativen Berechtigungen geändert werden, um sie restriktiver zu gestalten. | Siehe [Versionshinweise](https://helpx.adobe.com/de/experience-manager/aem-previous-versions.html) für AEM 6.1 und älter. |
| Offenlegung von Open Social Proxy für anonyme Benutzende | Dies wurde in Versionen ab 6.0 SP2 behoben. | Siehe [Versionshinweise](https://helpx.adobe.com/de/experience-manager/aem-previous-versions.html) für AEM 6.1 und älter. |
| CRX Explorer-Zugriff auf Produktionsinstanzen | Die Verwaltung des Zugriffs auf den CRX Explorer ist bereits in der Sicherheitscheckliste behandelt. Der CRX Explorer sollte aus der Produktionsautor- und -Veröffentlichungsinstanz entfernt werden und wenn er nicht entfernt wurde, meldet die Sicherheits-Konsistenzprüfung dies. | Siehe [AEM-Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-4/sites/administring/using/security-checklist.html). |
| BGServlets ist offengelegt | Dies wurde seit AEM 6.2 behoben. | Siehe [Versionshinweise zu AEM 6.2](https://helpx.adobe.com/de/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher-Benutzerhandbuch](https://helpx.adobe.com/de/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher-Versionshinweise](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM-Sicherheitsbulletins](https://helpx.adobe.com/de/security.html#experience-manager)
