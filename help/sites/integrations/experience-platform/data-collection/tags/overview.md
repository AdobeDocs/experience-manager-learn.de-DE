---
title: Integrieren von Experience Platform-Datenerfassungs-Tags (Launch) und AEM
description: Tags in der Datenerfassung von Experience Platform sind die Tag-Management-Lösung der nächsten Generation der Adobe und die beste Methode zur Bereitstellung von Adobe Analytics, Target, Audience Manager und vielen weiteren Lösungen. Verschaffen Sie sich einen Überblick über Tags (ehemals Launch) und die empfohlene Integration in Adobe Experience Manager.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# Integrieren von Experience Platform-Datenerfassungs-Tags und -AEM {#overview}

Erfahren Sie, wie Sie die Experience Platform integrieren. _Datenerfassungs-Tags_ (früher als Launch bezeichnet) mit Adobe Experience Manager.

>[!NOTE]
>
>Adobe Experience Platform Launch wurde als eine Suite von Datenerfassungstechnologien in Adobe Experience Platform umbenannt. Infolgedessen wurden in der gesamten Produktdokumentation mehrere terminologische Änderungen eingeführt. Siehe Folgendes [Dokument](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) für eine konsolidierte Übersicht über die terminologischen Änderungen.


Tags sind die nächste Generation der Tag-Management-Technologie von Adobe Experience Platform. Tags bieten die einfachste Möglichkeit zur Bereitstellung von Adobe Analytics, Target, Audience Manager und vielen weiteren Lösungen. Verschaffen Sie sich einen Überblick über Tags und die empfohlene Integration mit Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## Voraussetzungen

Bei der Integration von Datenerfassungs-Tags in Experience Platform sind folgende Elemente erforderlich:

+ AEM Administratorzugriff auf AEM as a Cloud Service Umgebung
+ Eine Referenz-Website wie [WKND](https://github.com/adobe/aem-guides-wknd) bereitgestellt.
+ Zugriff auf die Datenerfassungslösung von Adobe Experience Platform
+ Systemadministratorzugriff auf [Adobe Developer-Konsole](https://developer.adobe.com/developer-console/)


## Allgemeine Schritte

+ Erstellen Sie in der Adobe Experience Platform-Datenerfassung eine Tag-Eigenschaft und bearbeiten Sie sie in _Regel hinzufügen_. Dann _Bibliothek hinzufügen_, wählen Sie die neu hinzugefügte Regel aus, genehmigen und veröffentlichen Sie sie.
+ Verbinden von AEM und Tags mit einer vorhandenen (oder neuen) IMS-Konfiguration
+ Erstellen Sie in AEM eine Cloud Services-Konfiguration für Launch, wenden Sie sie dann auf eine vorhandene Site an und überprüfen Sie schließlich, ob die Eigenschaft &quot;Tags&quot;und ihre Bibliotheken auf der Veröffentlichungs- oder Autorensite geladen werden.

## Nächste Schritte

[Erstellen einer Tag-Eigenschaft](create-tag-property.md)

## Zusätzliche Ressourcen {#additional-resources}

+ [Experience Platform-Integrationen mit Experience Cloud-Anwendungen](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Übersicht über Tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementieren des Experience Cloud in Websites mit Tags](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
