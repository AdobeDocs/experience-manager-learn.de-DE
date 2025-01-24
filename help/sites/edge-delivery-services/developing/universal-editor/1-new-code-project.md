---
title: Erstellen eines Code-Projekts
description: Erstellen Sie ein Codeprojekt für Edge Delivery Services, das mit dem universellen Editor bearbeitet werden kann.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 1%

---


# Erstellen eines Edge Delivery Services-Code-Projekts

Verwenden Sie zum Erstellen von AEM-Websites für Edge Delivery Services und den universellen Editor die Projektvorlage Adobe [AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Diese Vorlage erstellt ein neues Codeprojekt, das das CSS und die JavaScript enthält, die zum Erstellen des Website-Erlebnisses verwendet werden. Diese Vorlage erstellt ein neues GitHub-Repository und lädt es mit Textbausteincode und Konfiguration der Adobe, was eine solide Grundlage für Ihr AEM-Website-Projekt bietet.

Denken Sie daran, dass [AEM-Websites, die von Edge Delivery Services bereitgestellt ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview), nur Client-seitigen (Browser-)Code haben. Website-Code wird nicht in den AEM-Autoren- oder Publish-Services ausgeführt.

![Neues Edge Delivery Services-Projekt](./assets/1-new-project/new-project.png)

Gehen Sie wie folgt vor, um ein Edge Delivery Services-Code-Projekt zu erstellen, dessen Inhalt im universellen Editor bearbeitbar ist:

1. **Einrichten eines GitHub-Kontos.** Wenn Sie ein Projekt für Ihre Organisation erstellen, stellen Sie sicher, dass die Organisation über ein GitHub-Konto verfügt und Sie Mitglied sind.
2. **Erstellen Sie ein neues Code** Projekt mithilfe der Projektvorlage [AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Installieren Sie die GitHub-App &quot;AEM Code Sync** und gewähren Sie Zugriff auf das Repository. Sie finden die [App hier](https://github.com/apps/aem-code-sync).
4. **Konfigurieren Sie die`fstab.yaml`** Ihres neuen Projekts so, dass sie auf den richtigen AEM-Autoren-Service verweist.

   * Zum Experimentieren können Sie niedrigere AEM as a Cloud Service-Umgebungen (Staging, Entwicklung oder RDE) verwenden. Echte Websites-Implementierungen sollten jedoch so konfiguriert werden, dass sie einen Produktions-AEM-Autoren-Service verwenden.

5. **Bearbeiten Sie die`paths.json`** Ihres neuen Projekts, um den AEM Author-Dienstpfad dem Stamm Ihrer Website zuzuordnen.

Detailliertere Anweisungen finden Sie im Abschnitt [Erstellen Ihres GitHub-Projekts](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) in Erste Schritte.
