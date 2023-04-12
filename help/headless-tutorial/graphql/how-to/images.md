---
title: Verwenden von Bildern mit AEM Headless
description: Erfahren Sie, wie Sie Referenz-URLs für Bildinhalte anfordern und benutzerdefinierte Ausgabeformate mit AEM Headless verwenden.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: ae49fb45db6f075a34ae67475f2fcc5658cb0413
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 97%

---

# Bilder mit AEM Headless {#images-with-aem-headless}

Bilder sind ein wichtiger Aspekt in der [Entwicklung vielfältiger, überzeugender AEM Headless-Erlebnisse](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=de). AEM Headless unterstützt die Verwaltung von Bild-Assets und deren optimierte Bereitstellung.

Inhaltsfragmente, die bei der Inhaltsmodellierung in AEM Headless verwendet werden, verweisen häufig auf Bild-Assets, die für die Anzeige im Headless-Erlebnis vorgesehen sind. GraphQL-Abfragen können in AEM geschrieben werden, um abhängig davon, von wo aus ein Bild referenziert wird, URLs zu Bildern bereitzustellen.

Der Typ `ImageRef` verfügt über drei URL-Optionen für Inhaltsverweise:

+ `_path` ist der referenzierte Pfad in AEM und enthält keinen AEM-Ursprung (Host-Namen)
+ `_authorUrl` ist die vollständige URL zum Bild-Asset in AEM Author
   + [AEM-Autor](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=de) kann verwendet werden, um eine Vorschau der Headless-Anwendung bereitzustellen.
+ `_publishUrl` ist die vollständige URL zum Bild-Asset in AEM Publish
   + [AEM-Veröffentlichung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=de) ist normalerweise der Ort, über den die Produktionsbereitstellung der Headless-Anwendung Bilder anzeigt.

Die Verwendung der Felder erfolgt idealerweise nach folgenden Kriterien:

| ImageRef-Felder | Von AEM bereitgestellte Client-Web-Anwendung | Client-App-Abfragen in AEM Author | Client-App-Abfragen in AEM Publish |
|--------------------|:------------------------------:|:-----------------------------:|:------------------------------:|
| `_path` | ✔ | ✔ (App muss Host in URL spezifizieren) | ✔ (App muss Host in URL spezifizieren) |
| `_authorUrl` | ✘ | ✔ | ✘ |
| `_publishUrl` | ✘ | ✘ | ✔ |

Die Verwendung von `_authorUrl` und `_publishUrl` sollte dem AEM-GraphQL-Endpunkt entsprechen, der für die Quelle der GraphQL-Antwort genutzt wird.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Bilder mit AEM Headless"
>abstract="Erfahren Sie, wie AEM Headless die Verwaltung von Bild-Assets und deren optimierte Bereitstellung unterstützt."

## Inhaltsfragmentmodell

Stellen Sie sicher, dass das Inhaltsfragment-Feld, das die Bildreferenz enthält, vom Datentyp __Inhaltsreferenz__ ist.

Sie können die Feldtypen im [Inhaltsfragmentmodell](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=de) überprüfen, indem Sie das Feld auswählen und die Registerkarte __Eigenschaften__ rechts anzeigen.

![Inhaltsfragmentmodell mit Inhaltsreferenz zu einem Bild](./assets/images/content-fragment-model.jpeg)

## GraphQL-persistierte Abfrage

