---
title: Benutzerdefinierte Rasterspalten in der Inhaltsfragmentkonsole
description: Erfahren Sie, wie Sie der Inhaltsfragmentkonsole eine benutzerdefinierte Rasterspalte hinzufügen können.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13453
thumbnail: KT-13453.jpeg
doc-type: article
last-substantial-update: 2023-06-07T00:00:00Z
exl-id: 87143cf9-e932-4ad6-afe2-cce093c520f4
duration: 198
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '406'
ht-degree: 100%

---

# Benutzerdefinierte Rasterspalten

![Benutzerdefinierte Rasterspalte in der Inhaltsfragmentkonsole](./assets/custom-grid-columns/hero.png){align="center"}

Benutzerdefinierte Rasterspalten können der Inhaltsfragmentkonsole mithilfe des Erweiterungspunkts `contentFragmentGrid` hinzugefügt werden. In diesem Beispiel wird gezeigt, wie eine benutzerdefinierte Spalte hinzugefügt wird, über die das Alter der Inhaltsfragmente basierend auf dem letzten Änderungsdatum in einem lesbaren Format angegeben wird.

## Erweiterungspunkt

Dieses Beispiel erstreckt sich auf den Erweiterungspunkt `contentFragmentGrid`, um der Inhaltsfragmentkonsole eine benutzerdefinierte Spalte hinzuzufügen.

| Erweiterte AEM-Benutzeroberfläche | Erweiterungspunkt |
| ------------------------ | --------------------- | 
| [Inhaltsfragment-Konsole](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/?lang=de) | [Rasterspalten](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/) |

## Beispielerweiterung

Im folgenden Beispiel wird die benutzerdefinierte Spalte `Age` erstellt, die das Alter des Inhaltsfragments in einem lesbaren Format anzeigt. Das Alter wird ab dem letzten Änderungsdatum des Inhaltsfragments berechnet.

Der Code zeigt an, wie die Metadaten des Inhaltsfragments in der Registrierungsdatei der Erweiterung abgerufen werden können und wie der JSON-Inhalt des Inhaltsfragments transformiert und exportiert werden kann.

In diesem Beispiel wird die [Luxon](https://moment.github.io/luxon/)-Bibliothek zur Berechnung des Inhaltsfragmentalters über `npm i luxon` installiert.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, die der Route „index.html“ zugeordnet ist, ist der Einstiegspunkt für die AEM-Erweiterung und definiert Folgendes:

+ Speicherort der Erweiterung, der sich selbst (`contentFragmentGrid`) in das AEM-Authoring-Erlebnis einfügt
+ Definition der benutzerdefinierten Spalte in der `getColumns()`-Funktion
+ Werte für die einzelnen benutzerdefinierten Spalten nach Zeile

```javascript
import React from "react";
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

Der Methode `render(..)` in `getColumns()` wird ein Array an Fragmenten weitergegeben. Jedes Objekt im Array stellt eine Zeile im Raster dar und enthält die folgenden Metadaten zum Inhaltsfragment. Diese Metadaten können für beliebte benutzerdefinierte Spalten im Raster verwendet werden.


```javascript
render: async function (fragments) {
    for (const fragment of fragments) {
        // An example value from this console log is displayed below.
        console.log(fragment);
    }
}
```

Beispiel für ein Inhaltsfragment-JSON, das als Element des `fragments`-Parameters in der Methode `render(..)` verfügbar ist:

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

Wenn zum Auffüllen der benutzerdefinierten Spalte andere Daten erforderlich sind, können HTTP-Anfragen an AEM Author gesendet werden, um die Daten abzurufen.

>[!IMPORTANT]
>
> Stellen Sie sicher, dass die AEM-Autoreninstanz so konfiguriert ist, dass [ursprungsübergreifende Anfragen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=de) von den Ursprüngen zulässig sind, bei denen die AppBuilder-App ausgeführt wird. Zulässige Ursprünge umfassen `https://localhost:9080`, den Ursprung „AppBuilder-Staging“ und den Ursprung „AppBuilder-Produktion“.
>
> Alternativ kann die Erweiterung eine benutzerdefinierte [AppBuilder-Aktion](../../runtime-action.md) aufrufen, die die Anfrage an die AEM-Autoreninstanz im Namen der Erweiterung stellt.


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

Das Ergebnis der Render-Methode ist ein JavaScript-Objekt, dessen Schlüssel der Pfad des Inhaltsfragments (oder `fragment.id`) und der Wert der in der Spalte anzuzeigende Wert ist.

Die Ergebnisse dieser Erweiterung für die Spalte `age` sind beispielsweise:

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
