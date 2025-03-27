---
title: Erstellen eines Code-Projekts
description: Erstellen Sie ein Code-Projekt für Edge Delivery Services, das mit dem universellen Editor bearbeitet werden kann.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# Erstellen eines Edge Delivery Services-Code-Projekts

Verwenden Sie zum Erstellen von AEM-Websites für Edge Delivery Services und den universellen Editor die Projektvorlage [Adobe AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Diese Vorlage erstellt ein neues Code-Projekt, das das CSS und JavaScript enthält, die zum Erstellen des Website-Erlebnisses verwendet werden. Sie erstellt ein neues GitHub-Repository und lädt es mit dem Textbaustein-Code und der Konfiguration von Adobe, wodurch eine solide Grundlage für Ihr AEM-Website-Projekt geschaffen wird.

Denken Sie daran, dass [von Edge Delivery Services bereitgestellte AEM-Websites](https://experienceleague.adobe.com/de/docs/experience-manager-learn/sites/edge-delivery-services/overview) nur Client-seitigen (Browser-)Code enthalten. Website-Code wird in den AEM-Autoren- oder Veröffentlichungs-Services nicht ausgeführt.

![Neues Edge Delivery Services-Projekt](./assets/1-new-project/new-project.png)

Folgen Sie den [ausführlichen Schritten in der Dokumentation](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) zur Erstellung eines Edge Delivery Services-Code-Projekts, dessen Inhalt im universellen Editor bearbeitet werden kann.  Nachfolgend finden Sie eine zusammenfassende Liste der Schritte, einschließlich der in diesem Tutorial verwendeten Werte.

1. **Richten Sie ein GitHub-Konto ein.** Wenn Sie ein Projekt für Ihre Organisation erstellen, stellen Sie sicher, dass die Organisation über ein GitHub-Konto verfügt und Sie Mitglied sind.
2. **Erstellen Sie ein neues Code-Projekt** mithilfe der Projektvorlage [AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Installieren Sie die GitHub-Anwendung „AEM Code Sync“** und gewähren Sie ihr Zugriff auf das Repository. Sie finden die Anwendung [hier](https://github.com/apps/aem-code-sync).
4. **Konfigurieren Sie die Datei`fstab.yaml`** Ihres neuen Projekts so, dass sie auf den richtigen AEM-Autoren-Service verweist.

   * Zum Experimentieren können Sie niedrigere AEM as a Cloud Service-Umgebungen (Staging oder Entwicklung) verwenden. Echte Website-Implementierungen sollten jedoch für die Verwendung eines Produktions-AEM-Service konfiguriert werden.

5. **Bearbeiten Sie die Datei`paths.json`** Ihres neuen Projekts, um den Pfad des AEM-Autoren-Service dem Stamm Ihrer Website zuzuordnen.

Dieses Git-Repository wird im Kapitel [lokale Entwicklungsumgebung](https://experienceleague.adobe.com/de/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) geklont, in dem Code entwickelt wird.
