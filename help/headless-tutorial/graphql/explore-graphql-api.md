---
title: GraphQL-APIs - Erste Schritte mit AEM Headless - GraphQL
description: Beginnen Sie mit Adobe Experience Manager (AEM) und GraphQL. Entdecken Sie AEM GraphQL APIs mit der integrierten GrapiQL IDE. Erfahren Sie, wie AEM basierend auf einem Inhaltsfragmentmodell automatisch ein GraphQL-Schema generiert. Experimentieren Sie mit der Erstellung grundlegender Abfragen mit der GraphQL-Syntax.
sub-product: Assets
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: null
thumbnail: null
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---


# GraphQL APIs {#explore-graphql-apis}

>[!CAUTION]
>
> Die AEM GraphQL API für Content Fragment Versand wird Anfang 2021 veröffentlicht.
> Die zugehörige Dokumentation steht zu Vorschauen zur Verfügung.

Die GraphQL API von AEM bietet eine leistungsstarke Abfrage, um Daten von Inhaltsfragmenten für nachgeschaltete Anwendungen verfügbar zu machen. Inhaltsfragmentmodelle definieren das Schema, das von Inhaltsfragmenten verwendet wird. Bei jeder Erstellung oder Aktualisierung eines Inhaltsfragmentmodells wird das Schema übersetzt und dem &quot;Diagramm&quot;, aus dem die GraphQL-API besteht, hinzugefügt.

