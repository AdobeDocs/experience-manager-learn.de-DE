---
title: Erweiterte Datenmodellierung mit Fragmentverweisen - Erste Schritte mit AEM Kopflos - GraphQL
description: Beginnen Sie mit Adobe Experience Manager (AEM) und GraphQL. Erfahren Sie, wie Sie mit der Funktion "Fragmentverweis"eine erweiterte Datenmodellierung durchführen und eine Beziehung zwischen zwei verschiedenen Inhaltsfragmenten erstellen können. Erfahren Sie, wie Sie eine GraphQL-Abfrage ändern, um Felder aus einem referenzierten Modell einzuschließen.
sub-product: Assets
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '858'
ht-degree: 2%

---


# Erweiterte Datenmodellierung mit Fragmentverweisen

>[!CAUTION]
>
> Die AEM GraphQL-API für den Versand &quot;Inhaltsfragmente&quot;steht auf Anfrage zur Verfügung.
> Wenden Sie sich an den Support für Adoben, um die API für Ihre AEM als Cloud Service-Programm zu aktivieren.

Es ist möglich, ein Inhaltsfragment aus einem anderen Inhaltsfragment zu referenzieren. Dadurch kann ein Benutzer komplexe Datenmodelle mit Beziehungen zwischen Fragmenten erstellen.

In diesem Kapitel aktualisieren Sie das Adventure-Modell, um mithilfe des Felds **Fragmentverweis** einen Verweis auf das Mitarbeiter-Modell einzuschließen. Außerdem erfahren Sie, wie Sie eine GraphQL-Abfrage ändern, um Felder aus einem referenzierten Modell einzuschließen.

## Voraussetzungen

Es handelt sich um ein mehrteiliges Tutorial, bei dem davon ausgegangen wird, dass die in den vorherigen Teilen beschriebenen Schritte abgeschlossen wurden.

## Ziele

In diesem Kapitel lernen wir Folgendes:

* Aktualisieren eines Inhaltsfragmentmodells zur Verwendung des Fragmentverweisfelds
* Erstellen einer GraphQL-Abfrage, die Felder eines referenzierten Modells zurückgibt

## hinzufügen eines Fragmentverweises {#add-fragment-reference}

Aktualisieren Sie das Adventure Content Fragment Model, um eine Referenz zum Mitarbeiter-Modell hinzuzufügen.

1. Öffnen Sie einen neuen Browser und navigieren Sie zu AEM.
1. Navigieren Sie im Menü **AEM Beginn** zu **Tools** > **Assets** > **Inhaltsfragmentmodelle** > **WKND-Site**.
1. Öffnen Sie das Inhaltsfragmentmodell **Adventure**

   ![Öffnen Sie das Adventure Content Fragment-Modell](assets/fragment-references/adventure-content-fragment-edit.png)

1. Ziehen Sie unter **Datentypen** ein Feld **Fragmentverweis** in das Hauptbedienfeld.

   ![hinzufügen Feld &quot;Fragmentverweis&quot;](assets/fragment-references/add-fragment-reference-field.png)

1. Aktualisieren Sie die **Eigenschaften** für dieses Feld mit folgendem Code:

   * Rendern als - `fragmentreference`
   * Feldbeschriftung - **Adventure-Mitarbeiter**
   * Eigenschaftsname - `adventureContributor`
   * Modelltyp - Wählen Sie das Modell **Mitarbeiter** aus.
   * Stammverzeichnis - `/content/dam/wknd`

   ![Fragmentverweiseigenschaften](assets/fragment-references/fragment-reference-properties.png)

   Der Eigenschaftsname `adventureContributor` kann jetzt als Verweis auf ein Inhaltsfragment des Beitragenden verwendet werden.

1. Speichern Sie die Änderungen am Modell.

## Zuweisen eines Beitrags zu einem Abenteuer

Nachdem das Adventure Content Fragment-Modell aktualisiert wurde, können wir ein vorhandenes Fragment bearbeiten und auf einen Mitarbeiter verweisen. Beachten Sie, dass sich die Bearbeitung des Inhaltsfragmentmodells *auf* alle vorhandenen Inhaltsfragmente auswirkt, die daraus erstellt wurden.

1. Navigieren Sie zu **Assets** > **Dateien** > **WKND-Site** > **Englisch** > **Abenteuer** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp).**

   ![Ordner &quot;Bali Surf Camp&quot;](assets/setup/bali-surf-camp-folder.png)

