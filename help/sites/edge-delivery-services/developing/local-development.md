---
title: Einrichten einer lokalen Entwicklungsumgebung für Edge Delivery Services
description: Finden Sie heraus, wie Sie eine lokale Entwicklungsumgebung für Edge Delivery Services einrichten.
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 169
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '87'
ht-degree: 100%

---

# Einrichten einer lokalen Entwicklungsumgebung

Finden Sie heraus, wie Sie eine lokale Entwicklungsumgebung für die Entwicklung für Edge Delivery Services einrichten.

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## Im Video beschriebene Schritte

1. Installieren Sie die AEM-CLI

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. Ändern Sie das Verzeichnis in Ihr Projektverzeichnis. Bei diesem Projektverzeichnis muss es sich um ein Git-Repository handeln, das auf der [AEM-Textbausteinvorlage](https://github.com/adobe/aem-boilerplate) basiert.

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. Führen Sie die AEM-CLI aus, um die lokale AEM-Instanz zu starten.

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. Öffnen Sie im Webbrowser die Website http://localhost:3000/, um Ihre AEM-Website anzuzeigen.
