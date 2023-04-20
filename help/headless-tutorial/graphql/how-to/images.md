---
title: Verwenden optimierter Bilder mit AEM Headless
description: Erfahren Sie, wie Sie optimierte Bild-URLs mit AEM Headless anfordern.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 2096c207ce14985b550b055ea0f51451544c085c
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 31%

---

# Optimierte Bilder mit AEM Headless {#images-with-aem-headless}

Bilder sind ein wichtiger Aspekt in der [Entwicklung vielfältiger, überzeugender AEM Headless-Erlebnisse](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=de). AEM Headless unterstützt die Verwaltung von Bild-Assets und deren optimierte Bereitstellung.

Inhaltsfragmente, die bei der Inhaltsmodellierung in AEM Headless verwendet werden, verweisen häufig auf Bild-Assets, die für die Anzeige im Headless-Erlebnis vorgesehen sind. GraphQL-Abfragen können in AEM geschrieben werden, um abhängig davon, von wo aus ein Bild referenziert wird, URLs zu Bildern bereitzustellen.

Die `ImageRef` Der Typ verfügt über vier URL-Optionen für Inhaltsverweise:

+ `_path` ist der referenzierte Pfad in AEM und enthält keinen AEM-Ursprung (Host-Namen)
+ `_dynamicUrl` ist die vollständige URL zum bevorzugten, Web-optimierten Bild-Asset.
   + Die `_dynamicUrl` enthält keine AEM. Daher muss die Domäne (AEM Author- oder AEM Publish-Dienst) von der Clientanwendung bereitgestellt werden.
+ `_authorUrl` ist die vollständige URL zum Bild-Asset in AEM Author
   + [AEM-Autor](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=de) kann verwendet werden, um eine Vorschau der Headless-Anwendung bereitzustellen.
+ `_publishUrl` ist die vollständige URL zum Bild-Asset in AEM Publish
   + [AEM-Veröffentlichung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=de) ist normalerweise der Ort, über den die Produktionsbereitstellung der Headless-Anwendung Bilder anzeigt.

Die `_dynamicUrl` ist die bevorzugte URL für Bild-Assets und sollte die Verwendung von `_path`, `_authorUrl`und `_publishUrl` wann immer möglich.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Bilder mit AEM Headless"
>abstract="Erfahren Sie, wie AEM Headless die Verwaltung von Bild-Assets und deren optimierte Bereitstellung unterstützt."

## Inhaltsfragmentmodell

Stellen Sie sicher, dass das Inhaltsfragment-Feld, das die Bildreferenz enthält, vom Datentyp __Inhaltsreferenz__ ist.

Sie können die Feldtypen im [Inhaltsfragmentmodell](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=de) überprüfen, indem Sie das Feld auswählen und die Registerkarte __Eigenschaften__ rechts anzeigen.

![Inhaltsfragmentmodell mit Inhaltsreferenz zu einem Bild](./assets/images/content-fragment-model.jpeg)

## GraphQL-persistierte Abfrage

