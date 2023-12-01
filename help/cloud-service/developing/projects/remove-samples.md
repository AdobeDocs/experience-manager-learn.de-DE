---
title: Entfernen von Beispielen aus einem AEM-Maven-Projekt
description: Erfahren Sie, wie Sie Beispiel-Code bereinigen und aus einem vom AEM-Projektarchetyp generierten AEM-Projekt entfernen.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 100%

---

# Entfernen von Beispielen aus einem AEM-Maven-Projekt

Erfahren Sie, wie Sie generierten Beispiel-Code bereinigen und aus einem vom AEM-Projektarchetyp erstellten AEM-Projekt entfernen.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Ressourcen

+ [AEM-Maven-Projektarchetyp](https://github.com/adobe/aem-project-archetype)

## Befehle

Die folgenden Befehle können ausgeführt werden, um die generierten Beispieldateien aus dem AEM-Maven-Projekt zu entfernen:

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## Bearbeitungen

Entfernen Sie `<div class="helloworld" ...></div>` aus:

```
ui.frontend/src/main/webpack/static/index.html
```

Entfernen Sie die Komponenten-Instanzdefinition `<helloworld>` aus:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
