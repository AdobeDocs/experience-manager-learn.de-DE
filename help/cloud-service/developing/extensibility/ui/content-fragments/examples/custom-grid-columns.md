---
title: Benutzerdefinierte Rasterspalten in der Inhaltsfragmentkonsole
description: Erfahren Sie, wie Sie der Inhaltsfragmentkonsole eine benutzerdefinierte Rasterspalte hinzufügen können.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13453
thumbnail: KT-13453.jpeg
doc-type: article
last-substantial-update: 2023-06-07T00:00:00Z
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 2%

---


# Benutzerdefinierte Rasterspalten

![Benutzerdefinierte Rasterspalte der Inhaltsfragmentkonsole](./assets/custom-grid-columns/hero.png){align="center"}

Benutzerdefinierte Rasterspalten können der Inhaltsfragmentkonsole mithilfe der  `contentFragmentGrid` Erweiterungspunkt. In diesem Beispiel wird gezeigt, wie eine benutzerdefinierte Spalte hinzugefügt wird, die die Seite &quot;Inhaltsfragmente&quot;basierend auf dem Datum der letzten Änderung in einem für Menschen lesbaren Format anzeigt.

## Erweiterungspunkt

Dieses Beispiel erstreckt sich auf den Erweiterungspunkt `contentFragmentGrid` , um der Inhaltsfragmentkonsole eine benutzerdefinierte Spalte hinzuzufügen.

| AEM Benutzeroberfläche erweitert | Erweiterungspunkt |
| ------------------------ | --------------------- | 
| [Inhaltsfragment-Konsole](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/?lang=de) | [Rasterspalten](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/) |

## Beispielerweiterung

Im folgenden Beispiel wird eine benutzerdefinierte Spalte erstellt. `Age` , das das Alter des Inhaltsfragments im für Menschen lesbaren Format anzeigt. Das Alter wird ab dem letzten Änderungsdatum des Inhaltsfragments berechnet.

Der Code zeigt, wie die Metadaten des Inhaltsfragments in der Registrierungsdatei der Erweiterung abgerufen werden können und wie der JSON-Inhalt des Inhaltsfragments transformiert werden kann.

