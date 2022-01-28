---
title: Erkunden Sie die AEM GraphQL-API - Erweiterte Konzepte von AEM Headless - GraphQL
description: Senden Sie GraphQL-Abfragen mit der GraphiQL-IDE. Erfahren Sie mehr über erweiterte Abfragen mit Filtern, Variablen und Anweisungen. Abfrage nach Fragmenten und Inhaltsverweisen, einschließlich Verweisen aus mehrzeiligen Textfeldern.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---

# GraphQL-API AEM

Mit der GraphQL-API in AEM können Sie Inhaltsfragmentdaten für nachgelagerte Anwendungen verfügbar machen. Im vorherigen [mehrstufiges GraphQL-Tutorial](../multi-step/explore-graphql-api.md)haben Sie die integrierte Entwicklungsumgebung für GraphiQL (IDE) untersucht, in der Sie einige gängige GraphQL-Abfragen getestet und optimiert haben. In diesem Kapitel verwenden Sie die GraphiQL-IDE, um erweiterte Abfragen zu untersuchen und so Daten aus den Inhaltsfragmenten zu erfassen, die Sie im vorherigen Kapitel erstellt haben.

## Voraussetzungen {#prerequisites}

Dieses Dokument ist Teil eines mehrteiligen Tutorials. Bevor Sie mit diesem Kapitel fortfahren, vergewissern Sie sich bitte, dass die vorherigen Kapitel abgeschlossen sind.

Die GraphiQL IDE muss installiert sein, bevor dieses Kapitel abgeschlossen werden kann. Befolgen Sie die Installationsanweisungen im vorherigen Abschnitt [mehrstufiges GraphQL-Tutorial](../multi-step/explore-graphql-api.md) für weitere Informationen.

## Ziele {#objectives}

In diesem Kapitel lernen Sie Folgendes:

* Filtern einer Liste von Inhaltsfragmenten mit Verweisen mithilfe von Abfragevariablen
* Filtern von Inhalten in einem Fragmentverweis
* Abfragen von Inline-Inhalten und Fragmentverweisen aus einem mehrzeiligen Textfeld
* Abfrage mithilfe von Anweisungen
* Abfrage nach dem Content-Typ JSON-Objekt

## Filtern einer Liste von Inhaltsfragmenten mithilfe von Abfragevariablen

Im vorherigen [mehrstufiges GraphQL-Tutorial](../multi-step/explore-graphql-api.md), haben Sie gelernt, wie Sie eine Liste von Inhaltsfragmenten filtern. Hier erweitern Sie dieses Wissen und filtern mithilfe von Variablen.

