---
title: Entfernen von Beispielen aus einem AEM Maven-Projekt
description: Erfahren Sie, wie Sie Beispielcode bereinigen und aus einem AEM entfernen, das vom Projektarchetyp AEM generiert wurde.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
source-git-commit: e8b3bcaeee40b4bfd4f967f929ad664e8d168cb0
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 10%

---


# Entfernen von Beispielen aus einem AEM Maven-Projekt

Erfahren Sie, wie Sie generierten Beispielcode aus einem AEM Projekt bereinigen und entfernen, der vom Projektarchetyp AEM generiert wurde.

>[!VIDEO](https://video.tv.adobe.com/v/337263/?quality=12&learn=on)


## Ressourcen

+ [AEM Maven-Projektarchetyp](https://github.com/adobe/aem-project-archetype)

## Befehle

Die folgenden Befehle können ausgeführt werden, um die generierten Beispieldateien aus dem AEM Maven-Projekt zu entfernen:

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

Entfernen Sie die `<helloworld>` Komponenteninstanzdefinition aus:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
