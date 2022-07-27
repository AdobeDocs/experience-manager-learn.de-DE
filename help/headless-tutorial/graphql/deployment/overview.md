---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# AEM Headless-Implementierungen

AEM Headless-Client-Bereitstellungen haben viele Formen. AEM gehostete SPA, externe SPA oder Website, mobile App oder sogar Server-zu-Server-Prozess.

Abhängig vom Client und dessen Bereitstellung gelten AEM Headless-Implementierungen unterschiedliche Überlegungen.

## AEM Service-Architektur

Bevor Sie Überlegungen zur Bereitstellung anstellen, müssen Sie AEM logische Architektur sowie die Trennung und die Rollen AEM Dienstebenen verstehen. AEM as a Cloud Service umfasst zwei logische Dienste:

+ __AEM-Autor__ ist der Dienst, bei dem Teams Inhaltsfragmente (und andere Assets) erstellen, zusammenarbeiten und veröffentlichen.
+ __AEM-Veröffentlichung__ ist der Dienst, bei dem veröffentlichte Inhaltsfragmente (und andere Assets) für den allgemeinen Gebrauch repliziert werden.

AEM Headless-Clients, die in einer Produktionskapazität arbeiten, interagieren normalerweise mit AEM Publish, das die genehmigten, veröffentlichten Inhalte enthält. Bei der Interaktion des Clients mit der AEM-Autoreninstanz muss besonders berücksichtigt werden, da die AEM-Autoreninstanz standardmäßig sicher ist und eine Autorisierung für alle Anforderungen erforderlich ist. Außerdem kann sie unvollständige oder nicht genehmigte Inhalte enthalten.

## Headless-Client-Bereitstellungen

+ [Einzelseitenanwendung](./spa.md)
+ [Webkomponente/JS](./web-component.md)
+ [Mobile App](./mobile.md)
+ [Server an Server](./server-to-server.md)

