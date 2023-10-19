---
title: Integrieren von AEM Headless und Target
description: Erfahren Sie, wie Sie AEM Headless und Adobe Target integrieren, um Headless-Erlebnisse mithilfe des Experience Platform Web SDK zu personalisieren.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '1679'
ht-degree: 100%

---

# Integrieren von AEM Headless und Target

Erfahren Sie, wie Sie AEM Headless mit Adobe Target integrieren können, indem Sie AEM-Inhaltsfragmente in Adobe Target exportieren und sie verwenden, um Headless-Erlebnisse mithilfe der Datei „legate.js“ des Adobe Experience Platform Web SDK zu personalisieren. Die [React-WKND-App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html?lang=de) wird verwendet, um zu erforschen, wie eine personalisierte Target-Aktivität unter Verwendung von Inhaltsfragment-Angeboten zum Erlebnis hinzugefügt werden kann, um ein WKND-Abenteuer zu bewerben.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

In diesem Tutorial werden die Schritte zum Einrichten von AEM und Adobe Target beschrieben:

1. [Erstellen der Adobe IMS-Konfiguration für Adobe Target](#adobe-ims-configuration) in AEM Author
2. [Erstellen eines Adobe Target Cloud Service](#adobe-target-cloud-service) in AEM Author
3. [Anwenden von Adobe Target Cloud Service auf AEM Assets-Ordner](#configure-asset-folders) in AEM Author
4. [Zulassen des Adobe Target Cloud Service](#permission) in Adobe Admin Console
5. [Exportieren von Inhaltsfragmenten](#export-content-fragments) von AEM Author zu Target
6. [Erstellen einer Aktivität mit Inhaltsfragmentangeboten](#activity) in Adobe Target
7. [Erstellen eines Experience Platform-Datenstroms](#datastream-id) in Experience Platform
8. [Integrieren der Personalisierung in eine React-basierte AEM Headless-App](#code) unter Verwendung des Adobe Web SDK

## Adobe IMS-Konfiguration{#adobe-ims-configuration}

Eine Adobe IMS-Konfiguration, die die Authentifizierung zwischen AEM und Adobe Target erleichtert.

In der [Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html?lang=de) finden Sie eine schrittweise Anleitung zum Erstellen einer Adobe IMS-Konfiguration.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe Target Cloud Service{#adobe-target-cloud-service}

In AEM wird ein Adobe Target Cloud Service erstellt, um den Export von Inhaltsfragmenten in Adobe Target zu erleichtern.

In der [Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=de) finden Sie eine schrittweise Anleitung, wie Sie einen Adobe Target Cloud Service erstellen.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Konfigurieren von Asset-Ordnern{#configure-asset-folders}

Der in einer kontextsensitiven Konfiguration konfigurierte Adobe Target Cloud Service muss auf die AEM Assets-Ordnerhierarchie mit den Inhaltsfragmenten angewendet werden, die nach Adobe Target exportiert werden sollen.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Melden Sie sich beim __AEM-Author-Service__ als DAM-Admin an.
1. Navigieren Sie zu __Assets > Dateien__ und suchen Sie den Asset-Ordner, auf den `/conf` angewendet wurde.
1. Wählen Sie den Asset-Ordner aus und wählen Sie __Eigenschaften__ in der oberen Aktionsleiste.
1. Wählen Sie die Registerkarte __Cloud-Services__ aus
1. Stellen Sie sicher, dass die Cloud-Konfiguration auf die kontextabhängige Konfiguration (`/conf`) eingestellt ist, die die Konfiguration der Adobe Target Cloud Services enthält
1. Wählen Sie __Adobe Target__ aus dem Dropdown-Menü __Cloud Service-Konfigurationen__.
1. Wählen Sie __Speichern und schließen__ oben rechts.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Zugriffsberechtigung für die AEM Target-Integration{#permission}

Die Adobe Target-Integration, die sich als developer.adobe.com-Projekt manifestiert, muss in der Adobe Admin Console die Produktrolle __Editor__ erhalten, um Inhaltsfragmente in Adobe Target exportieren zu können.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Melden Sie sich bei Experience Cloud als eine Person an, die das Adobe Target-Produkt in Adobe Admin Console verwalten kann
1. Öffnen Sie die [Adobe Admin Console](https://adminconsole.adobe.com)
1. Wählen Sie __Produkte__ und öffnen Sie dann __Adobe Target__
1. Wählen Sie auf der Registerkarte __Produktprofile__ die Option __*DefaultWorkspace*__.
1. Wählen Sie die Registerkarte __API-Anmeldeinformationen__
1. Suchen Sie Ihre developer.adobe.com-Anwendung in dieser Liste und setzen Sie ihre __Produktrolle__ auf __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Exportieren von Inhaltsfragmenten in Target{#export-content-fragments}

Inhaltsfragmente, die in der [konfigurierten AEM-Assets-Ordnerhierarchie](#apply-adobe-target-cloud-service-to-aem-assets-folders) vorhanden sind, können als Inhaltsfragmentangebote in Adobe Target exportiert werden. Diese Inhaltsfragmentangebote, eine spezielle Form von JSON-Angeboten in Target, können in Target-Aktivitäten verwendet werden, um personalisierte Erlebnisse in Headless-Apps bereitzustellen.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Melden Sie sich bei __AEM Author__ als DAM-Benutzerin bzw. -Benutzer an
1. Navigieren Sie zu __Assets > Dateien__ und suchen Sie im Ordner „Adobe Target aktiviert“ die Inhaltsfragmente, die als JSON in Target exportiert werden sollen
1. Wählen Sie die Inhaltsfragmente aus, die nach Adobe Target exportiert werden sollen
1. Wählen Sie __Nach Adobe Target-Angebote exportieren__ in der oberen Aktionsleiste
   + Diese Aktion exportiert die vollständig hydrierte JSON-Darstellung des Inhaltsfragments als „Inhaltsfragmentangebot“ in Adobe Target
   + Die vollständig hydrierte JSON-Darstellung kann in AEM überprüft werden
      + Wählen Sie das Inhaltsfragment aus
      + Erweitern Sie die Seitenleiste
      + Wählen Sie das Symbol __Vorschau__ in der linken Seitenleiste
      + Die JSON-Darstellung, die nach Adobe Target exportiert wird, wird in der Hauptansicht angezeigt
1. Melden Sie sich bei [Adobe Experience Cloud](https://experience.adobe.com) als eine Person in der Rolle „Editor“ für Adobe Target an
1. Wählen Sie in [Experience Cloud](https://experience.adobe.com) oben rechts in der Produktauswahl die Option __Target__, um Adobe Target zu öffnen
1. Vergewissern Sie sich, dass der Standard-Arbeitsbereich im __Arbeitsbereich-Umschalter__ oben rechts ausgewählt ist
1. Wählen Sie die Registerkarte __Angebote__ in der oberen Navigationsleiste
1. Wählen Sie das Dropdown-Menü __Typ__ und wählen Sie __Inhaltsfragmente__
1. Überprüfen Sie, ob das aus AEM exportierte Inhaltsfragment in der Liste angezeigt wird
   + Bewegen Sie den Mauszeiger über das Angebot und wählen Sie die Schaltfläche __Ansicht__
   + Überprüfen Sie die __Angebotsinformationen__ und sehen Sie den __AEM-Deep-Link__, der das Inhaltsfragment direkt im AEM-Author-Service öffnet

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Target-Aktivität mit Inhaltsfragmentangeboten{#activity}

In Adobe Target kann eine Aktivität erstellt werden, die die JSON-Ausgabe des Inhaltsfragmentangebots als Inhalt verwendet und so personalisierte Erlebnisse in der Headless-App mit in AEM erstellten und verwalteten Inhalten ermöglicht.

In diesem Beispiel verwenden wir eine einfache A/B-Aktivität, es kann aber jede Target-Aktivität verwendet werden.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Wählen Sie die Registerkarte __Aktivitäten__ in der oberen Navigationsleiste aus
1. Wählen Sie __+ Aktivität erstellen__, und wählen Sie dann die Art der zu erstellenden Aktivität
   + In diesem Beispiel wird ein einfacher __A/B-Test__ erstellt – Inhaltsfragmentangebote unterstützen aber jeden Aktivitätstyp
1. Im Assistenten __Aktivität erstellen__:
   + Wählen Sie __Web__ aus
   + In __Experience Composer auswählen__ wählen Sie __Formular__ aus
   + Wählen Sie unter __Arbeitsbereich auswählen__ die Option __Standardarbeitsbereich__ aus
   + Wählen Sie unter __Eigenschaft wählen__ die Eigenschaft aus, in der die Aktivität verfügbar ist, oder wählen Sie __Keine Eigenschaftseinschränkungen__, damit sie in allen Eigenschaften verwendet werden kann
   + Wählen Sie __Weiter__, um die Aktivität zu erstellen
1. Benennen Sie die Aktivität um, indem Sie oben links __Umbenennen__ wählen
   + Geben Sie der Aktivität einen aussagekräftigen Namen
1. Legen Sie im ersten Erlebnis __Speicherort 1__ für die Zielaktivität fest
   + In diesem Beispiel wählen Sie einen benutzerdefinierten Speicherort namens `wknd-adventure-promo`
1. Wählen Sie unter __Inhalt__ den Standardinhalt aus, und wählen Sie __Inhaltsfragment ändern__
1. Wählen Sie das exportierte Inhaltsfragment aus, das für dieses Erlebnis bereitgestellt werden soll, und wählen Sie __Fertig__
1. Überprüfen Sie das JSON des Inhaltsfragmentangebots im Textbereich des Inhalts. Dies ist dasselbe JSON, das im AEM-Author-Service über die Aktion „Vorschau“ des Inhaltsfragments verfügbar ist.
1. Fügen Sie in der linken Leiste ein Erlebnis hinzu und wählen Sie ein anderes Inhaltsfragmentangebot aus, das bereitgestellt werden soll
1. Wählen Sie __Weiter__, und konfigurieren Sie die für die Aktivität erforderlichen Regeln für die Zielerfassung
   + In diesem Beispiel belassen Sie den A/B-Test bei einer manuellen 50/50-Aufteilung
1. Wählen Sie __Weiter__ und vervollständigen Sie die Aktivitätseinstellungen
1. Wählen Sie __Speichern und schließen__ und geben Sie der Datei einen aussagekräftigen Namen
1. Wählen Sie in der Aktivität in Adobe Target die Option __Aktivieren__ aus dem Dropdown-Menü „Inaktiv/Aktivieren/Archivieren“ oben rechts

Die Adobe Target-Aktivität, die auf den Speicherort `wknd-adventure-promo` abzielt, kann jetzt in eine AEM Headless-Anwendung integriert und angezeigt werden.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform-Datenstrom-ID{#datastream-id}

Eine [Adobe Experience Platform-Datenstrom-ID](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html?lang=de) ist für AEM Headless-Anwendungen erforderlich, um mit Adobe Target unter Verwendung des [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=de) zu interagieren.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Navigieren Sie zu [Adobe Experience Cloud](https://experience.adobe.com/)
1. Öffnen Sie __Experience Platform__
1. Wählen Sie __Datenerfassung > Datenströme__ und wählen Sie __Neuer Datenstrom__
1. Geben Sie im Assistenten für neue Datenströme Folgendes ein:
   + Name: `AEM Target integration`
   + Beschreibung: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Ereignisschema: `Leave blank`
1. Wählen Sie __Speichern__ aus
1. Wählen Sie __Service hinzufügen__ aus
1. Wählen Sie unter __Service__ die Option __Adobe Target__
   + Aktiviert: __Ja__
   + Eigenschafts-Token: __Leer lassen__
   + Zielumgebungs-ID: __Leer lassen__
      + Die Zielumgebung kann in Adobe Target unter __Administration > Hosts__ festgelegt werden
   + Target-Drittanbieter-ID-Namespace: __Leer lassen__
1. Wählen Sie __Speichern__ aus
1. Kopieren Sie auf der rechten Seite die __Datenstrom-ID__ für die Verwendung im Konfigurationsaufruf [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=de)

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Hinzufügen der Personalisierung zu einer AEM Headless-App{#code}

In diesem Tutorial geht es um die Personalisierung einer einfachen React-App mit Inhaltsfragmentangeboten in Adobe Target über das [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=de). Dieser Ansatz kann verwendet werden, um jedes JavaScript-basierte Web-Erlebnis zu personalisieren.

Android™- und iOS-Mobilgeräte können mit dem [Adobe Mobile SDK](https://developer.adobe.com/client-sdks/documentation/) nach ähnlichen Mustern personalisiert werden.

### Voraussetzungen

+ Node.js 14
+ Git
+ [WKND Shared 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) auf Author- und Publish-Service in AEM as a Cloud installiert

### Setup

1. Laden Sie den Quell-Code für die Beispiel-React-App von [Github.com](https://github.com/adobe/aem-guides-wknd-graphql) herunter

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie die Code-Basis unter `~/Code/aem-guides-wknd-graphql/personalization-tutorial` in Ihrer bevorzugten IDE
1. Aktualisieren Sie den Host des AEM-Dienstes, mit dem die App eine Verbindung herstellen soll `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Führen Sie die App aus und stellen Sie sicher, dass sie eine Verbindung zum konfigurierten AEM-Dienst herstellt. Führen Sie in der Befehlszeile Folgendes aus:

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Installieren Sie das [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=de#option-3%3A-using-the-npm-package) als NPM-Paket.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Das Web SDK kann im Code verwendet werden, um die JSON-Datei für Inhaltsfragmentangebote nach Ort der Aktivität abzurufen.

   Beim Konfigurieren des Web SDK sind zwei IDs erforderlich:

   + `edgeConfigId`, die [Datenstrom-ID](#datastream-id)
   + `orgId`, die Adobe-Organisations-ID für AEM as a Cloud Service/Target, die unter __Experience Cloud > Profil > Kontoinformationen > Aktuelle Organisations-ID__ gefunden werden kann

   Beim Aufrufen des Web SDK muss der Ort der Adobe Target-Aktivität (in unserem Beispiel `wknd-adventure-promo`) als Wert im Array `decisionScopes` festgelegt werden.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementierung

1. Erstellen Sie eine React-Komponente `AdobeTargetActivity.js`, um Adobe Target-Aktivitäten anzuzeigen:

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   Die React-Komponente „AdobeTargetActivity“ wird wie folgt aufgerufen:

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Erstellen Sie eine React-Komponente `AdventurePromo.js`, um das JSON des Abenteuers zu rendern, das Adobe Target bereitstellt.

   Diese React-Komponente nutzt die vollständig erweiterte JSON-Datei, die ein Abenteuer-Inhaltsfragment darstellt und im Werbestil angezeigt wird. Die React-Komponenten, die die von Adobe Target-Inhaltsfragment-Angeboten bereitgestellte JSON anzeigen, können je nach den in Adobe Target exportierten Inhaltsfragmenten so vielfältig und komplex sein wie erforderlich.

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   Diese React-Komponente wird wie folgt aufgerufen:

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. Fügen Sie die AdobeTargetActivity-Komponente zur `Home.js` der React-Anwendung über der Liste der Abenteuer hinzu.

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. Wenn die React-Anwendung nicht ausgeführt wird, starten Sie sie mit `npm run start` neu.

   Öffnen Sie die React-Anwendung in zwei verschiedenen Browsern, damit der A/B-Test verschiedenen Erlebnisse für jeden Browser bereitstellen kann. Wenn in beiden Browsern dasselbe Abenteuerangebot angezeigt wird, versuchen Sie, einen der Browser zu schließen und erneut zu öffnen, bis das andere Erlebnis angezeigt wird.

   Die folgende Abbildung zeigt die beiden unterschiedlichen Inhaltsfragmentangebote, die für die `wknd-adventure-promo`-Aktivität angezeigt werden, basierend auf der Logik von Adobe Target.

   ![Erlebnisangebote](./assets/target/offers-in-app.png)

## Herzlichen Glückwunsch!

Jetzt haben wir AEM as a Cloud Service für den Export von Inhaltsfragmenten in Adobe Target konfiguriert, die Inhaltsfragmentangebote in einer Adobe Target-Aktivität verwendet und diese Aktivität in einer AEM Headless-Anwendung angezeigt, um das Erlebnis zu personalisieren.