Geben Sie in der GraphQL-Abfrage das Feld als `ImageRef` Typ eingeben und die `_dynamicUrl` -Feld. Beispielsweise können Sie ein Abenteuer im [WKND-Site-Projekt](https://github.com/adobe/aem-guides-wknd) , einschließlich der Bild-URL für Bild-Asset-Verweise in `primaryImage` -Feld kann mit einer neuen persistenten Abfrage durchgeführt werden `wknd-shared/adventure-image-by-path` definiert als:

```graphql
query($path: String!, $assetTransform: AssetTransform!) {
  adventureByPath(
    _path: $path
    _assetTransform: $assetTransform
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### Abfragevariablen

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "assetTransform": { "format": "JPG", "quality": 80, "preferWebp": true}
}
```

Die Variable `$path`, die im Filter `_path` verwendet wird, erfordert den vollständigen Pfad zum Inhaltsfragment (zum Beispiel `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

Die `_assetTransform` definiert, wie die `_dynamicUrl` wird erstellt, um die Darstellung des bereitgestellten Bildes zu optimieren. Web-optimierte Bild-URLs können auch auf dem Client angepasst werden, indem die Abfrageparameter der URL geändert werden.

| GraphQL-Parameter | URL-Parameter | Beschreibung | Erforderlich | GraphQL-Variablenwerte | URL-Parameterwerte | Beispiel für eine GraphQL-Variable | Beispiel-URL-Parameter |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:---|:--|
| `format` | `format` | Das Format des Bild-Assets. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | Nicht zutreffend | `{ format: JPG }` | Nicht zutreffend |
| `seoName` | Nicht zutreffend | Name des Dateisegments in URL. Wenn kein Bild-Asset-Name angegeben wurde, wird verwendet. | ✘ | alphanumerisch, `-`oder `_` | Nicht zutreffend | `{ seoName: "bali-surf-camp" }` | Nicht zutreffend |
| `crop` | `crop` | Aus dem Bild entnommener Zuschnittrahmen muss innerhalb der Bildgröße liegen | ✘ | Positive Ganzzahlen, die einen Zuschnittbereich innerhalb der Grenzen der ursprünglichen Bildabmessungen definieren | Kommagetrennte Zeichenfolge numerischer Koordinaten `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `{ crop: { xOrigin: 10, yOrigin: 20, width: 300, height: 400} }` | `?crop=10,20,300,400` |
| `size` | `size` | Größe des Ausgabebilds (sowohl Höhe als auch Breite) in Pixel. | ✘ | Positive Ganzzahlen | Kommagetrennte positive Ganzzahlen in der Reihenfolge `<WIDTH>,<HEIGHT>` | `{ size: { width: 1200, height: 800 } }` | `?size=1200,800` |
| `rotation` | `rotate` | Drehung des Bildes in Grad. | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `{ rotation: R90 }` | `?rotate=90` |
| `flip` | `flip` | Spiegeln Sie das Bild. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `{ flip: horizontal }` | `?flip=h` |
| `quality` | `quality` | Bildqualität in Prozent der Originalqualität. | ✘ | 1-100 | 1-100 | `{ quality: 80 }` | `?quality=80` |
| `width` | `width` | Breite des Ausgabebilds in Pixel. Wann `size` bereitgestellt wird `width` wird ignoriert. | ✘ | Positive Ganzzahl | Positive Ganzzahl | `{ width: 1600 }` | `?width=1600` |
| `preferWebP` | `preferwebp` | Wenn `true` und AEM eine WebP bereitstellen, wenn der Browser sie unterstützt, unabhängig von der `format`. | ✘ | `true`, `false` | `true`, `false` | `{ preferWebp: true }` | `?preferwebp=true` |

## GraphQL-Antwort

Die resultierende JSON-Antwort enthält die angeforderten Felder, die die Web-optimierte URL zu den Bild-Assets enthalten.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&quality=80"
        }
      }
    }
  }
}
```

Verwenden Sie zum Laden des Web-optimierten Bildes des referenzierten Bildes in Ihrer Anwendung das `_dynamicUrl` des `primaryImage` als Quell-URL des Bildes.

In React sieht die Anzeige eines Web-optimierten Bildes aus AEM Publish wie folgt aus:

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Denken Sie daran, `_dynamicUrl` enthält nicht die AEM-Domäne. Daher müssen Sie den gewünschten Ursprung für die aufzulösende Bild-URL angeben.

### Responsive URLs

Das obige Beispiel zeigt die Verwendung eines Bildes in einer Größe. In Web-Erlebnissen sind jedoch häufig responsive Bildsets erforderlich. Responsive Bilder können mithilfe von [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) oder [Bildelement](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). Das folgende Codefragment zeigt die Verwendung der `_dynamicUrl` als Grundlage verwenden und verschiedene Breitenparameter anhängen, um unterschiedliche responsive Ansichten zu ermöglichen. Die `width` -Abfrageparameter verwendet werden, aber der Client kann weitere Abfrageparameter hinzufügen, um das Bild-Asset entsprechend seinen Anforderungen weiter zu optimieren.

```javascript
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

### React-Beispiel

Erstellen wir eine einfache React-Anwendung, die weboptimierte Bilder anzeigt, die folgende [responsiven Bildmustern](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Es gibt zwei Hauptmuster für responsive Bilder:

+ [Element mit srcset importieren](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) für höhere Leistung
+ [Bildelement](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) für die Entwurfskontrolle

#### Element mit srcset importieren

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Elemente mit srcset importieren](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) werden mit dem `sizes` -Attribut verwenden, um verschiedene Bild-Assets für unterschiedliche Bildschirmgrößen bereitzustellen. Img-Sets sind nützlich, wenn Sie verschiedene Bild-Assets für unterschiedliche Bildschirmgrößen bereitstellen.

#### Bildelement

[Bildelement](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) werden mit mehreren `source` -Elemente verwenden, um verschiedene Bild-Assets für unterschiedliche Bildschirmgrößen bereitzustellen. Bildelemente sind nützlich, wenn Sie verschiedene Bilddarstellungen für unterschiedliche Bildschirmgrößen bereitstellen.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

#### Beispielcode

Diese einfache React-App verwendet [AEM Headless-SDK](./aem-headless-sdk.md) , um AEM Headless-APIs für einen Adventure-Inhalt abzufragen, und zeigt das Web-optimierte Bild mit [img-Element mit srcset](#img-element-with-srcset) und [Musterelement](#picture-element). Die `srcset` und `sources` benutzerspezifische `setParams` Funktion zum Anhängen des Weboptimierten Abfrageparameters für die Bereitstellung an die `_dynamicUrl` des Bildes, ändern Sie also die bereitgestellte Bilddarstellung entsprechend den Anforderungen des Webclients.

Die Abfrage in AEM wird über den benutzerdefinierten React-Hook [useAdventureByPath, der das AEM Headless SDK verwendet](./aem-headless-sdk.md#graphql-persisted-queries), ausgeführt.

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} dynamicUrl the base dynamic URL for the web-optimized image
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setParams(dynamicUrl, params) {
    let url = new URL(dynamicUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { assetTransform: { format: "JPG", preferWebp: true } }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setParams(dynamicUrl, { width: 1000 })}
        srcSet={
            `${setParams(dynamicUrl, { width: 1000 })} 1000w,
             ${setParams(dynamicUrl, { width: 1600 })} 1600w,
             ${setParams(dynamicUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setParams(dynamicUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setParams(dynamicUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setParams(dynamicUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
