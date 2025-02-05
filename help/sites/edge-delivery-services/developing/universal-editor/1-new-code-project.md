---
title: Erstellen eines Code-Projekts
description: Erstellen Sie ein Code-Projekt für Edge Delivery Services, das mit dem universellen Editor bearbeitet werden kann.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 176be4209409763b69ceff5e893d0e857efa6d30
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 71%

---

# Erstellen eines Edge Delivery Services-Code-Projekts

Verwenden Sie zum Erstellen von AEM-Websites für Edge Delivery Services und den universellen Editor die Projektvorlage [Adobe AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Diese Vorlage erstellt ein neues Code-Projekt, das das CSS und JavaScript enthält, die zum Erstellen des Website-Erlebnisses verwendet werden. Sie erstellt ein neues GitHub-Repository und lädt es mit dem Textbaustein-Code und der Konfiguration von Adobe, wodurch eine solide Grundlage für Ihr AEM-Website-Projekt geschaffen wird.

Denken Sie daran, dass [von Edge Delivery Services bereitgestellte AEM-Websites](https://experienceleague.adobe.com/de/docs/experience-manager-learn/sites/edge-delivery-services/overview) nur Client-seitigen (Browser-)Code enthalten. Website-Code wird in den AEM-Autoren- oder Veröffentlichungs-Services nicht ausgeführt.

![Neues Edge Delivery Services-Projekt](./assets/1-new-project/new-project.png)

Befolgen Sie die [detaillierten Schritte in der Dokumentation](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) zum Erstellen eines Edge Delivery Services-Code-Projekts, dessen Inhalt im universellen Editor bearbeitbar ist.  Nachfolgend finden Sie eine zusammenfassende Liste der Schritte, einschließlich der in diesem Tutorial verwendeten Werte.

1. **Richten Sie ein GitHub-Konto ein.** Wenn Sie ein Projekt für Ihre Organisation erstellen, stellen Sie sicher, dass die Organisation über ein GitHub-Konto verfügt und Sie Mitglied sind.
2. **Erstellen Sie ein neues Code-Projekt** mithilfe der Projektvorlage [AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Installieren Sie die GitHub-Anwendung „AEM Code Sync“** und gewähren Sie ihr Zugriff auf das Repository. Sie finden die Anwendung [hier](https://github.com/apps/aem-code-sync).
4. **Konfigurieren Sie die Datei`fstab.yaml`** Ihres neuen Projekts so, dass sie auf den richtigen AEM-Autoren-Service verweist.

   * Zum Experimentieren können Sie niedrigere AEM as a Cloud Service-Umgebungen (Staging oder Entwicklung) verwenden. Echte Websites-Implementierungen sollten jedoch so konfiguriert werden, dass sie einen Produktions-AEM-Service verwenden.

5. **Bearbeiten Sie die Datei`paths.json`** Ihres neuen Projekts, um den Pfad des AEM-Autoren-Service dem Stamm Ihrer Website zuzuordnen.

Dieses Git-Repository wird in das Kapitel [Lokale Entwicklungsumgebung](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) geklont, in dem Code entwickelt wird.
