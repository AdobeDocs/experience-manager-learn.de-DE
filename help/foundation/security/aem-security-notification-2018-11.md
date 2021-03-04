---
title: AEM Security Notification (November 2018)
seo-title: AEM Security Notification (November 2018)
description: Dispatcher zur Sicherheitsbenachrichtigung von AEM Experience Manager
seo-description: Dispatcher zur Sicherheitsbenachrichtigung von AEM Experience Manager
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 17%

---


# AEM Security Notification (November 2018)

## Zusammenfassung

Dieser Artikel behandelt einige Schwachstellen, die in letzter Zeit und früher aufgetreten sind und kürzlich in AEM gemeldet wurden. Beachten Sie, dass die meisten identifizierten Schwachstellen bekannte Probleme für das AEM-Produkt waren und die Abschwächung bereits vorher identifiziert wurde. Eine neue Dispatcher-Version steht für die neuen Schwachstellen zur Verfügung. Adobe fordert Kunden außerdem auf, die [AEM Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administring/using/security-checklist.html) auszufüllen und die entsprechenden Richtlinien zu befolgen.

## Aktion erforderlich

* AEM Bereitstellungen sollten mit der neuesten Dispatcher-Version Beginn werden.
* Die Dispatcher-Sicherheitsregeln müssen gemäß der empfohlenen Konfiguration angewendet werden.
* Die [AEM Sicherheits-Checkliste](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) sollte für AEM Bereitstellung abgeschlossen werden.

## Schwachstellen und Auflösungen

| Problem | Auflösung | Links |
|-------|------------|-------|
| Umgehen AEM Dispatcher-Regeln | Installieren Sie die neueste Version von Dispatcher(4.3.1) und befolgen Sie die empfohlene Dispatcher-Konfiguration. | Siehe [AEM Dispatcher-Versionshinweise](https://helpx.adobe.com/de/experience-manager/dispatcher/release-notes.html) und [Konfigurieren von Dispatcher](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| URL-Filter umgehen Schwachstelle, die zur Umgehung von Dispatcher-Regeln verwendet werden könnte - CVE-2016-0957 | Dies wurde in einer älteren Version von Dispatcher behoben, es wird jedoch empfohlen, die neueste Version von Dispatcher (4.3.1) zu installieren und die empfohlene Dispatcher-Konfiguration zu befolgen. | Siehe [AEM Dispatcher-Versionshinweise](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) und [Konfigurieren von Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| XSS-Sicherheitslücke im Zusammenhang mit gespeicherten SWF-Dateien | Dieses Problem wurde mit zuvor veröffentlichten Sicherheitskorrekturen behoben. | Siehe [AEM Sicherheitsbulletin APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Passwortbezogene Ausnutzungen | Befolgen Sie die Empfehlungen in der Sicherheitsprüfliste, um sicherere Passwörter zu erhalten. | Siehe [AEM Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Festplattennutzung für anonyme Benutzer | Dieses Problem wurde für AEM 6.1 und höher behoben, für AEM 6.0 können die Out-of-the-Box-Berechtigungen geändert werden, um sie restriktiver zu gestalten. | Siehe [Versionshinweise](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=de#how-to-install-documentation-package)für AEM Version 6.1 und älter. |
| Exposition von Open Social Proxy für anonyme Benutzer | Dies wurde in Versionen ab 6.0 SP2 behoben. | Siehe [Versionshinweise](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) für AEM Version 6.1 und älter. |
| Zugriff auf CRX Explorer auf Produktionsinstanzen | Die Verwaltung des Zugriffs auf CRX Explorer wird bereits in der Checkliste für die Sicherheit beschrieben. CRX Explorer sollte aus dem Produktionsautor entfernt und veröffentlicht werden, und die Überprüfung des Sicherheitsstatus meldet dies, wenn sie nicht entfernt wurde. | Siehe [AEM Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-4/sites/administring/using/security-checklist.html). |
| BGServlets ist verfügbar | Dies wurde seit AEM 6.2 behoben. | Siehe [AEM 6.2 Versionshinweise](https://helpx.adobe.com/de/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher-Benutzerhandbuch](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Versionshinweise zu AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM Sicherheitsbulletins](https://helpx.adobe.com/security.html#experience-manager)

