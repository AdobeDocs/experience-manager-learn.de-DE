---
title: Integrieren von Experience Platform-Datenerfassungs-Tags (Launch) in AEM
description: Experience Platform-Datenerfassungs-Tags sind die Tag-Management-Plattform der nächsten Generation von Adobe und die beste Methode zur Implementierung von Adobe Analytics, Target, Audience Manager und vielen weiteren Lösungen. Verschaffen Sie sich einen Überblick über Tags (ehemals Launch) und die empfohlene Integration in Adobe Experience Manager.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 247
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 94%

---

# Integrieren von Experience Platform-Datenerfassungs-Tags in AEM {#overview}

Erfahren Sie, wie Sie Experience Platform-_Datenerfassungs-Tags_ (früher Launch) in Adobe Experience Manager integrieren.

>[!NOTE]
>
>Adobe Experience Platform Launch wurde als eine Suite von Datenerfassungstechnologien in Adobe Experience Platform umbenannt. Infolgedessen wurden in der gesamten Produktdokumentation verschiedene terminologische Änderungen durchgeführt. Eine konsolidierte Übersicht über die terminologischen Änderungen finden Sie in diesem [Dokument](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?lang=de).


Tags sind die nächste Generation der Tag-Management-Technologien von Adobe Experience Platform. Tags stellen die einfachste Möglichkeit zur Bereitstellung von Adobe Analytics, Target, Audience Manager und vielen weiteren Lösungen dar. Verschaffen Sie sich einen Überblick über Tags und die empfohlene Integration in Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## Voraussetzungen

Bei der Integration der Experience Platform-Datenerfassungs-Tag ist Folgendes erforderlich:

+ AEM-Adminzugriff auf AEM as a Cloud Service-Umgebungen
+ Bereitstellung einer Referenz-Website wie [WKND](https://github.com/adobe/aem-guides-wknd) in der Umgebung
+ Zugriff auf die Datenerfassungslösung von Adobe Experience Platform
+ Systemadmin-Zugriff auf [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## Allgemeine Schritte

+ Erstellen Sie in der Adobe Experience Platform-Datenerfassung eine Tag-Eigenschaft und bearbeiten Sie sie, um eine _Regel hinzuzufügen_. _Fügen Sie dann eine Bibliothek hinzu_, wählen Sie die neu hinzugefügte Regel aus, genehmigen und veröffentlichen Sie sie.
+ Verbinden Sie AEM und Tags mit einer vorhandenen (oder neuen) IMS-Konfiguration.
+ Erstellen Sie in AEM eine Cloud-Service-Konfiguration für Launch, wenden Sie sie dann auf eine vorhandene Site an und überprüfen Sie schließlich, ob die Tags-Eigenschaft und ihre Bibliotheken in der Veröffentlichungs- oder Autoren-Site geladen werden.

## Nächste Schritte

[Erstellen einer Tag-Eigenschaft](create-tag-property.md)

## Zusätzliche Ressourcen {#additional-resources}

+ [Experience Platform-Integrationen mit Experience Cloud-Anwendungen](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=de)
+ [Tags-Überblick](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=de)
+ [Implementieren von Experience Cloud in Websites mit Launch](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=de)
