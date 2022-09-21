---
title: Aktivieren der Front-End-Pipeline für den standardmäßigen AEM Projektarchetyp
description: Erfahren Sie, wie Sie eine Front-End-Pipeline für standardmäßige AEM-Projekte aktivieren, um statische Ressourcen wie CSS, JavaScript, Schriftarten und Symbole schneller bereitzustellen. Trennung der Frontend-Entwicklung von der vollständigen Stack-Back-End-Entwicklung auf AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# Aktivieren der Front-End-Pipeline für den standardmäßigen AEM Projektarchetyp{#enable-front-end-pipeline-standard-aem-project}

Erfahren Sie, wie Sie die [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) (auch als Standard-AEM-Projekt bezeichnet), das mithilfe von [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) , um Frontend-Ressourcen wie CSS, JavaScript, Schriftarten und Symbole mithilfe einer Front-End-Pipeline bereitzustellen, um einen schnelleren Entwicklungszyklus bis zur Bereitstellung zu ermöglichen. Die Trennung der Frontend-Entwicklung von der vollständigen Stack-Back-End-Entwicklung auf AEM. Außerdem erfahren Sie, wie diese Frontend-Ressourcen __not__ aus dem AEM-Repository, aber aus dem CDN bereitgestellt wird, eine Änderung des Bereitstellungsparadigmas.


In Adobe Cloud Manager wird eine neue Front-End-Pipeline erstellt, die nur erstellt und bereitgestellt wird. `ui.frontend` Artefakte zum integrierten CDN hinzu und informiert AEM über seinen Speicherort. Bei AEM während der HTML-Erstellung der Webseite muss die `<link>` und `<script>` -Tags, siehe diesen Artefakt-Speicherort im `href` -Attributwert.

Nach der WKND Sites-AEM der Projektkonvertierung können die Frontend-Entwickler jedoch getrennt von und parallel zu jeder vollständigen Back-End-Entwicklung auf AEM arbeiten, die über eigene Bereitstellungs-Pipelines verfügt.

>[!IMPORTANT]
>
>Im Allgemeinen wird die Front-End-Pipeline normalerweise mit dem [AEM schnelle Site-Erstellung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), gibt es ein entsprechendes Tutorial [Erste Schritte mit AEM Sites - Schnelle Site-Erstellung](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) um mehr darüber zu erfahren. In diesem Tutorial und den zugehörigen Videos finden Sie Verweise darauf, um sicherzustellen, dass subtile Unterschiede herausgestellt werden und es einen direkten oder indirekten Vergleich gibt, um wichtige Konzepte zu erklären.


Eine verwandte [Mehrstufiges Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) führt Sie durch die Implementierung einer AEM Site für eine fiktive Lifestyle-Marke des WKND mithilfe der Funktion &quot;Schnelle Site-Erstellung&quot;. Überprüfen der [Designarbeitsablauf](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) Es ist auch hilfreich, die Funktionsweise der Frontend-Pipeline zu verstehen.

## Überblick, Vorteile und Überlegungen zur Front-End-Pipeline

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>Dies gilt nur für AEM as a Cloud Service und nicht für AMS-basierte Adobe Cloud Manager-Implementierungen.

## Voraussetzungen

Der Implementierungsschritt in diesem Tutorial findet in einer Adobe Cloud Manager statt. Stellen Sie sicher, dass Sie über eine __Bereitstellungsmanager__ Rolle, siehe Cloud-Verwaltung [Rollendefinitionen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Stellen Sie sicher, dass Sie die [Sandbox-Programm](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) und [Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) zum Abschluss dieses Tutorials.

## Nächste Schritte {#next-steps}

Eine schrittweise Anleitung führt Sie durch das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) Konvertierung, um sie für die Front-End-Pipeline zu aktivieren.

Worauf wartest du? Starten Sie das Tutorial, indem Sie zur [Vollstack-Projekt überprüfen](review-uifrontend-module.md) Kapitel erstellen und den Lebenszyklus der Frontend-Entwicklung im Kontext des standardmäßigen AEM Sites-Projekts neu definieren.