Geben Sie in der GraphQL-Abfrage das Feld als Typ `ImageRef` zurück und fordern Sie die entsprechenden Felder `_path`, `_authorUrl` oder `_publishUrl` an, die von Ihrer Anwendung benötigt werden. Beispielsweise können Sie ein Abenteuer im [WKND-Site-Projekt](https://github.com/adobe/aem-guides-wknd) , einschließlich der Bild-URL für Bild-Asset-Verweise in `primaryImage` -Feld kann mit einer neuen persistenten Abfrage durchgeführt werden `wknd-shared/adventure-image-by-path` definiert als:

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

Die Variable `$path`, die im `_path`-Filter verwendet wird, erfordert den vollständigen Pfad zum Inhaltsfragment (zum Beispiel `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

## GraphQL-Antwort

Die resultierende JSON-Antwort beinhaltet die angeforderten Felder, die die URLs zu den Bild-Assets enthalten.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
          "_authorUrl": "https://author-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
          "_publishUrl": "https://publish-p123-e789.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"
        }
      }
    }
  }
}
```

Verwenden Sie das entsprechende Feld, um das referenzierte Bild in Ihre Anwendung zu laden – `_path`, `_authorUrl` oder `_publishUrl` des `adventurePrimaryImage` als Quell-URL des Bildes.

Die Domains der `_authorUrl` und `_publishUrl` werden automatisch von AEM as a Cloud Service mithilfe des [Externalizers](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/externalizer.html?lang=de) definiert.

In React sieht die Anzeige des Bildes über AEM Publish wie folgt aus:

```html
<img src={ data.adventureByPath.item.primaryImage._publishUrl } />
```

## Bilddarstellungen

Bild-Assets unterstützen anpassbare [Ausgabeformate](../../../assets/authoring/renditions.md), die alternative Darstellungen des ursprünglichen Assets sind. Benutzerdefinierte Ausgabeformate können bei der Optimierung eines Headless-Erlebnisses helfen. Statt des ursprünglichen Bild-Assets, bei dem es sich häufig um eine große, hochauflösende Datei handelt, können optimierte Ausgabeformate von der Headless-Anwendung angefordert werden.

### Erstellen von Ausgabedarstellungen

Administrierende von AEM Assets definieren die benutzerdefinierten Ausgabedarstellungen mithilfe von Verarbeitungsprofilen. Die Verarbeitungsprofile können dann direkt auf bestimmte Ordnerhierarchien oder Assets angewendet werden, um die Ausgabedarstellungen für diese Assets zu generieren.

#### Verarbeitungsprofile

Spezifikationen für Asset-Ausgabedarstellungen werden von AEM Assets-Administrierenden in [Verarbeitungsprofilen](../../../assets/configuring/processing-profiles.md) definiert.

Erstellen oder aktualisieren Sie ein Verarbeitungsprofil und fügen Sie Ausgabedarstellungsdefinitionen für die Bildgrößen hinzu, die für die Headless-Anwendung erforderlich sind. Ausgabedarstellungen können beliebig benannt werden, der Name sollte jedoch verständlich sein.

![Für AEM-Headless-optimierte Ausgabedarstellungen](./assets/images/processing-profiles.png)

In diesem Beispiel werden drei Ausgabedarstellungen erstellt:

| Name der Ausgabedarstellung | Erweiterung | Max. Breite |
|-----------------------|:---------:|----------:|
| web-optimiert groß | webp | 1200 px |
| web-optimiert mittel | webp | 900 px |
| web-optimiert klein | webp | 600 px |

Die in der obigen Tabelle aufgeführten Attribute sind wichtig:

+ __Name der Ausgabedarstellung__ wird verwendet, um die Ausgabedarstellung anzufordern.
+ __Erweiterung__ ist die Erweiterung, die zum Anfordern des __Namens der Ausgabedarstellung__ verwendet wird. `webp`-Ausgabedarstellungen werden empfohlen, da diese für die Web-Bereitstellung optimiert sind.
+ __Max. Breite__ wird verwendet, um den Entwickler bzw. die Entwicklerin darüber zu informieren, welche Ausgabedarstellung basierend auf ihrer Verwendung in der Headless-Anwendung genutzt werden soll.

Ausgabedarstellungsdefinitionen hängen von den Anforderungen Ihrer Headless-Anwendung ab. Stellen Sie daher sicher, dass Sie für Ihren Anwendungsfall den optimalen Satz an Ausgabedarstellungen definieren und ihn entsprechend der Verwendungsart semantisch benennen.

#### Erneutes Verarbeiten von Assets{#reprocess-assets}

Nachdem das Verarbeitungsprofil erstellt (oder aktualisiert) wurde, verarbeiten Sie die Assets erneut, um die neuen, im Verarbeitungsprofil definierten Ausgabedarstellungen zu erstellen. Neue Ausgabedarstellungen sind erst vorhanden, wenn Assets mit dem Verarbeitungsprofil verarbeitet werden.

+ Nach Möglichkeit sollten Sie [das Verarbeitungsprofil einem Ordner zuweisen](../../../assets/configuring//processing-profiles.md), sodass alle neuen Assets, die in diesen Ordner hochgeladen werden, automatisch die Ausgabedarstellungen erstellen. Bestehende Assets müssen mithilfe des unten stehenden Ad-hoc-Ansatzes erneut verarbeitet werden.

+ Oder ad hoc, indem Sie einen Ordner oder ein Asset auswählen, __Assets erneut verarbeiten__ und den neuen Verarbeitungsprofil-Namen auswählen.

   ![Ad-hoc-Wiederverarbeitung von Assets](./assets/images/ad-hoc-reprocess-assets.jpg)

#### Überprüfen von Ausgabedarstellungen

Ausgabedarstellungen können überprüft werden, indem [die Ausgabedarstellungsansicht eines Assets geöffnet wird](../../../assets/authoring/renditions.md) und die neuen Ausgabedarstellungen, die in der Vorschau angezeigt werden sollen, in der Ausgabedarstellungsleiste ausgewählt werden. Wenn Ausgabedarstellungen fehlen, [stellen Sie sicher, dass die Assets mithilfe des Verarbeitungsprofils verarbeitet werden](#reprocess-assets).

![Überprüfen von Ausgabedarstellungen](./assets/images/review-renditions.png)

#### Veröffentlichen von Assets

Stellen Sie sicher, dass die Assets mit den neuen Ausgabedarstellungen [(erneut) veröffentlicht](../../../assets/sharing/publish.md) wurden, sodass die neuen Ausgabedarstellungen in der AEM-Veröffentlichungsinstanz verfügbar sind.

### Zugriff auf Ausgabedarstellungen

Auf Ausgabedarstellungen können Sie direkt zugreifen, indem Sie die im Verarbeitungsprofil definierten __Ausgabedarstellungsnamen__ und __Ausgabedarstellungserweiterungen__ an die URL des Assets anhängen.

| Asset-URL | Unterpfad für Ausgabedarstellungen | Name der Ausgabedarstellung | Ausgabedarstellungserweiterung |  | Ausgabedarstellungs-URL |
|-----------|:------------------:|:--------------:|--------------------:|:--:|---|
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr_content/renditions/ | web-optimiert groß | .webp | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/web-optimized-large.webp |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr_content/renditions/ | web-optimiert mittel | .webp | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/web-optimized-medium.webp |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr_content/renditions/ | web-optimiert klein | .webp | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/web-optimized-small.webp |

{style="table-layout:auto"}

### GraphQL-Abfrage{#renditions-graphl-query}

AEM GraphQL benötigt eine zusätzliche Syntax, um Bildausgabedarstellungen anzufordern. Stattdessen werden [Bilder auf die übliche Weise abgefragt](#images-graphql-query) und die gewünschte Ausgabedarstellung wird im Code angegeben. Sie müssen [sicherstellen, dass die von der Headless-Anwendung verwendeten Bild-Assets über identisch benannte Ausgabedarstellungen verfügen](#reprocess-assets).

### React-Beispiel

Erstellen wir eine einfache React-App, die drei Ausgabedarstellungen eines Einzelbild-Assets anzeigt: web-optimiert klein, web-optimiert mittel und web-optimiert groß.

![React-Beispiel für Bild-Asset-Ausgabedarstellungen](./assets/images/react-example-renditions.jpg)

#### Erstellen einer Bildkomponente{#react-example-image-component}

Erstellen Sie eine React-Komponente, die die Bilder rendert. Diese Komponente akzeptiert vier Eigenschaften:

+ `assetUrl`: Die Bild-Asset-URL, wie in der Antwort der GraphQL-Abfrage angegeben.
+ `renditionName`: Den Namen der zu ladenden Ausgabedarstellung.
+ `renditionExtension`: Die Erweiterung der zu ladenden Ausgabedarstellung.
+ `alt`: Den Alt-Text für das Bild – Barrierefreiheit ist wichtig!

Diese Komponente erstellt die [URL für die Ausgabedarstellung mit dem Format, das unter __Zugreifen auf Ausgabedarstellungen__](#access-renditions) erklärt wird. Ein `onError`-Handler wird gesetzt, um das ursprüngliche Asset anzuzeigen, falls die Ausgabedarstellung fehlt.

In diesem Beispiel wird für den Fall, dass keine Ausgabedarstellung vorhanden ist, die ursprüngliche Asset-URL als Fallback im `onError`-Handler verwendet.

```javascript
// src/Image.js

export default function Image({ assetUrl, renditionName, renditionExtension, alt }) {
  // Construct the rendition Url in the format:
  //   <ASSET URL>/_jcr_content/renditions<RENDITION NAME>.<RENDITION EXTENSION>
  const renditionUrl = `${assetUrl}/_jcr_content/renditions/${renditionName}.${renditionExtension}`;

  // Load the original image asset in the event the named rendition is missing
  const handleOnError = (e) => { e.target.src = assetUrl; }

  return (
    <>
      <img src={renditionUrl} 
            alt={alt} 
            onError={handleOnError}/>
    </>
  );
}
```

#### Definieren der `App.js`{#app-js}

Diese einfache `App.js` führt in AEM eine Abfrage zu einem Adventure-Bild aus und zeigt dann die drei Ausgabedarstellungen dieses Bildes an: web-optimiert klein, web-optimiert mittel und web-optimiert groß.

Die Abfrage in AEM wird über den benutzerdefinierten React-Hook [useAdventureByPath, der das AEM Headless SDK verwendet](./aem-headless-sdk.md#graphql-persisted-queries), ausgeführt.

Die Ergebnisse der Abfrage und die spezifischen Parameter der Ausgabedarstellung werden an die [Bild-React-Komponente](#react-example-image-component) übergeben.

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'
import Image from "./Image";

function App() {

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  let { data, error } = useAdventureByPath("/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp");

  // Wait for GraphQL to provide data
  if (!data) { return <></> }

  return (
    <div className="app">
      
      <h2>Small rendition</h2>
      {/* Render the web-optimized-small rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="web-optimized-small"
        renditionExtension="webp"
        alt={data.adventureByPath.item.title}
      />

      <hr />

      <h2>Medium rendition</h2>
      {/* Render the web-optimized-medium rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="web-optimized-medium"
        renditionExtension="webp"
        alt={data.adventureByPath.item.title}
      />

      <hr />

      <h2>Large rendition</h2>
      {/* Render the web-optimized-large rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="web-optimized-large"
        renditionExtension="webp"
        alt={data.adventureByPath.item.title}
      />
    </div>
  );
}

export default App;
```
