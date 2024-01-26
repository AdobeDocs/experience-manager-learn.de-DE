---
title: Verwenden lokalisierter Inhalte mit AEM Headless
description: Erfahren Sie, wie Sie lokalisierte Inhalte in AEM mit GraphQL abfragen können.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 149
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 100%

---

# Lokalisierte Inhalte mit AEM Headless

AEM bietet ein [Framework für die Übersetzungsintegration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html?lang=de) für Headless-Inhalte. Damit können Inhaltsfragmente und unterstützende Assets einfach übersetzt werden, damit sie in verschiedenen Gebietsschemata verwendet werden können. Hierbei handelt es sich um dasselbe Framework, mit dem andere AEM-Inhalte wie Seiten, Experience Fragments, Assets und Formulare übersetzt werden. Sobald [Headless-Inhalte übersetzt](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=de) und veröffentlicht wurden, sind sie für die Verwendung durch Headless-Anwendungen bereit.

## Asset-Ordnerstruktur{#assets-folder-structure}

Stellen Sie sicher, dass die lokalisierten Inhaltsfragmente in AEM der [empfohlenen Lokalisierungsstruktur](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html?lang=de#recommended-structure) entsprechen.

![Ordner für lokalisierte AEM-Assets](./assets/localized-content/asset-folders.jpg)

Die Ordner für die Gebietsschemata müssen gleichrangige Ordner sein, und der Ordnername muss anstelle des Titels ein gültiger [ISO-639-1-Code](https://de.wikipedia.org/wiki/Liste_der_ISO-639-1-Codes) sein, der das Gebietsschema des im Ordner enthaltenen Inhalts angibt.

Der Gebietsschema-Code ist auch der Wert, der zum Filtern der von der GraphQL-Abfrage zurückgegebenen Inhaltsfragmente verwendet wird.

| Gebietsschema-Code | AEM-Pfad | Gebietsschema für Inhalte |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Deutscher Inhalt |
| en | /content/dam/.../**en**/... | Englischer Inhalt |
| es | /content/dam/.../**es**/... | Spanischer Inhalt |

## GraphQL-persistierte Abfrage

AEM bietet einen `_locale`-GraphQL-Filter, der Inhalte automatisch nach Gebietsschema-Code filtert. Zum Beispiel kann das Abrufen aller englischen Abenteuer im [WKND-Site-Projekt](https://github.com/adobe/aem-guides-wknd) mit der neuen persistierten Abfrage `wknd-shared/adventures-by-locale` durchgeführt werden, die wie folgt definiert ist:

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

Die Variable `$locale`, die im `_locale`-Filter verwendet wird, erfordert den Gebietsschema-Code (z. B. `en`, `en_us` oder `de`), wie in der [Basis-Lokalisierungskonvention für AEMs Asset-Ordner](#assets-folder-structure) angegeben.

## React-Beispiel

Erstellen wir eine einfache React-App, die steuert, welche Adventure-Inhalte von AEM basierend auf einem Gebietsschema-Selektor mithilfe des `_locale`-Filters abgefragt werden.

Wenn __Englisch__ im Gebietsschema-Selektor ausgewählt wird, werden englische Adventure-Inhaltsfragmente unter `/content/dam/wknd/en` zurückgegeben. Wenn __Spanisch__ ausgewählt wird, werden spanische Inhaltsfragmente unter `/content/dam/wknd/es` zurückgegeben, und so weiter.

![Lokalisierte React-Beispiel-App](./assets/localized-content/react-example.png)

### Erstellen eines `LocaleContext`{#locale-context}

Erstellen Sie zunächst einen [React-Kontext](https://reactjs.org/docs/context.html), damit das Gebietsschema über die Komponenten der React-App hinweg verwendet werden kann.

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### Erstellen der React-Komponente `LocaleSwitcher`{#locale-switcher}

Als Nächstes erstellen Sie eine React-Komponente mit einem Gebietsschema-Umschalter, der den Wert von [LocaleContext](#locale-context) auf die Auswahl der Person festlegt.

Dieser Gebietsschemawert wird verwendet, um die GraphQL-Abfragen auszuführen und um sicherzustellen, dass nur Inhalte zurückgegeben werden, die dem ausgewählten Gebietsschema entsprechen.

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### Abfragen von Inhalt mit dem `_locale`-Filter{#adventures}

Die Adventure-Komponente führt in AEM eine Abfrage für alle Adventures nach Gebietsschema aus und listet deren Titel auf. Dies wird erreicht, indem der im React-Kontext gespeicherte Gebietsschemawert mithilfe des `_locale`-Filters an die Abfrage übergeben wird.

Dieser Ansatz kann auf andere Abfragen in Ihrer Anwendung erweitert werden, um sicherzustellen, dass alle Abfragen nur Inhalte enthalten, die durch die Gebietsschema-Auswahl einer Person vorgegeben werden.

Die Abfrage in AEM wird im benutzerdefinierten React-Hook [getAdventuresByLocale ausgeführt, der in der Dokumentation zu Abfragen von AEM GraphQL](./aem-headless-sdk.md) näher beschrieben wird.

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### Definieren der `App.js`{#app-js}

Zum Schluss fügen Sie alles zusammen, indem Sie die React-App in `LanguageContext.Provider` einschließen und den Wert für das Gebietsschema festlegen. Dies ermöglicht den anderen React-Komponenten, [LocaleSwitcher](#locale-switcher) und [Adventures](#adventures), den Status der Gebietsschema-Auswahl zu teilen.

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
