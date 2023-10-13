---
title: Integrieren AEM Headless und Target
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
workflow-type: tm+mt
source-wordcount: '1679'
ht-degree: 2%

---

# Integrieren AEM Headless und Target

Erfahren Sie, wie Sie AEM Headless mit Adobe Target integrieren können, indem Sie AEM Inhaltsfragmente in Adobe Target exportieren und sie verwenden, um Headless-Erlebnisse mithilfe der Datei &quot;legate.js&quot;des Adobe Experience Platform Web SDK zu personalisieren. Die [React WKND App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) wird verwendet, um zu untersuchen, wie eine personalisierte Target-Aktivität mit Inhaltsfragmentangeboten zum Erlebnis hinzugefügt werden kann, um ein WKND-Abenteuer zu fördern.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

In diesem Tutorial werden die Schritte zum Einrichten von AEM und Adobe Target beschrieben:

1. [Erstellen der Adobe IMS-Konfiguration für Adobe Target](#adobe-ims-configuration) in AEM Author
2. [Adobe Target-Cloud Service erstellen](#adobe-target-cloud-service) in AEM Author
3. [Anwenden von Adobe Target Cloud Service auf AEM Assets-Ordner](#configure-asset-folders) in AEM Author
4. [Berechtigung für den Adobe Target-Cloud Service](#permission) in Adobe Admin Console
5. [Exportieren von Inhaltsfragmenten](#export-content-fragments) von AEM-Autor zu Target
6. [Erstellen einer Aktivität mit Inhaltsfragmentangeboten](#activity) in Adobe Target
7. [Erstellen eines Experience Platform-Datenspeichers](#datastream-id) in Experience Platform
8. [Integrieren der Personalisierung in eine React-basierte AEM Headless-App](#code) unter Verwendung des Adobe Web SDK.

## Adobe IMS-Konfiguration{#adobe-ims-configuration}

Eine Adobe IMS-Konfiguration, die die Authentifizierung zwischen AEM und Adobe Target erleichtert.

Überprüfen [die Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) für eine schrittweise Anleitung zum Erstellen einer Adobe IMS-Konfiguration.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe Target Cloud Service{#adobe-target-cloud-service}

In AEM wird ein Adobe Target-Cloud Service erstellt, um den Export von Inhaltsfragmenten in Adobe Target zu erleichtern.

Überprüfen [die Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) für eine schrittweise Anleitung zum Erstellen eines Adobe Target-Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Asset-Ordner konfigurieren{#configure-asset-folders}

Der in einer kontextsensitiven Konfiguration konfigurierte Adobe Target-Cloud Service muss auf die AEM Assets-Ordnerhierarchie mit den Inhaltsfragmenten angewendet werden, die nach Adobe Target exportiert werden sollen.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Anmelden bei __AEM-Autorendienst__ als DAM-Administrator
1. Navigieren Sie zu __Assets > Dateien__, suchen Sie den Asset-Ordner mit dem `/conf` angewendet auf
1. Wählen Sie den Asset-Ordner aus und wählen Sie __Eigenschaften__ in der oberen Aktionsleiste
1. Wählen Sie die Registerkarte __Cloud-Services__ aus
1. Stellen Sie sicher, dass die Cloud-Konfiguration auf die kontextabhängige Konfiguration (`/conf`), die die Adobe Target Cloud Service-Konfiguration enthält.
1. Auswählen __Adobe Target__ aus dem __Cloud Service-Konfigurationen__ Dropdown.
1. Auswählen __Speichern und schließen__ oben rechts

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Berechtigung für die AEM Target-Integration{#permission}

Die Adobe Target-Integration, die sich als developer.adobe.com -Projekt äußert, muss mit dem __Bearbeiter__ Produktrolle in Adobe Admin Console verwenden, um Inhaltsfragmente in Adobe Target zu exportieren.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Melden Sie sich bei Experience Cloud als Benutzer an, der das Adobe Target-Produkt in Adobe Admin Console verwalten kann
1. Öffnen Sie die [Adobe Admin Console](https://adminconsole.adobe.com)
1. Auswählen __Produkte__ und dann öffnen __Adobe Target__
1. Im __Produktprofile__ Registerkarte auswählen __*DefaultWorkspace*__
1. Wählen Sie die __API-Anmeldeinformationen__ tab
1. Suchen Sie Ihre developer.adobe.com -App in dieser Liste und legen Sie ihre __Produktrolle__ nach __Bearbeiter__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Exportieren von Inhaltsfragmenten in Target{#export-content-fragments}

Inhaltsfragmente, die unter dem [konfigurierte AEM Assets-Ordnerhierarchie](#apply-adobe-target-cloud-service-to-aem-assets-folders) kann als Inhaltsfragmentangebote in Adobe Target exportiert werden. Diese Inhaltsfragmentangebote, eine spezielle Form von JSON-Angeboten in Target, können in Target-Aktivitäten verwendet werden, um personalisierte Erlebnisse in Headless-Apps bereitzustellen.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Anmelden bei __AEM__ als DAM-Benutzer
1. Navigieren Sie zu __Assets > Dateien__ und suchen Sie nach Inhaltsfragmenten, die als JSON in Target exportiert werden sollen, im Ordner &quot;Adobe Target aktiviert&quot;.
1. Wählen Sie die Inhaltsfragmente aus, die nach Adobe Target exportiert werden sollen
1. Auswählen __Exportieren in Adobe Target-Angebote__ in der oberen Aktionsleiste
   + Diese Aktion exportiert die vollständig hydrierte JSON-Darstellung des Inhaltsfragments als &quot;Inhaltsfragmentangebot&quot;in Adobe Target
   + Die vollständig hydrierte JSON-Darstellung kann in AEM
      + Inhaltsfragment auswählen
      + Seitenbereich erweitern
      + Auswählen __Vorschau__ Symbol im linken Seitenbereich
      + Die JSON-Darstellung, die nach Adobe Target exportiert wird, wird in der Hauptansicht angezeigt
1. Anmelden bei [Adobe Experience Cloud](https://experience.adobe.com) mit einem Benutzer in der Editor-Rolle für Adobe Target
1. Aus dem [Experience Cloud](https://experience.adobe.com)auswählen __Target__ über den Produktschalter oben rechts, um Adobe Target zu öffnen.
1. Stellen Sie sicher, dass der Standardarbeitsbereich im __Workspace-Umschalter__ oben rechts.
1. Wählen Sie die __Angebote__ Registerkarte in der oberen Navigation
1. Wählen Sie die __Typ__ Dropdown und __Inhaltsfragmente__
1. Überprüfen Sie, ob das aus AEM exportierte Inhaltsfragment in der Liste angezeigt wird.
   + Bewegen Sie den Mauszeiger über das Angebot und wählen Sie die __Ansicht__ button
   + Überprüfen Sie die __Angebotsinformationen__ und sehen Sie die __AEM Deep-Link__ , das das Inhaltsfragment direkt im AEM Authoring-Dienst öffnet

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Target-Aktivität mit Inhaltsfragmentangeboten{#activity}

In Adobe Target kann eine Aktivität erstellt werden, die die JSON-Ausgabe des Inhaltsfragmentangebots als Inhalt verwendet und so personalisierte Erlebnisse in der Headless-App mit in AEM erstellten und verwalteten Inhalten ermöglicht.

In diesem Beispiel verwenden wir eine einfache A/B-Aktivität, jedoch kann jede Target-Aktivität verwendet werden.

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Wählen Sie die __Tätigkeiten__ Registerkarte in der oberen Navigation
1. Auswählen __+ Aktivität erstellen__ und wählen Sie dann den zu erstellenden Aktivitätstyp aus.
   + In diesem Beispiel wird eine einfache __A/B-Test__ Inhaltsfragmentangebote können jedoch jeden Aktivitätstyp unterstützen
1. Im __Aktivität erstellen__ Assistent
   + Auswählen __Web__
   + In __Experience Composer auswählen__ auswählen __Formular__
   + In __Arbeitsbereich auswählen__ auswählen __Standardarbeitsbereich__
   + In __Eigenschaft auswählen__, wählen Sie die Eigenschaft aus, in der die Aktivität verfügbar ist, oder wählen Sie __Keine Eigenschaftenbeschränkungen__ , damit sie in allen Eigenschaften verwendet werden kann.
   + Auswählen __Nächste__ zur Erstellung der Aktivität
1. Benennen Sie die Aktivität um, indem Sie __umbenennen__ oben links
   + Geben Sie der Aktivität einen aussagekräftigen Namen
1. Legen Sie im ersten Erlebnis __Position 1__ Zielgruppe der Aktivität
   + In diesem Beispiel wird ein benutzerdefinierter Ort mit dem Namen `wknd-adventure-promo`
1. under __Inhalt__ Wählen Sie den Standardinhalt aus und wählen Sie __Inhaltsfragment ändern__
1. Wählen Sie das exportierte Inhaltsfragment aus, das für dieses Erlebnis bereitgestellt werden soll, und wählen Sie __Fertig__
1. Überprüfen Sie die JSON-Datei für Inhaltsfragmentangebote im Textbereich &quot;Inhalt&quot;. Dies ist die gleiche JSON, die im Autorendienst über die Vorschauaktion des Inhaltsfragments verfügbar ist.
1. Fügen Sie in der linken Leiste ein Erlebnis hinzu und wählen Sie ein anderes Inhaltsfragmentangebot aus, das bereitgestellt werden soll
1. Auswählen __Nächste__ und konfigurieren Sie die Targeting-Regeln nach Bedarf für die Aktivität.
   + Lassen Sie in diesem Beispiel die A/B-Tests als manuelle 50/50-Aufteilung.
1. Auswählen __Nächste__ und die Aktivitätseinstellungen abschließen
1. Auswählen __Speichern und schließen__ und einen aussagekräftigen Namen geben
1. Wählen Sie aus der Aktivität in Adobe Target die Option __Aktivieren__ oben rechts aus dem Dropdown-Menü Inaktiv/Aktivieren/Archivieren .

Die Adobe Target-Aktivität, die auf die `wknd-adventure-promo` Der Standort kann jetzt in eine AEM Headless-App integriert und verfügbar gemacht werden.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform Datastream-ID{#datastream-id}

Ein [Adobe Experience Platform-Datenspeicher](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) Die ID ist erforderlich, damit AEM Headless-Apps mit Adobe Target mithilfe der [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Erweitern für Schritt-für-Schritt-Anweisungen

1. Navigieren Sie zu [Adobe Experience Cloud](https://experience.adobe.com/)
1. Öffnen __Experience Platform__
1. Auswählen __Datenerfassung > Datenspeicher__ und wählen __Neuer Datenspeicher__
1. Geben Sie im Assistenten für neue Datensätze Folgendes ein:
   + Name: `AEM Target integration`
   + Beschreibung: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Ereignisschema: `Leave blank`
1. Wählen Sie __Speichern__ aus
1. Wählen Sie __Service hinzufügen__ aus
1. In __Dienst__ select __Adobe Target__
   + Aktiviert: __Ja__
   + Eigenschafts-Token: __Leer lassen__
   + Ziel-Umgebungs-ID: __Leer lassen__
      + Die Zielumgebung kann in Adobe Target unter __Administration > Hosts__.
   + Target-Drittanbieter-ID-Namespace: __Leer lassen__
1. Wählen Sie __Speichern__ aus
1. Kopieren Sie rechts die __Datenspeicher-ID__ zur Verwendung in [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) Konfigurationsaufruf.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Hinzufügen der Personalisierung zu einer AEM Headless-App{#code}

In diesem Tutorial wird die Personalisierung einer einfachen React-App mithilfe von Inhaltsfragmentangeboten in Adobe Target über [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Dieser Ansatz kann verwendet werden, um jedes JavaScript-basierte Web-Erlebnis zu personalisieren.

Die mobilen Erlebnisse von Android™ und iOS können mithilfe des [Adobe Mobile SDK](https://developer.adobe.com/client-sdks/documentation/).

### Voraussetzungen

+ Node.js 14
+ Git
+ [WKND Shared 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) auf AEM as a Cloud Author and Publish services installiert

### Setup

1. Quellcode für die React-Beispielanwendung herunterladen von [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie die Codebasis unter `~/Code/aem-guides-wknd-graphql/personalization-tutorial` in der bevorzugten IDE
1. Aktualisieren des Hosts des AEM-Dienstes, mit dem die App eine Verbindung herstellen soll `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

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

1. Installieren Sie die [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) als NPM-Paket.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Das Web SDK kann in Code verwendet werden, um die JSON-Datei für Inhaltsfragmentangebote nach Aktivitätsspeicherort abzurufen.

   Beim Konfigurieren des Web SDK sind zwei IDs erforderlich:

   + `edgeConfigId` , die [Datenspeicher-ID](#datastream-id)
   + `orgId` die AEM as a Cloud Service/Target-Adobe-Organisations-ID, die unter __Experience Cloud > Profil > Kontoinformationen > Aktuelle Organisations-ID__

   Beim Aufrufen des Web SDK der Adobe Target-Aktivitätsspeicherort (in unserem Beispiel: `wknd-adventure-promo`) muss als Wert im `decisionScopes` Array.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementierung

1. Erstellen einer React-Komponente `AdobeTargetActivity.js` , um Adobe Target-Aktivitäten aufzudecken.

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

   Die AdobeTargetActivity React-Komponente wird wie folgt aufgerufen:

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Erstellen einer React-Komponente `AdventurePromo.js` um die Abenteuer JSON Adobe Target-Dienste zu rendern.

   Diese React-Komponente nimmt die vollständig hydrierte JSON-Datei, die ein Inhaltsfragment für Abenteuer darstellt und in einer Werbeanzeige angezeigt wird. Die React-Komponenten, die die von Adobe Target Content Fragment-Angeboten bereitgestellten JSON anzeigen, können je nach den nach Adobe Target exportierten Inhaltsfragmenten so vielfältig und komplex sein wie erforderlich.

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

1. Fügen Sie die AdobeTargetActivity-Komponente der React-App hinzu. `Home.js` über der Liste der Abenteuer.

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

1. Wenn die React-App nicht ausgeführt wird, verwenden Sie erneut `npm run start`.

   Öffnen Sie die React-App in zwei verschiedenen Browsern, damit der A/B-Test die verschiedenen Erlebnisse für jeden Browser bereitstellen kann. Wenn in beiden Browsern dasselbe Angebot angezeigt wird, versuchen Sie, einen der Browser zu schließen/erneut zu öffnen, bis das andere Erlebnis angezeigt wird.

   Die folgende Abbildung zeigt die beiden unterschiedlichen Inhaltsfragmentangebote, die für die `wknd-adventure-promo` Aktivität, basierend auf der Logik von Adobe Target.

   ![Erlebnisangebote](./assets/target/offers-in-app.png)

## Herzlichen Glückwunsch!

Nachdem wir AEM as a Cloud Service für den Export von Inhaltsfragmenten in Adobe Target konfiguriert haben, haben wir die Inhaltsfragmentangebote in einer Adobe Target-Aktivität verwendet und diese Aktivität in einer AEM Headless-App angezeigt, um das Erlebnis zu personalisieren.
