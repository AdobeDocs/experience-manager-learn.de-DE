---
title: Aktivieren der Frontend-Pipeline für den standardmäßigen AEM-Projektarchetyp
description: Erfahren Sie, wie Sie eine Frontend-Pipeline für standardmäßige AEM-Projekte aktivieren, um statische Ressourcen wie CSS, JavaScript, Schriftarten und Symbole schneller bereitzustellen. Ferner geht es um die Trennung der Frontend-Entwicklung von der Full-Stack-Backend-Entwicklung in AEM.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 216
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 100%

---

# Aktivieren der Frontend-Pipeline für den standardmäßigen AEM-Projektarchetyp{#enable-front-end-pipeline-standard-aem-project}

Erfahren Sie, wie Sie das [AEM-WKND-Sites-Projekt](https://github.com/adobe/aem-guides-wknd) (auch als Standard-AEM-Projekt bezeichnet) aktivieren, das mithilfe des [AEM-Projektarchetyps](https://github.com/adobe/aem-project-archetype) erstellt wurde, um Frontend-Ressourcen wie CSS, JavaScript, Schriftarten und Symbole über eine Frontend-Pipeline bereitzustellen und so einen schnelleren Entwicklungs-Bereitstellungs-Zyklus zu ermöglichen. Ferner geht es um die Trennung der Frontend-Entwicklung von der Full-Stack-Backend-Entwicklung in AEM. Außerdem erfahren Sie, dass diese Frontend-Ressourcen __nicht__ aus dem AEM-Repository, sondern aus dem CDN bereitgestellt werden, was eine Änderung des Bereitstellungsparadigmas bedeutet.


In Adobe Cloud Manager wird eine neue Frontend-Pipeline erstellt, die `ui.frontend`-Artefakte nur erstellt und dem integrierten CDN bereitstellt und AEM über den entsprechenden Speicherort informiert. In AEM verweisen während der HTML-Erstellung der Web-Seite die Tags `<link>` und `<script>` auf diesen Artefaktspeicherort im Attributwert `href`.

Nach der Nach der Konversion des AEM-Projekts „WKND Sites“ können die Frontend-Entwicklerinnen und -Entwickler getrennt von und parallel zu jeder Full-Stack-Backend-Entwicklung in AEM arbeiten, die über eigene Bereitstellungs-Pipelines verfügt.

>[!IMPORTANT]
>
>Im Allgemeinen wird die Frontend-Pipeline normalerweise mit der [schnellen Site-Erstellung von AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=de) verwendet. Weitere Informationen hierzu finden Sie im entsprechenden Tutorial [Erste Schritte mit AEM Sites – Schnelle Site-Erstellung](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=de). In diesem Tutorial und den zugehörigen Videos wird immer wieder darauf verwiesen, damit die feinen Unterschiede beachtet werden. Anhand von direkten oder indirekten Vergleichen werden wichtige Konzepte erklärt.


Ein damit zusammenhängendes [mehrstufiges Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=de) führt Sie durch die Implementierung einer AEM-Website mithilfe der Funktion „Schnelle Site-Erstellung“ für die fiktive Lifestyle-Marke WKND. Sich den [Design-Workflow](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=de) anzusehen, ist ebenfalls hilfreich, um die Funktionsweise der Frontend-Pipeline zu verstehen.

## Überblick, Vorteile und Überlegungen zur Frontend-Pipeline

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Dies gilt nur für AEM as a Cloud Service und nicht für AMS-basierte Adobe Cloud Manager-Bereitstellungen.

## Voraussetzungen

Der Bereitstellungsschritt in diesem Tutorial findet in Adobe Cloud Manager statt. Stellen Sie sicher, dass Sie über eine Rolle __Bereitstellungs-Manager__ verfügen (siehe die [Rollendefinitionen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=de#role-definitions) für Cloud-Manager).

Stellen Sie sicher, dass Sie beim Abschluss dieses Tutorials das [Sandbox-Programm](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=de) und die [Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=de) verwenden.

## Nächste Schritte {#next-steps}

Ein schrittweises Tutorial führt Sie durch die Konversion des[AEM-WKND-Sites-Projekts](https://github.com/adobe/aem-guides-wknd), um diese für die Frontend-Pipeline zu aktivieren.

Worauf warten Sie noch? Starten Sie das Tutorial, indem Sie zum Kapitel [Überprüfen des Full-Stack-Projekts](review-uifrontend-module.md) navigieren und den Lebenszyklus für die Frontend-Entwicklung im Kontext des standardmäßigen AEM Sites-Projekts nochmals durchgehen.