1. Klicken Sie in das Inhaltsfragment **Bali Surf Camp**, um den Inhaltsfragment-Editor zu öffnen.
1. Aktualisieren Sie das Feld **Adventure Contributor** und wählen Sie einen Mitarbeiter, indem Sie auf das Ordnersymbol klicken.

   ![Wählen Sie Stacey Roswells als Mitarbeiter](assets/fragment-references/stacey-roswell-contributor.png)

   *Pfad zu einem Beitragsfragment auswählen*

   ![gefüllt Pfad zum Mitarbeiter](assets/fragment-references/populated-path.png)

   Beachten Sie, dass nur mit dem **Mitarbeiter**-Modell erstellte Fragmente ausgewählt werden können.

1. Speichern Sie die Änderungen am Fragment.

1. Wiederholen Sie die oben genannten Schritte, um Abenteuern wie [Yosemite Backpacking](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) und [Colorado Rock Climbing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing) einen Mitarbeiter zuzuweisen.

## Abfrage verschachteltes Inhaltsfragment mit GraphiQL

Führen Sie anschließend eine Abfrage für ein Abenteuer durch und fügen Sie verschachtelte Eigenschaften des referenzierten Mitarbeiter-Modells hinzu. Wir werden das GraphiQL-Tool verwenden, um die Syntax der Abfrage schnell zu überprüfen.

1. Navigieren Sie zum GraphiQL-Tool in AEM: [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. Geben Sie die folgende Abfrage ein:

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   Die obige Abfrage ist für ein einziges Abenteuer nach seinem Pfad. Die `adventureContributor`-Eigenschaft verweist auf das Mitarbeiter-Modell und wir können dann Eigenschaften vom verschachtelten Inhaltsfragment anfordern.

1. Führen Sie die Abfrage aus, und Sie sollten ein Ergebnis wie das folgende erhalten:

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. Experimentieren Sie mit anderen Abfragen wie `adventureList` und fügen Sie Eigenschaften für das referenzierte Inhaltsfragment unter `adventureContributor` hinzu.

## Aktualisieren der React-App zur Anzeige des Beitragsinhalts

Aktualisieren Sie anschließend die Abfragen, die von der React-Anwendung verwendet werden, um den neuen Mitarbeiter einzuschließen und Informationen zum Mitarbeiter als Teil der Adventure-Details-Ansicht anzuzeigen.

1. Öffnen Sie die WKND GraphQL React-App in Ihrer IDE.

1. Öffnen Sie die Datei `src/components/AdventureDetail.js`.

   ![Adventure Detail-IDE](assets/fragment-references/adventure-detail-ide.png)

1. Suchen Sie die Funktion `adventureDetailQuery(_path)`. Die `adventureDetailQuery(..)`-Funktion umschließt einfach eine GraphicQL-Abfrage, die AEM `<modelName>ByPath`-Syntax verwendet, um ein einzelnes Inhaltsfragment, das durch seinen JCR-Pfad identifiziert wird, Abfrage.

1. Aktualisieren Sie die Abfrage, um Informationen zum referenzierten Mitarbeiter einzuschließen:

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
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
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   Mit diesem Update werden weitere Eigenschaften zu `adventureContributor`, `fullName`, `occupation` und `pictureReference` in die Abfrage aufgenommen.

1. Inspect Sie die Komponente `Contributor`, die in der Datei `AdventureDetail.js` unter `function Contributor(...)` eingebettet ist. Diese Komponente gibt den Namen, die Tätigkeit und das Bild des Beitragenden wieder, wenn die Eigenschaften vorhanden sind.

   Die Komponente `Contributor` wird in der `AdventureDetail(...)` `return`-Methode referenziert:

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. Speichern Sie die Änderungen in der Datei.
1. Beginn der React-App, falls noch nicht ausgeführt:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Navigieren Sie zu [http://localhost:3000](http://localhost:3000/) und klicken Sie auf ein Abenteuer, auf das ein verwiesener Mitarbeiter verweist. Sie sollten nun die Angaben zum Mitarbeiter unter dem **Itinerary** sehen:

   ![Mitarbeiter in der App hinzugefügt](assets/fragment-references/contributor-added-detail.png)

## Zusätzliche Ressourcen

Weitere Informationen zu Inhaltsfragmenten und GraphQL finden Sie in den folgenden Ressourcen:

* [Versand ohne Inhalt mit Inhaltsfragmenten mit GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API zur Verwendung mit Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)

## Herzlichen Glückwunsch!{#congratulations}

Herzlichen Glückwunsch! Sie haben ein vorhandenes Inhaltsfragmentmodell aktualisiert, um mit dem Feld **Fragmentverweis** auf ein verschachteltes Inhaltsfragment zu verweisen. Sie haben auch gelernt, wie Sie eine GraphQL-Abfrage ändern, um Felder aus einem referenzierten Modell einzuschließen.
