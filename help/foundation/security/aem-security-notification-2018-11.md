---
title: AEM Sicherheitsbenachrichtigung (November 2018)
seo-title: AEM Sicherheitsbenachrichtigung (November 2018)
description: Dispatcher für Experience Manager-Sicherheitsbenachrichtigungen AEM
seo-description: Dispatcher für Experience Manager-Sicherheitsbenachrichtigungen AEM
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Sicherheit
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 17%

---


# AEM Sicherheitsbenachrichtigung (November 2018)

## Zusammenfassung

In diesem Artikel werden einige Schwachstellen behandelt, die kürzlich in AEM gemeldet wurden. Beachten Sie, dass die am meisten identifizierten Sicherheitslücken bekannte Probleme für das AEM-Produkt waren und bereits erkannt wurden. Für die neuen Schwachstellen ist eine neue Dispatcher-Version verfügbar. Adobe fordert Kunden außerdem nachdrücklich auf, die [AEM Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administring/using/security-checklist.html) auszuführen und die entsprechenden Richtlinien zu befolgen.

## Aktion erforderlich

* AEM -Implementierungen sollten mit der Verwendung der neuesten Dispatcher-Version beginnen.
* Die Dispatcher-Sicherheitsregeln müssen gemäß der empfohlenen Konfiguration angewendet werden.
* Die [AEM Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) sollte für AEM -Bereitstellungen ausgefüllt werden.

## Schwachstellen und Lösungen

| Problem | Auflösung | Links |
|-------|------------|-------|
| Umgehen AEM Dispatcher-Regeln | Installieren Sie die neueste Version des Dispatchers (4.3.1) und befolgen Sie die empfohlene Dispatcher-Konfiguration. | Siehe [AEM Versionshinweise zu Dispatcher](https://helpx.adobe.com/de/experience-manager/dispatcher/release-notes.html) und [Konfigurieren des Dispatchers](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| URL-Filter umgehen Sicherheitslücke, die zur Umgehung von Dispatcher-Regeln verwendet werden könnte - CVE-2016-0957 | Dies wurde in einer älteren Version des Dispatchers behoben. Jetzt wird jedoch empfohlen, die neueste Version des Dispatchers (4.3.1) zu installieren und die empfohlene Dispatcher-Konfiguration zu befolgen. | Siehe [AEM Versionshinweise zu Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) und [Konfigurieren des Dispatchers](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| XSS-Verwundbarkeit im Zusammenhang mit gespeicherten SWF-Dateien | Dies wurde mit Sicherheitskorrekturen behoben, die zuvor veröffentlicht wurden. | Siehe [AEM Sicherheitsbulletin APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Kennwortbezogene Exploits | Befolgen Sie die Empfehlungen in der Sicherheitscheckliste für bessere Passwörter. | Siehe [AEM Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Festplattenauslastung anonymer Benutzer | Dieses Problem wurde für AEM 6.1 und höher behoben. Für AEM 6.0 können die nativen Berechtigungen geändert werden, um sie restriktiver zu gestalten. | Siehe [Versionshinweise](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=de#how-to-install-documentation-package)für AEM 6.1 und älter. |
| Exposition von Open Social Proxy für anonyme Benutzer | Dies wurde in Versionen ab 6.0 SP2 behoben. | Siehe [Versionshinweise](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) für AEM 6.1 und älter. |
| CRX Explorer Zugriff auf Produktionsinstanzen | Die Verwaltung des Zugriffs auf den CRX Explorer ist bereits in der Sicherheitscheckliste behandelt. Der CRX Explorer sollte aus der Produktionsautor- und -Veröffentlichungsinstanz entfernt werden und die Sicherheits-Konsistenzprüfung meldet dies, wenn er nicht entfernt wurde. | Siehe [AEM Sicherheitscheckliste](https://helpx.adobe.com/experience-manager/6-4/sites/administring/using/security-checklist.html). |
| BGServlets wird verfügbar gemacht | Dies wurde seit AEM 6.2 behoben. | Siehe [AEM 6.2 Versionshinweise](https://helpx.adobe.com/de/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher-Benutzerhandbuch](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Versionshinweise zu AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM Sicherheitsbulletins](https://helpx.adobe.com/security.html#experience-manager)