In diesem Kapitel werden wir einige gängige GraphQL-Abfragen untersuchen, um Inhalte zu sammeln. In AEM integriert ist eine IDE mit dem Namen [GraphiQL](https://github.com/graphql/graphiql). Mit der GraphiQL IDE können Sie die zurückgegebenen Abfragen und Daten schnell testen und verfeinern. GraphiQL bietet außerdem einen einfachen Zugriff auf die Dokumentation, sodass Sie leicht erkennen und verstehen können, welche Methoden verfügbar sind.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Lernprogramm und es wird davon ausgegangen, dass die in [Inhaltsfragmente erstellen](./author-content-fragments.md) beschriebenen Schritte abgeschlossen wurden.

## Ziele {#objectives}

* Hier erfahren Sie, wie Sie mit dem GraphiQL-Tool eine Abfrage mit GraphQL-Syntax erstellen.
* Erfahren Sie, wie Sie eine Liste von Inhaltsfragmenten und einem Inhaltsfragment Abfrage haben.
* Erfahren Sie, wie Sie bestimmte Datenattribute filtern und anfordern.
* Erfahren Sie, wie Sie eine Variation eines Inhaltsfragments Abfrage haben.
* Erfahren Sie, wie Sie einer Abfrage mehrerer Inhaltsfragmentmodelle beitreten.

## Abfrage einer Liste von Inhaltsfragmenten {#query-list-cf}

Eine gängige Anforderung ist die Abfrage mehrerer Inhaltsfragmente.

1. Navigieren Sie zur GraphiQL IDE unter [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html).
1. Fügen Sie die folgende Abfrage in den linken Bereich ein (unterhalb der Liste der Kommentare):

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. Drücken Sie die Taste **Play** im oberen Menü, um die Abfrage auszuführen. Sie sollten die Ergebnisse der Inhaltsfragmente der Mitarbeiter aus dem vorherigen Kapitel sehen:

   ![Ergebnisse der mitwirkenden Liste](assets/explore-graphql-api/contributorlist-results.png)

1. Positionieren Sie den Cursor unter dem Text `_path` und geben Sie **STRG+Space** ein, um Codehinweise auszulösen. hinzufügen `fullName` und `occupation` zur Abfrage.

   ![Abfrage mit Code-Treffern aktualisieren](assets/explore-graphql-api/update-query-codehinting.png)

1. Führen Sie die Abfrage erneut aus, indem Sie auf die Schaltfläche **Abspielen** klicken. Die Ergebnisse sollten die zusätzlichen Eigenschaften von `fullName` und `occupation` enthalten.

   ![Vollname- und Berufsergebnisse](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` und  `occupation` sind einfache Eigenschaften. Rufen Sie im Kapitel [Definieren von Inhaltsfragmentmodellen](./content-fragment-models.md) auf, dass `fullName` und `occupation` die Werte sind, die beim Definieren des **Eigenschaftsnamens** der entsprechenden Felder verwendet werden.

1. `pictureReference` und  `biography` stellen komplexere Felder dar. Aktualisieren Sie die Abfrage mit den folgenden Elementen, um Daten zu den Feldern `pictureReference` und `biography` zurückzugeben.

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biography {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biography` ist ein mehrzeiliges Textfeld und die GraphQL API ermöglicht es uns, eine Vielzahl von Formaten für die Ergebnisse wie  `html`,  `markdown`,  `json` oder  `plaintext`.

   `pictureReference` ist ein Inhaltsverweis und es wird erwartet, dass es ein Bild ist, daher wird ein integriertes  `ImageRef` Objekt verwendet. Auf diese Weise können wir zusätzliche Daten über das referenzierende Bild anfordern, z. B. `width` und `height`.

1. Experimentieren Sie mit der Abfrage nach einer Liste von **Abenteuer**. Führen Sie die folgende Abfrage aus:

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   Es sollte eine Liste von **Abenteuer** zurückgegeben werden. Experimentieren Sie mit der Abfrage, indem Sie der Tabelle weitere Felder hinzufügen.

## Filtern einer Liste von Inhaltsfragmenten {#filter-list-cf}

Als Nächstes sehen wir uns an, wie die Ergebnisse basierend auf einem Eigenschaftswert in eine Untergruppe von Inhaltsfragmenten gefiltert werden können.

1. Geben Sie die folgende Abfrage in die GraphiQL-Benutzeroberfläche ein:

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   Die obige Abfrage führt eine Suche nach allen Mitwirkenden im System durch. Der am Anfang der Abfrage hinzugefügte Filter führt einen Vergleich mit dem Feld `occupation` und der Zeichenfolge &quot;**Fotograf**&quot;durch.

1. Führen Sie die Abfrage aus. Es wird erwartet, dass nur ein einzelner **Mitarbeiter** zurückgegeben wird.
1. Geben Sie die folgende Abfrage zur Abfrage einer Liste von **Abenteuer** ein, wobei `adventureActivity` **nicht** gleich **&quot;Surfen&quot;** ist:

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. Führen Sie die Abfrage aus und überprüfen Sie die Ergebnisse. Beachten Sie, dass keines der Ergebnisse ein `adventureType` gleich **&quot;Surfen&quot;** enthält.

Es gibt noch viele andere Optionen zum Filtern und Erstellen komplexer Abfragen, obige nur einige Beispiele.

## Abfrage eines einzelnen Inhaltsfragments {#query-single-cf}

Es ist auch möglich, ein einzelnes Inhaltsfragment direkt Abfrage. Inhalte in AEM werden hierarchisch gespeichert, und die eindeutige Kennung für ein Fragment basiert auf dem Pfad des Fragments. Wenn das Ziel darin besteht, Daten über ein einzelnes Fragment zurückzugeben, wird empfohlen, den Pfad und die Abfrage des Modells direkt zu verwenden. Die Verwendung dieser Syntax bedeutet, dass die Komplexität der Abfrage sehr gering ist und zu einem schnelleren Ergebnis führt.

1. Geben Sie die folgende Abfrage im GraphiQL-Editor ein:

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

1. Führen Sie die Abfrage aus und stellen Sie fest, dass das Einzelergebnis für das Fragment **Stacey Roswells** zurückgegeben wird.

   In der vorherigen Übung haben Sie mithilfe eines Filters eine Liste der Ergebnisse eingegrenzt. Sie können eine ähnliche Syntax verwenden, um nach Pfad zu filtern. Die obige Syntax wird jedoch aus Leistungsgründen bevorzugt.

1. Rufen Sie im Kapitel [Inhaltsfragmente erstellen](./author-content-fragments.md) auf, dass eine **Zusammenfassung**-Variante für **statische Roswells** erstellt wurde. Aktualisieren Sie die Abfrage, um die Variation **Zusammenfassung** zurückzugeben:

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

   Obwohl die Variation mit **Zusammenfassung** benannt wurde, werden Variationen in Kleinbuchstaben beibehalten und deshalb wird `summary` verwendet.

1. Führen Sie die Abfrage aus und stellen Sie fest, dass das `biography`-Feld ein viel kürzeres `html`-Ergebnis enthält.

## Abfrage für mehrere Inhaltsfragmentmodelle {#query-multiple-models}

Es ist auch möglich, separate Abfragen in einer einzigen Abfrage zu kombinieren. Dies ist nützlich, um die Anzahl der HTTP-Anforderungen zu minimieren, die zur Stromversorgung der Anwendung erforderlich sind. Beispielsweise kann die *Home*-Ansicht einer Anwendung Inhalte anzeigen, die auf **zwei** unterschiedlichen Inhaltsfragmentmodellen basieren. Statt zwei separate Abfragen **auszuführen, können wir die Abfragen zu einer einzigen Anforderung kombinieren.**

1. Geben Sie die folgende Abfrage im GraphiQL-Editor ein:

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. Führen Sie die Abfrage aus und stellen Sie sicher, dass die Ergebnismenge Daten von **Adventures** und **Mitarbeiter** enthält:

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade mehrere GraphQL Abfragen erstellt und ausgeführt!

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [AEM von einer React-App](./graphql-and-external-app.md) werden Sie untersuchen, wie eine externe Anwendung AEM GraphQL-Endpunkte Abfrage werden kann. Die externe App, die die WKND GraphQL React-Beispielanwendung zum Hinzufügen von GraphQL-Abfragen zum Filtern ändert, sodass der Benutzer der App Abenteuer nach Aktivität filtern kann. Sie werden außerdem mit einer grundlegenden Fehlerbehandlung vertraut gemacht.
