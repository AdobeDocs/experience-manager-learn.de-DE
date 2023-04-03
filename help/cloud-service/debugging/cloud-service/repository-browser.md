---
title: Debugging von AEM mit dem Repository-Browser
description: Der Repository-Browser ist ein leistungsstarkes Tool, das AEM zugrunde liegenden Datenspeicher sichtbar macht und das einfache Debugging AEM as a Cloud Service Umgebung ermöglicht.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Debugging von mit dem Repository-Browser as a Cloud Service AEM

Der Repository-Browser ist ein leistungsstarkes Tool, das AEM zugrunde liegenden Datenspeicher sichtbar macht und das einfache Debugging AEM as a Cloud Service Umgebung ermöglicht. Der Repository-Browser unterstützt eine schreibgeschützte Ansicht der Ressourcen und Eigenschaften von AEM für Produktions-, Staging- und Entwicklungsdienste sowie Autoren-, Veröffentlichungs- und Vorschaudienste.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

Repository-Browser ist __NUR__ verfügbar in AEM as a Cloud Service Umgebungen (verwenden [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) Debugging des lokalen AEM SDK).

## Zugriff auf den Repository-Browser

So greifen Sie auf AEM as a Cloud Service Repository-Browser zu:

1. Stellen Sie sicher, dass Ihr Benutzer [den erforderlichen Zugriff](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Anmelden bei [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Wählen Sie das Programm aus, das die zu debuggende AEM as a Cloud Service Umgebung enthält
1. Öffnen Sie die [Entwicklerkonsole](./developer-console.md) entspricht der zu debuggenden AEM as a Cloud Service Umgebung
1. Wählen Sie die __Repository-Browser__ tab
1. Wählen Sie die zu durchsuchende AEM Dienstebene aus.
   + Alle Autoren
   + Alle Herausgeber
   + Alle Vorschauen
1. Auswählen __Repository-Browser öffnen__

Der Repository-Browser wird für die ausgewählte Dienstebene (Autor, Veröffentlichung oder Vorschau) in einem schreibgeschützten Modus geöffnet und zeigt Ressourcen und Eigenschaften an, auf die Ihr Benutzer Zugriff hat.

## Zugriff veröffentlichen und Vorschau anzeigen

Standardmäßig ist der Zugriff auf Veröffentlichen oder Vorschau eingeschränkt, wodurch die verfügbaren Ressourcen im Repository-Browser reduziert werden. [Um alle Ressourcen in der Veröffentlichungs- (oder Vorschau-)Ansicht anzuzeigen, fügen Sie Benutzer einer Administrator-Rolle vom Typ Veröffentlichen (oder Vorschau) hinzu.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
