---
title: Komponente des Begrüßungs-Kits erstellen
description: Erstellen Sie eine AEM Siteseite mit Links zum Herunterladen von Assets basierend auf den gesendeten Formulardaten.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '74'
ht-degree: 0%

---

# Komponente des Begrüßungs-Kits

Eine Seitenkomponente wurde erstellt, um die Assets auf der Seite aufzulisten, die vom Endbenutzer heruntergeladen werden können. Die Pfade zu den einzelnen Assets werden in einer Eigenschaft namens **Pfade**. Die gesendeten Formulardaten bestimmen die einzuschließenden Assets.

Der folgende Code listet die Assets auf der Seite auf:

```html
   <p class="cmp-press-kit__press-kit-size">
        Welcome kit contains ${pressKit.assets.size} assets.
    </p>
<ul class="cmp-press-kit__asset-list" data-sly-list.asset="${pressKit.assets}">
    <li class="cmp-press-kit__asset-item">
        <div class="cmp-press-kit__asset " >
            <div class="cmp-press-kit__asset-content">
                <img class="cmp-press-kit__asset-image" src="${asset.path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png" alt="${asset.name}"/>
                <p class="cmp-press-kit__asset-title">${asset.title}</p>
            </div>
            <div class="cmp-press-kit__asset-actions">
                <a class="cmp-press-kit__asset-download-button" href="${asset.path}">Download</a>
            </div>
        </div>
    </li>
</ul>
</sly>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!ready}"></sly>
```


