---
title: Integrieren von Tags in Adobe Experience Platform und AEM
description: Experience Platform-Datenerfassungs-Tags sind die Tag-Management-Plattform der nächsten Generation von Adobe und die beste Methode zur Implementierung von Adobe Analytics, Target, Audience Manager und vielen weiteren Lösungen. Verschaffen Sie sich einen Überblick über Tags in Adobe Experience Platform und die empfohlene Integration mit Adobe Experience Manager.
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
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Integrieren von Experience Platform-Datenerfassungs-Tags in AEM {#overview}

Erfahren Sie, wie Sie die Tags in Adobe Experience Platform mit Adobe Experience Manager integrieren.

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
+ Verbinden Sie AEM und Tags mit einer vorhandenen (oder neuen) IMS-Konfiguration
+ Erstellen Sie in AEM eine Cloud-Service-Konfiguration für Tags, wenden Sie sie dann auf eine vorhandene Site an und überprüfen Sie schließlich, ob die Tags-Eigenschaft und ihre Bibliotheken in der veröffentlichten oder autorenseitigen Site geladen werden.

## Nächste Schritte

[Erstellen einer Tag-Eigenschaft](create-tag-property.md)

## Zusätzliche Ressourcen {#additional-resources}

+ [Experience Platform-Integrationen mit Experience Cloud-Anwendungen](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=de)
+ [Tags-Überblick](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=de)
+ [Implementieren von Experience Cloud in Websites mit Launch](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=de)
