---
title: Aktivieren der Front-End-Pipeline für den standardmäßigen AEM Projektarchetyp
description: Konvertieren Sie ein standardmäßiges AEM-Projekt für eine schnellere Bereitstellung stmöglicher statischer Ressourcen wie CSS, JavaScript, Schriftarten und Symbole mithilfe der Front-End-Pipeline. Und Trennung der Front-End-Entwicklung von der vollständigen Stack-Back-End-Entwicklung auf AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 2%

---


# Aktivieren der Front-End-Pipeline für den standardmäßigen AEM Projektarchetyp{#enable-front-end-pipeline-standard-aem-project}

Erfahren Sie, wie Sie das AEM mit [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) um statische Ressourcen wie CSS, JavaScript, Schriftarten und Symbole mithilfe der Front-End-Pipeline bereitzustellen, um den Entwicklungszyklus zu beschleunigen. Und Trennung der Front-End-Entwicklung von der vollständigen Stack-Back-End-Entwicklung auf AEM. Außerdem erfahren Sie, wie diese statischen Ressourcen __not__ aus AEM Repository, aber aus dem CDN bereitgestellt wird, eine Änderung des Bereitstellungsparadigmas.

In Adobe Cloud Manager wird eine neue Front-End-Pipeline erstellt, die nur erstellt und bereitgestellt wird. `ui.frontend` Artefakte an CDN weiterleiten und AEM über den Speicherort informieren. Während der HTML-Erstellung der Webseite muss die `<link>` und `<script>` -Tags verweisen auf diesen Ort in `href` -Attributwert.

Nach der standardmäßigen AEM-Projektkonvertierung können die Frontend-Entwickler getrennt von und parallel zu jeder vollständigen Back-End-Entwicklung in AEM arbeiten, die über eigene Bereitstellungs-Pipelines verfügt.

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>Dies gilt nur für AEM as a Cloud Service und nicht für AMS-basierte Adobe Cloud Manager-Implementierungen.

