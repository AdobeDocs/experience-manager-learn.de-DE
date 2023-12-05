---
title: Erkunden der AEM-GraphQL-API – Erweiterte Konzepte von AEM Headless – GraphQL
description: Senden Sie GraphQL-Abfragen mit der GraphiQL-IDE. Erfahren Sie mehr über erweiterte Abfragen mithilfe von Filtern, Variablen und Anweisungen. Erstellen Sie Abfragen von Fragmenten und Inhaltsverweisen, einschließlich der Verweise aus mehrzeiligen Textfeldern.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
duration: 628
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 100%

---

# Erkunden der AEM-GraphQL-API

Mit der GraphQL-API in AEM können Sie Inhaltsfragmentdaten für nachgelagerte Anwendungen bereitstellen. Im [mehrstufigen GraphQL-Einführungs-Tutorial](../multi-step/explore-graphql-api.md) haben Sie den GraphiQL-Explorer zum Testen und Verfeinern von GraphQL-Abfragen verwendet.

In diesem Kapitel verwenden Sie den GraphiQL-Explorer, um erweiterte Abfragen zum Erfassen von Daten der im [vorherigen Kapitel](../advanced-graphql/author-content-fragments.md) erstellten Inhaltsfragmente zu definieren.

## Voraussetzungen {#prerequisites}

Dieses Dokument ist Teil eines mehrteiligen Tutorials. Bevor Sie mit diesem Kapitel fortfahren, sollten Sie die vorherigen Kapitel abgeschlossen haben.

## Ziele {#objectives}

In diesem Kapitel lernen Sie Folgendes:

* Filtern einer Liste von Inhaltsfragmenten mit Verweisen mithilfe von Abfragevariablen
* Filtern von Inhalten in einem Fragmentverweis
* Abfragen von Inline-Inhalten und Fragmentverweisen über ein mehrzeiliges Textfeld
* Abfragen mithilfe von Anweisungen
* Abfragen nach dem Content-Typ des JSON-Objekts

## Verwenden des GraphiQL-Explorers


Der [GraphiQL-Explorer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=de) ermöglicht es Entwickelnden, Abfragen für Inhalte in der aktuellen AEM-Umgebung zu erstellen und zu testen. Mit dem GraphiQL-Tool können Benutzende auch Abfragen **beibehalten oder speichern**, die von Client-Anwendungen in einem Produktionsszenario verwendet werden sollen.

Erkunden Sie als Nächstes die Leistungsfähigkeit der AEM-GraphQL-API mit dem integrierten GraphiQL-Explorer.