In diesem Beispiel wird die [Luxon](https://moment.github.io/luxon/) Bibliothek zur Berechnung des Alters des Inhaltsfragments, installiert über `npm i luxon`.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, der der Route index.html zugeordnet ist, ist der Einstiegspunkt für die AEM Erweiterung und definiert:

+ Der Speicherort der Erweiterung injiziert sich selbst (`contentFragmentGrid`) im AEM Authoring-Erlebnis
+ Die Definition der benutzerdefinierten Spalte im `getColumns()` function
+ Die Werte für jede benutzerdefinierte Spalte, aufgeschlüsselt nach Zeilen

```javascript
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";
import { Duration } from "luxon";

/**
 * Set up a in-memory cache of the custom column data.
 * This is important if the work to contain column data is expensive, such as making HTTP requests to obtain the value.
 * 
 * The caching of computed value is optional, but recommended if the work to compute the column data is expensive.
 */
const COLUMN_AGE_ID = "age";
const cache = {
  [COLUMN_AGE_ID]: {},
};

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        contentFragmentGrid: {
          getColumns() {
            return [
              {
                id: COLUMN_AGE_ID,          // Give the column a unique ID.
                label: "Age",               // The label to display
                render: async function (fragments) {
                  // This function is run for each row in the grid.
                  // The fragments parameter is an array of all the fragments (aka each row) in the grid.
                  const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();

                  // Iterate over each fragment in the grid
                  for (const fragment of fragments) {
                    // Check if a previous pass has computed the value for this fragment.id. If it has, we can skip it.
                    if (!cache[COLUMN_AGE_ID][fragment.id]) {
                      // If the fragment.id has not been computed and cached, then compute the value and cache it.
                      cache[COLUMN_AGE_ID][fragment.id] = await computeAgeColumnValue(fragment, context);
                    }
                  }
                  // Return the populated cache of the custom column data.
                  return cache[COLUMN_AGE_ID];
                }
              },
              // Add other custom columns here...
            ];
          },
        },
      },
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

async function computeAgeColumnValue(fragment, context) {
  // Various data is available in the sharedContext, such as the AEM host, the IMS token, and the fragment itself.
  
  // Accessing AEM APIs requires the IMS token, which is available in the sharedContext, and also supporting CORS configurations deployed to AEM Author.
  //const aemHost = context.aemHost;
  //const accessToken = context.auth.imsToken;
  
  // Get the user's locale to format the value of the column.
  const locale = context.locale;

  // Compute the value of the column, in this case we are computing the age of the fragment from its last modified date.
  const duration = Duration.fromMillis(Date.now() - fragment.modifiedDate).rescale();

  let largestUnit = {};

  if (duration.years > 0) {
    largestUnit = {years: duration.years};
  } else if (duration.months > 0) {
    largestUnit = {months: duration.months};
  } else if (duration.days > 0) {
    largestUnit = {days: duration.days};
  } else if (duration.hours > 0) {
    largestUnit = {hours: duration.hours};
  } else if (duration.minutes > 0) {
    largestUnit = {minutes: duration.minutes};
  } else if (duration.seconds > 0) {
    largestUnit = {seconds: duration.seconds};
  }

  // Convert the largest unit of the age to human readable format.
  const columnValue = Duration.fromObject(largestUnit, {locale: locale}).toHuman();

  // Return the value of the column.
  return columnValue;
}

export default ExtensionRegistration;
```

#### Inhaltsfragmentdaten

Die `render(..)` -Methode in `getColumns()` wird ein Array von Fragmenten übergeben. Jedes Objekt im Array stellt eine Zeile im Raster dar und enthält die folgenden Metadaten zum Inhaltsfragment. Diese Metadaten können für beliebte benutzerdefinierte Spalten im Raster verwendet werden.


```javascript
render: async function (fragments) {
    for (const fragment of fragments) {
        // An example value from this console log is displayed below.
        console.log(fragment);
    }
}
```

Beispiel für ein Inhaltsfragment-JSON, das als Element der `fragments` -Parameter in der `render(..)` -Methode.

```json
{
    "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
    "name": "alaskan-adventures",
    "title": "Alaskan Adventures",
    "status": "draft",
    "statusPreview": null,
    "model": {
        "name": "Article",
        "id": "/conf/wknd-shared/settings/dam/cfm/models/article"
    },
    "folderId": "/content/dam/wknd-shared/en/magazine/alaska-adventure",
    "folderName": "Alaska Adventure",
    "createdBy": "admin",
    "createdDate": 1684875665786,
    "modifiedBy": "admin",
    "modifiedDate": 1654104774889,
    "publishedBy": "",
    "publishedDate": 0,
    "locale": "",
    "main": true,
    "translations": {
        "locale": "en",
        "languageCopies": [
            {
                "path": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
                "title": "Alaskan Adventures",
                "locale": "en",
                "model": "Article",
                "status": "DRAFT",
                "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures"
            }
        ]
    },
    "transientStatus": {},
    "extensions": {
        "age": "1 year old"
    }
}
```

Wenn zum Ausfüllen der benutzerdefinierten Spalte andere Daten erforderlich sind, können HTTP-Anfragen an die AEM-Autoreninstanz gesendet werden, um die Daten abzurufen.

>[!IMPORTANT]
>
> Stellen Sie sicher, dass die AEM-Autoreninstanz so konfiguriert ist, dass [Cross-Origin-Anforderungen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html) aus den Quellen, in denen die AppBuilder-App ausgeführt wird. Zulässige Ursprünge umfassen `https://localhost:9080`, der AppBuilder-Staging-Herkunft und der AppBuilder-Produktionsursprung.
>
> Alternativ kann die Erweiterung eine benutzerdefinierte [AppBuilder-Aktion](../../runtime-action.md) , der die Anfrage an die AEM-Autoreninstanz im Namen der Erweiterung sendet.


```javascript
const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();
...
// Fetch the Content Fragment JSON from AEM Author using the AEM Assets HTTP API
const response = await fetch(`${context.aemHost}${fragment.id.slice('/content/dam'.length)}.json`, {
    headers: {
        'Authorization': `Bearer ${context.auth.imsToken}`
    }
})
...
```

#### Spaltendefinition

Das Ergebnis der render-Methode ist ein JavaScript-Objekt, dessen Schlüssel der Pfad des Inhaltsfragments (oder der `fragment.id`) und der Wert der Wert ist, der in der Spalte angezeigt werden soll.

Die Ergebnisse dieser Erweiterung beispielsweise für die `age` -Spalte sind:

```json
{
    "/content/dam/wknd-shared/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks": "22 minutes",
    "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp": "1 hour",
    "/content/dam/wknd-shared/en/magazine/western-australia/western-australia-by-camper-van": "1 hour",
    "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/climbing-new-zealand": "10 months",
    "/content/dam/wknd-shared/en/magazine/skitouring/skitouring": "1 year",
    "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland": "1 year",
    "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures": "1 year",
    "/content/dam/wknd-shared/en/magazine/arctic-surfing/aloha-spirits-in-northern-norway": "1 year",
    "/content/dam/wknd-shared/en/magazine/san-diego-surf-spots/san-diego-surfspots": "1 year",
    "/content/dam/wknd-shared/en/magazine/fly-fishing-amazon/fly-fishing": "1 year",
    "/content/dam/wknd-shared/en/adventures/napa-wine-tasting/napa-wine-tasting": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah": "1 year",
    "/content/dam/wknd-shared/en/adventures/gastronomic-marais-tour/gastronomic-marais-tour": "1 year",
    "/content/dam/wknd-shared/en/adventures/tahoe-skiing/tahoe-skiing": "1 year",
    "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica": "1 year",
    "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking": "1 year",
    "/content/dam/wknd-shared/en/adventures/whistler-mountain-biking/whistler-mountain-biking": "1 year",
    "/content/dam/wknd-shared/en/adventures/colorado-rock-climbing/colorado-rock-climbing": "1 year",
    "/content/dam/wknd-shared/en/adventures/ski-touring-mont-blanc/ski-touring-mont-blanc": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany": "1 year",
    "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling": "1 year",
    "/content/dam/wknd-shared/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming": "1 year",
    "/content/dam/wknd-shared/en/adventures/riverside-camping-australia/riverside-camping-australia": "1 year",
    "/content/dam/wknd-shared/en/contributors/ian-provo": "1 year",
    "/content/dam/wknd-shared/en/contributors/sofia-sj-berg": "1 year",
    "/content/dam/wknd-shared/en/contributors/justin-barr": "1 year",
    "/content/dam/wknd-shared/en/contributors/jake-hammer": "1 year",
    "/content/dam/wknd-shared/en/contributors/jacob-wester": "1 year",
    "/content/dam/wknd-shared/en/contributors/stacey-roswells": "1 year",
    "/content/dam/wknd-shared/en/contributors/kumar-selveraj": "1 year"
}
```