Bei der Entwicklung von Clientanwendungen müssen Sie in den meisten Fällen Inhaltsfragmente anhand dynamischer Argumente filtern. Mit der AEM GraphQL-API können Sie diese Argumente als Variablen in einer Abfrage übergeben, um Zeichenfolgenkonstruktionen auf Client-Seite zur Laufzeit zu vermeiden. Weitere Informationen zu GraphQL-Variablen finden Sie unter [GraphQL-Dokumentation](https://graphql.org/learn/queries/#variables).

In diesem Beispiel werden alle Instruktoren abgefragt, die eine bestimmte Fähigkeit besitzen.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in den linken Bereich ein:

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
         fullName
         contactInfo {
           phone
           email
         }
         profilePicture {
           ... on ImageRef {
             _path
           }
         }
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   Die `listPersonBySkill` -Abfrage akzeptiert eine Variable (`skillFilter`), die erforderlich ist `String`. Diese Abfrage führt eine Suche für alle Personen-Inhaltsfragmente durch und filtert sie basierend auf der `skills` und die übergebene Zeichenfolge `skillFilter`.

   Beachten Sie Folgendes: `listPersonBySkill` enthält `contactInfo` -Eigenschaft, die einen Fragmentverweis auf das in den vorherigen Kapiteln definierte Modell &quot;Kontaktinfo&quot;darstellt. Das Modell Kontaktinformationen enthält `phone` und `email` -Felder. Sie müssen mindestens eines dieser Felder in die Abfrage einschließen, damit sie ordnungsgemäß ausgeführt werden kann.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. Als Nächstes definieren wir `skillFilter` und alle Instruktoren, die im Skifahren befähigt sind. Fügen Sie die folgende JSON-Zeichenfolge in den Bereich &quot;Query Variables&quot;in die GraphiQL-IDE ein:

   ```json
   {
   	    "skillFilter": "Skiing"
   }
   ```

1. Führen Sie die Abfrage aus. Das Ergebnis sollte in etwa wie folgt aussehen:

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer.\nBorn in Baltimore, Maryland, Stacey is the youngest of six children. Her father was a lieutenant colonel in the US Navy and her mother was a modern dance instructor. Her family moved frequently with her father’s duty assignments, and she took her first pictures when he was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

## Filtern von Inhalten in einem Fragmentverweis

Mit der AEM GraphQL-API können Sie verschachtelte Inhaltsfragmente abfragen. Im vorherigen Kapitel haben Sie drei neue Fragmentverweise zu einem Abenteuer-Inhaltsfragment hinzugefügt: `location`, `instructorTeam`und `administrator`. Filtern wir nun alle Adventures nach jedem Administrator, der einen bestimmten Namen hat.

>[!CAUTION]
>
>Nur ein Modell darf als Referenz verwendet werden, damit diese Abfrage ordnungsgemäß ausgeführt werden kann.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in den linken Bereich ein:

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         adventureTitle
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. Fügen Sie anschließend die folgende JSON-Zeichenfolge in das Bedienfeld &quot;Abfragevariablen&quot;ein:

   ```json
   {
   	    "name": "Jacob Wester"
   }
   ```

   Die `getAdventureAdministratorDetailsByAdministratorName` Abfrage filtert alle Abenteuer für beliebige `administrator` von `fullName` &quot;Jacob Wester&quot;zurück, der Informationen aus zwei verschachtelten Inhaltsfragmenten zurückgibt: Adventure und Instructor.

1. Führen Sie die Abfrage aus. Das Ergebnis sollte in etwa wie folgt aussehen:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "adventureTitle": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for 3 years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## Abfragen von Inline-Verweisen aus einem mehrzeiligen Textfeld {#query-rte-reference}

Mit der AEM GraphQL-API können Sie in mehrzeiligen Textfeldern nach Inhalten und Fragmentverweisen abfragen. Im vorherigen Kapitel haben Sie beide Referenztypen zum **Beschreibung** -Feld des Yosemite-Team-Inhaltsfragments. Nun rufen wir diese Verweise ab.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in den linken Bereich ein:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   Die `getTeamByAdventurePath` Abfrage filtert alle Abenteuer nach Pfad und gibt Daten für die `instructorTeam` Fragmentverweis eines bestimmten Abenteuers.

   `_references` ist ein systemgeneriertes Feld, das verwendet wird, um Verweise anzuzeigen, einschließlich derjenigen, die in mehrzeilige Textfelder eingefügt werden.

   Die `getTeamByAdventurePath` Abfrage ruft mehrere Verweise ab. Zunächst wird die integrierte `ImageRef` -Objekt zum Abrufen der `_path` und `__typename` von Bildern, die als Inhaltsreferenzen in das mehrzeilige Textfeld eingefügt wurden. Als Nächstes verwendet er `LocationModel` , um die Daten des im selben Feld eingefügten Location Content Fragment abzurufen.

   Beachten Sie, dass die Abfrage auch die `_metadata` -Feld. Auf diese Weise können Sie den Namen des Team Content Fragments abrufen und später in der WKND-App anzeigen.

1. Fügen Sie als Nächstes die folgende JSON-Zeichenfolge in das Bedienfeld &quot;Abfragevariablen&quot;ein, um das Yosemite-Backpackerabenteuer zu erhalten:

   ```json
   {
   	    "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Führen Sie die Abfrage aus. Das Ergebnis sollte in etwa wie folgt aussehen:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   Beachten Sie Folgendes: `_references` zeigt sowohl das Logo-Bild als auch das Yosemite Valley Lodge Content Fragment an, das in das **Beschreibung** -Feld.


## Abfrage mithilfe von Anweisungen

Manchmal müssen Sie bei der Entwicklung von Clientanwendungen die Struktur Ihrer Abfragen bedingt ändern. In diesem Fall können Sie mit der AEM GraphQL-API GraphQL-Anweisungen verwenden, um das Verhalten Ihrer Abfragen basierend auf den bereitgestellten Kriterien zu ändern. Weitere Informationen zu GraphQL-Direktiven finden Sie unter [GraphQL-Dokumentation](https://graphql.org/learn/queries/#directives).

Im [vorheriger Abschnitt](#query-rte-reference), haben Sie gelernt, wie Sie in mehrzeiligen Textfeldern nach Inline-Verweisen abfragen können. Beachten Sie, dass der Inhalt aus dem `description` im `plaintext` Format. Als Nächstes erweitern wir diese Abfrage und verwenden eine Anweisung, um bedingt abzurufen. `description` im `json` -Format.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in den linken Bereich ein:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   Die oben stehende Abfrage akzeptiert eine weitere Variable (`includeJson`), die erforderlich ist `Boolean`, auch als Abfragerichtlinie bezeichnet. Eine Direktive kann verwendet werden, um Daten aus dem `description` im Feld `json` Format basierend auf dem booleschen Wert, der übergeben wird `includeJson`.

1. Fügen Sie anschließend die folgende JSON-Zeichenfolge in das Bedienfeld &quot;Abfragevariablen&quot;ein:

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Führen Sie die Abfrage aus. Sie sollten dasselbe Ergebnis erhalten wie im vorherigen Abschnitt auf [Anleitung zum Abfragen von Inline-Verweisen in mehrzeiligen Textfeldern](#query-rte-reference).

1. Aktualisieren Sie die `includeJson` Richtlinie `true` und führen Sie die Abfrage erneut aus. Das Ergebnis sollte in etwa wie folgt aussehen:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Abfrage nach dem Content-Typ JSON-Objekt

Denken Sie daran, dass Sie im vorherigen Kapitel über die Bearbeitung von Inhaltsfragmenten ein JSON-Objekt zum **Wetter nach Saison** -Feld. Rufen wir diese Daten jetzt im Standortinhaltsfragment ab.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in den linken Bereich ein:

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
           json
         }
         contactInfo {
           phone
           email
         }
         locationImage {
           ... on ImageRef {
             _path
           }
         }
         weatherBySeason
         address {
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   ```

1. Fügen Sie anschließend die folgende JSON-Zeichenfolge in das Bedienfeld &quot;Abfragevariablen&quot;ein:

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Führen Sie die Abfrage aus. Das Ergebnis sollte in etwa wie folgt aussehen:

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California’s Sierra Nevada mountains. It’s famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   Beachten Sie Folgendes: `weatherBySeason` enthält das JSON-Objekt, das im vorherigen Kapitel hinzugefügt wurde.

## Alle Inhalte gleichzeitig abfragen

Bisher wurden mehrere Abfragen ausgeführt, um die Funktionen der AEM GraphQL-API zu veranschaulichen. Dieselben Daten können nur mit einer einzigen Abfrage abgerufen werden:

```graphql
query getAllAdventureDetails($fragmentPath: String!) {
  adventureByPath(_path: $fragmentPath){
    item {
      _path
      adventureTitle
      adventureActivity
      adventureType
      adventurePrice
      adventureTripLength
      adventureGroupSize
      adventureDifficulty
      adventurePrice
      adventurePrimaryImage{
        ...on ImageRef{
          _path
          mimeType
          width
          height
        }
      }
      adventureDescription {
        html
        json
      }
      adventureItinerary {
        html
        json
      }
      location {
        _path
        name
        description {
          html
          json
        }
        contactInfo{
          phone
          email
        }
        locationImage{
          ...on ImageRef{
            _path
          }
        }
        weatherBySeason
        address{
            streetAddress
            city
            state
            zipCode
            country
        }
      }
      instructorTeam {
        _metadata{
            stringMetadata{
                name
                value
            }
        }        
        teamFoundingDate
        description {
            json
        }
        teamMembers {
            fullName
            contactInfo {
                phone
                email
            }
            profilePicture{
                ...on ImageRef {
                    _path
                }
            }
            instructorExperienceLevel
            skills
            biography {
                html
            }
        }       
     }
      administrator {
            fullName
            contactInfo {
                phone
                email
            }
            biography {
                html
            }
        }
    }
    _references {
        ...on ImageRef {
            _path
        mimeType
        }
        ...on LocationModel {
            _path
                __typename
        }
    }
  }
}

# in Query Variables
{
  "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
}
```

## Zusätzliche Abfragen für die WKND-App

Die folgenden Abfragen sind aufgeführt, um alle in der WKND-App benötigten Daten abzurufen. Diese Abfragen zeigen keine neuen Konzepte und werden nur als Referenz zum Erstellen Ihrer Implementierung bereitgestellt.

1. **Team-Mitglieder für ein bestimmtes Abenteuer abrufen**:

   ```graphql
   query getTeamMembersByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath ) {
       item {
         instructorTeam {
           teamMembers{
             fullName
             contactInfo{
               phone
               email
             }
           profilePicture {
               ... on ImageRef {
                 _path
               }
           }
             instructorExperienceLevel
             skills
             biography{
               plaintext
             }
           }
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **Standort-Pfad für ein bestimmtes Abenteuer abrufen**

   ```graphql
   query getLocationPathByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath){
       item {
         location{
           _path  
         } 
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **Team-Standort nach Pfad abrufen**

   ```graphql
   query getTeamLocationByLocationPath ($fragmentPath: String!){
     locationByPath (_path: $fragmentPath) {
       item {
         name
         description{
           json
         }
         contactInfo{
           phone
           email
         }
           address{
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge"
   }
   ```

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben jetzt erweiterte Abfragen getestet, um Daten zu den Inhaltsfragmenten zu erfassen, die Sie im vorherigen Kapitel erstellt haben.

## Nächste Schritte

Im [Nächstes Kapitel](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), erfahren Sie, wie Sie GraphQL-Abfragen beibehalten und warum es Best Practice ist, persistente Abfragen in Ihren Anwendungen zu verwenden.