1. Navigieren Sie im AEM-Startbildschirm zu **Tools** > **Allgemein** > **GraphQL-Abfrage-Editor**.

   ![Navigieren zur GraphiQL-IDE](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>In einigen Versionen von AEM (6.X.X) muss das Tool GraphiQL-Explorer (auch GraphiQL-IDE genannt) manuell installiert werden. Befolgen Sie hierzu [diese Anweisung](../how-to/install-graphiql-aem-6-5.md).

1. Stellen Sie sicher, dass oben rechts der Endpunkt auf **Freigegebener WKND-Endpunkt** eingestellt ist. Wenn Sie hier den Dropdown-Wert für den _Endpunkt_ ändern, werden oben links die vorhandenen _persistierten Abfragen_ angezeigt.

   ![Festlegen des GraphQL-Endpunkts](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Dadurch werden alle Abfragen auf Modelle angewendet, die im **freigegebenen WKND-Projekt** erstellt wurden.


## Filtern einer Liste von Inhaltsfragmenten mithilfe von Abfragevariablen

Im vorherigen [mehrstufigen GraphQL-Tutorial](../multi-step/explore-graphql-api.md) haben Sie grundlegende persistierte Abfragen definiert und verwendet, um Inhaltsfragmentdaten abzurufen. Hier erweitern Sie dieses Wissen und filtern Inhaltsfragmentdaten, indem Sie Variablen an die persistierten Abfragen übergeben.

Bei der Entwicklung von Client-Anwendungen müssen Sie normalerweise Inhaltsfragmente basierend auf dynamischen Argumenten filtern. Mit der AEM-GraphQL-API können Sie diese Argumente als Variablen in einer Abfrage übergeben, um die Erstellung von Zeichenfolgen zur Laufzeit auf Client-Seite zu vermeiden. Weitere Informationen zu GraphQL-Variablen finden Sie in der [GraphQL-Dokumentation](https://graphql.org/learn/queries/#variables).

In diesem Beispiel wird sämtliches Lehrpersonal abgefragt, das eine bestimmte Fähigkeit besitzt.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in das linke Panel ein:

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

   Die oben stehende Abfrage `listPersonBySkill` akzeptiert 1 Variable (`skillFilter`), bei der es sich um einen erforderlichen `String` handelt. Diese Abfrage führt eine Suche für alle personenbezogenen Inhaltsfragmente durch und filtert sie basierend auf dem Feld `skills` und der in `skillFilter` übergebenen Zeichenfolge.

   `listPersonBySkill` umfasst die Eigenschaft `contactInfo`, die einen Fragmentverweis auf das in den vorherigen Kapiteln definierte contactInfo-Modell darstellt. Das contactInfo-Modell enthält die Felder `phone` und `email`. Mindestens eines dieser Felder muss in der Abfrage vorhanden sein, damit diese ordnungsgemäß ausgeführt werden kann.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. Als Nächstes definieren wir `skillFilter` und rufen sämtliches Lehrpersonal ab, das Skifahren kann. Fügen Sie die folgende JSON-Zeichenfolge in das Panel „Abfragevariablen“ der GraphiQL-IDE ein:

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. Führen Sie die Abfrage aus. Das Ergebnis sollte etwa wie folgt aussehen:

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
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
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

Wählen Sie im oberen Menü die Schaltfläche für die **Wiedergabe** aus, um die Abfrage auszuführen. Sie sollten die Ergebnisse der Inhaltsfragmente aus dem vorherigen Kapitel sehen:

![Person nach Qualifikationsergebnissen](assets/explore-graphql-api/person-by-skill.png)

## Filtern von Inhalten in einem Fragmentverweis

Mit der AEM GraphQL-API können Sie verschachtelte Inhaltsfragmente abfragen. Im vorherigen Kapitel haben Sie drei neue Fragmentverweise zu einem Adventure-Inhaltsfragment hinzugefügt: `location`, `instructorTeam` und `administrator`. Filtern wir nun alle Adventures nach Administrierenden mit einem bestimmten Namen.

>[!CAUTION]
>
>Nur ein Modell darf als Verweis verwendet werden, damit diese Abfrage ordnungsgemäß ausgeführt werden kann.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in das linke Panel ein:

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
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

1. Fügen Sie anschließend die folgende JSON-Zeichenfolge in das Panel „Abfragevariablen“ ein:

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   Die Abfrage `getAdventureAdministratorDetailsByAdministratorName` filtert alle Adventures für beliebige `administrator` mit `fullName` „Jacob Wester“ und gibt Informationen aus zwei verschachtelten Inhaltsfragmenten zurück: Adventure und Instructor.

1. Führen Sie die Abfrage aus. Das Ergebnis sollte etwa wie folgt aussehen:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
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
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
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

## Abfrage für Inline-Referenzen aus einem mehrzeiligen Textfeld {#query-rte-reference}

Mit der AEM GraphQL-API können Sie Inhalte und Fragmentreferenzen in mehrzeiligen Textfeldern abfragen. Im vorherigen Kapitel haben Sie beide Referenztypen zum Feld **Beschreibung** des Inhaltsfrgments „Yosemite Team“ hinzugefügt. Nun rufen wir diese Referenzen ab.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in das linke Panel ein:

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

   Die Abfrage `getTeamByAdventurePath` filtert alle Adventures nach Pfad und gibt Daten für die `instructorTeam`-Fragmentreferenz eines bestimmten Adventures zurück.

   `_references` ist ein systemgeneriertes Feld, das für die Anzeige von Referenzen verwendet wird, einschließlich derjenigen, die in mehrzeilige Textfelder eingefügt werden.

   Die Abfrage `getTeamByAdventurePath` ruft mehrere Referenzen ab. Zunächst wird das integrierte Objekt `ImageRef` zum Abrufen von `_path` und `__typename` von Bildern verwendet, die als Inhaltsreferenzen in das mehrzeilige Textfeld eingefügt wurden. Als Nächstes verwendet sie `LocationModel`, um die Daten des im selben Feld eingefügten Inhaltsfragments „Standort“ abzurufen.

   Die Abfrage enthält auch das Feld `_metadata`. Auf diese Weise können Sie den Namen des Inhaltsfragments „Team“ abrufen und später in der WKND-App anzeigen.

1. Fügen Sie als Nächstes die folgende JSON-Zeichenfolge in das Panel „Abfragevariablen“ ein, um das Yosemite-Backpacker-Adventure zu erhalten:

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Führen Sie die Abfrage aus. Das Ergebnis sollte etwa wie folgt aussehen:

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
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   Das Feld `_references` zeigt sowohl das Logo-Bild als auch das Inhaltsfragment „Yosemite Valley Lodge“ an, das in das Feld **Beschreibung** eingefügt wurde.


## Abfragen mithilfe von Anweisungen

Manchmal müssen Sie bei der Entwicklung von Client-Anwendungen die Struktur Ihrer Abfragen beim Vorliegen bestimmter Bedingungen ändern. In diesem Fall können Sie mit der AEM GraphQL-API GraphQL-Anweisungen verwenden, um das Verhalten Ihrer Abfragen anhand der vorliegenden Kriterien zu ändern. Weitere Informationen zu GraphQL-Anweisungen finden Sie in der [Dokumentation zu GraphQL](https://graphql.org/learn/queries/#directives).

Im [vorherigen Abschnitt](#query-rte-reference) haben Sie gelernt, wie Sie Inline-Verweise in mehrzeiligen Textfeldern abfragen können. Der Inhalt wurde aus der `description` im `plaintext`-Format abgerufen. Als Nächstes erweitern wir diese Abfrage und verwenden eine Anweisung, um die `description` mit Bedingungen auch im `json`-Format abzurufen.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in das linke Panel ein:

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

   Die oben stehende Abfrage akzeptiert eine weitere Variable (`includeJson`), die vom Typ `Boolean` sein muss und auch als Abfrageanweisung bezeichnet wird. Basierend auf dem in `includeJson` übergebenen booleschen Wert kann eine Anweisung verwendet werden, um Daten aus dem Feld `description` im `json`-Format einzuschließen.

1. Fügen Sie anschließend die folgende JSON-Zeichenfolge in das Panel „Abfragevariablen“ ein:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Führen Sie die Abfrage aus. Sie sollten dasselbe Ergebnis erhalten wie im vorherigen Abschnitt mit der [Anleitung zum Abfragen von Inline-Referenzen in mehrzeiligen Textfeldern](#query-rte-reference).

1. Aktualisieren Sie die `includeJson`-Anweisung auf `true` und führen Sie die Abfrage erneut aus. Das Ergebnis sollte etwa wie folgt aussehen:

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
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
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
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Abfragen nach dem Content-Typ des JSON-Objekts

Im vorherigen Kapitel zur Bearbeitung von Inhaltsfragmenten haben Sie ein JSON-Objekt zum Feld **Wetter nach Saison** hinzugefügt. Rufen wir diese Daten jetzt im Inhaltsfragment „Standort“ ab.

1. Fügen Sie in die GraphiQL-IDE die folgende Abfrage in das linke Panel ein:

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

1. Fügen Sie anschließend die folgende JSON-Zeichenfolge in das Panel „Abfragevariablen“ ein:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Führen Sie die Abfrage aus. Das Ergebnis sollte etwa wie folgt aussehen:

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
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
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
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
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

   Das Feld `weatherBySeason` enthält das JSON-Objekt, das im vorherigen Kapitel hinzugefügt wurde.

## Alle Inhalte gleichzeitig abfragen

Bisher wurden mehrere Abfragen ausgeführt, um die Funktionen der AEM GraphQL-API zu veranschaulichen.

Dieselben Daten können mit einer einzigen Abfrage abgerufen werden. Diese Abfrage wird später in der Client-Anwendung verwendet, um zusätzliche Informationen wie Ort, Team-Name und Team-Mitglieder eines Abenteuers abzurufen:

```graphql
query getAdventureDetailsBySlug($slug: String!) {
  adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
    items {
      _path
      title
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        html
        json
      }
      itinerary {
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
      instructorTeam {
        _metadata {
          stringMetadata {
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
          profilePicture {
            ... on ImageRef {
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
      ... on ImageRef {
        _path
        mimeType
      }
      ... on LocationModel {
        _path
        __typename
      }
    }
  }
}


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben jetzt erweiterte Abfragen zum Erfassen von Daten zu den Inhaltsfragmenten getestet, die Sie im vorherigen Kapitel erstellt haben.

## Nächste Schritte

Im [nächsten Kapitel](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md) erfahren Sie, wie Sie GraphQL-Abfragen persistieren und warum es als Best Practice gilt, persistierte Abfragen in Ihren Anwendungen zu verwenden.
