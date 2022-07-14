---
title: GraphQL-APIs - Erste Schritte mit AEM Headless - GraphQL
description: Erste Schritte mit Adobe Experience Manager (AEM) und GraphQL. Erkunden Sie AEM GraphQL-APIs mit der integrierten GrapiQL-IDE. Erfahren Sie, wie AEM basierend auf einem Inhaltsfragmentmodell automatisch ein GraphQL-Schema generiert. Experimentieren Sie mit der Erstellung grundlegender Abfragen mithilfe der GraphQL-Syntax.
version: Cloud Service
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
source-git-commit: a49e56b6f47e477132a9eee128e62fe5a415b262
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# GraphQL-APIs {#explore-graphql-apis}

Die GraphQL-API von AEM bietet eine leistungsstarke Abfragesprache, um Daten von Inhaltsfragmenten für nachgelagerte Anwendungen verfügbar zu machen. Inhaltsfragmentmodelle definieren das Datenschema, das von Inhaltsfragmenten verwendet wird. Jedes Mal, wenn ein Inhaltsfragmentmodell erstellt oder aktualisiert wird, wird das Schema übersetzt und zum &quot;Diagramm&quot;hinzugefügt, aus dem die GraphQL-API besteht.

In diesem Kapitel werden wir einige gängige GraphQL-Abfragen untersuchen, um Inhalte mithilfe einer IDE namens [GraphiQL](https://github.com/graphql/graphiql). Mit der GraphiQL IDE können Sie die zurückgegebenen Abfragen und Daten schnell testen und verfeinern. GraphiQL bietet auch einen einfachen Zugang zur Dokumentation, sodass man leicht lernen und verstehen kann, welche Methoden verfügbar sind.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im [Erstellen von Inhaltsfragmenten](./author-content-fragments.md) wurden abgeschlossen.

## Ziele {#objectives}

* Erfahren Sie, wie Sie mit dem GraphQL-Tool eine Abfrage mithilfe der GraphQL-Syntax erstellen können.
* Erfahren Sie, wie Sie eine Liste von Inhaltsfragmenten und ein einzelnes Inhaltsfragment abfragen.
* Erfahren Sie, wie Sie bestimmte Datenattribute filtern und anfordern.
* Erfahren Sie, wie Sie eine Abfrage mehrerer Inhaltsfragmentmodelle verbinden.
* Erfahren Sie, wie Sie die GraphQL-Abfrage beibehalten.

## Aktivieren eines GraphQL-Endpunkts {#enable-graphql-endpoint}

Es muss ein GraphQL-Endpunkt konfiguriert werden, um GraphQL-API-Abfragen für Inhaltsfragmente zu aktivieren.

1. Navigieren Sie im Bildschirm AEM Start zu **Instrumente** > **Allgemein** > **GraphQL**.

   ![Navigieren zum GraphQL-Endpunkt](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. Tippen **Erstellen** in der oberen rechten Ecke. Geben Sie im Dialogfeld die folgenden Werte ein:

   * Name*: **Mein Projektendpunkt**.
   * GraphQL-Schema verwenden, das von ... * bereitgestellt wird: **Mein Projekt**

   ![GraphQL-Endpunkt erstellen](assets/explore-graphql-api/create-graphql-endpoint.png)

   Tippen **Erstellen** , um den Endpunkt zu speichern.

   GraphQL-Endpunkte, die basierend auf einer Projektkonfiguration erstellt wurden, ermöglichen nur Abfragen für Modelle, die zu diesem Projekt gehören. In diesem Fall stellt nur die **Person** und **Team** -Modelle verwendet werden.

   >[!NOTE]
   >
   > Es kann auch ein globaler Endpunkt erstellt werden, der Abfragen von Modellen über mehrere Projekte hinweg ermöglicht. Wenn Sie beispielsweise eine Abfrage mit den Modellen in der **WKND Shared** und im **Mein Projekt**. Dies sollte mit Vorsicht und nur bei Bedarf verwendet werden, da dadurch die Umgebung möglicherweise zusätzlichen Sicherheitslücken ausgesetzt wird.

1. Es sollten nun zwei GraphQL-Endpunkte angezeigt werden, die in Ihrer Umgebung aktiviert sind (vorausgesetzt, Sie haben den freigegebenen WKND-Inhalt installiert).

   ![Aktivierte grafische Endpunkte](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## Verwenden der GraphiQL-IDE

Die [GraphiQL-Tool](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) ermöglicht es Entwicklern, Abfragen für Inhalte in der aktuellen AEM-Umgebung zu erstellen und zu testen. Mit dem GraphQL-Tool können Benutzer auch **persist** oder speichern Sie Abfragen, die von Clientanwendungen in einer Produktionseinstellung verwendet werden sollen.

Erkunden Sie als Nächstes die Leistungsfähigkeit AEM GraphQL-API mit der integrierten GraphiQL-IDE.

1. Navigieren Sie im Bildschirm AEM Start zu **Instrumente** > **Allgemein** > **GraphQL-Abfrage-Editor**.

   ![Navigieren Sie zur GraphiQL-IDE.](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > Bei älteren Versionen von AEM ist die GraphiQL IDE möglicherweise nicht integriert. Sie kann manuell installiert werden, indem Sie den folgenden Schritten folgen: [instructions](#install-graphiql).

1. Setzen Sie in der oberen rechten Ecke die **Endpunkt** nach **Mein Projektendpunkt**.

   ![GraphQL-Endpunkt festlegen](assets/explore-graphql-api/set-my-project-endpoint.png)

   Dadurch werden alle Abfragen auf Modelle angewendet, die in der **Mein Projekt** Projekt. Beachten Sie, dass es auch einen -Endpunkt für **WKND Shared**.

### Liste von Inhaltsfragmenten abfragen {#query-list-cf}

Eine gängige Anforderung besteht darin, mehrere Inhaltsfragmente abzufragen.

1. Fügen Sie die folgende Abfrage in den Hauptbereich ein (ersetzen Sie die Liste der Kommentare):

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. Drücken Sie die **Play** im oberen Menü, um die Abfrage auszuführen. Sie sollten die Ergebnisse der Inhaltsfragmente aus dem vorherigen Kapitel sehen:

   ![Ergebnisse der Personenliste](assets/explore-graphql-api/all-teams-list.png)

1. Positionieren Sie den Cursor unter dem `title` Text und Eingabe **STRG+Leertaste** Trigger-Code-Hinweise. Hinzufügen `shortname` und `description` zur Abfrage hinzufügen.

   ![Abfrage mit Code-Hash aktualisieren](assets/explore-graphql-api/update-query-codehinting.png)

1. Führen Sie die Abfrage erneut aus, indem Sie die **Play** und Sie sollten sehen, dass die Ergebnisse die zusätzlichen Eigenschaften von `shortname` und `description`.

   ![Kurznamen- und Beschreibungsergebnisse](assets/explore-graphql-api/updated-query-shortname-description.png)

   Die `shortname` ist eine einfache Eigenschaft und `description` ist ein mehrzeiliges Textfeld, und die GraphQL-API ermöglicht es uns, eine Vielzahl von Formaten für die Ergebnisse auszuwählen, z. B. `html`, `markdown`, `json` oder `plaintext`.

### Abfrage für verschachtelte Fragmente

Als Nächstes experimentieren Sie mit der Abfrage, indem Sie verschachtelte Fragmente abrufen. Denken Sie daran, dass die **Team** -Modell referenziert die **Person** -Modell.

1. Aktualisieren Sie die Abfrage, um die `teamMembers` -Eigenschaft. Erinnern Sie sich daran, dass dies eine **Fragmentverweis** zum Personenmodell. Eigenschaften des Personen-Modells können zurückgegeben werden:

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   JSON-Antwort:

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   Die Möglichkeit, mit verschachtelten Fragmenten abzufragen, ist eine leistungsstarke AEM GraphQL-API. In diesem einfachen Beispiel ist die Verschachtelung nur zwei Ebenen tief. Es ist jedoch möglich, Fragmente noch weiter zu verschachteln. Wenn beispielsweise **Adresse** mit einem **Person** Es wäre möglich, Daten aus allen drei Modellen in einer einzigen Abfrage zurückzugeben.

### Filtern einer Liste von Inhaltsfragmenten {#filter-list-cf}

Als Nächstes sehen wir uns an, wie es möglich ist, die Ergebnisse basierend auf einem Eigenschaftswert nach einer Untergruppe von Inhaltsfragmenten zu filtern.

1. Geben Sie die folgende Abfrage in die GraphiQL-Benutzeroberfläche ein:

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   Die obige Abfrage führt eine Suche nach allen Personen-Fragmenten im System durch. Der am Anfang der Abfrage hinzugefügte Filter führt einen Vergleich der `name` -Feld und der Variablenzeichenfolge `$name`.

1. Im **Abfragevariablen** -Bereich geben Sie Folgendes ein:

   ```json
   {"name": "John Doe"}
   ```

1. Führen Sie die Abfrage aus. Es wird erwartet, dass nur **Personen** wird mit dem Wert &quot;John Doe&quot;zurückgegeben.

   ![Verwenden von Abfragevariablen zum Filtern](assets/explore-graphql-api/using-query-variables-filter.png)

   Es gibt viele weitere Optionen zum Filtern und Erstellen komplexer Abfragen, siehe [Verwendung von GraphQL mit AEM - Beispielinhalt und Abfragen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html?lang=de).

1. Erweiterung der obigen Abfrage zum Abrufen des Profilbilds

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   Die `profilePicture` ist eine Inhaltsreferenz, und es wird erwartet, dass es sich um ein Bild handelt. Daher ist es integriert `ImageRef` -Objekt verwendet wird. Dadurch können wir zusätzliche Daten zum Bild anfordern, auf das verwiesen wird, z. B. die `width` und `height`.

### Einzelnes Inhaltsfragment abfragen {#query-single-cf}

Es ist auch möglich, ein einzelnes Inhaltsfragment direkt abzufragen. Der Inhalt in AEM wird hierarchisch gespeichert und die eindeutige Kennung für ein Fragment basiert auf dem Pfad des Fragments.

1. Geben Sie die folgende Abfrage im Editor &quot;GraphiQL&quot;ein:

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. Geben Sie Folgendes für die **Abfragevariablen**:

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. Führen Sie die Abfrage aus und beobachten Sie, dass das einzelne Ergebnis zurückgegeben wird.

## Dauerhafte Abfragen {#persist-queries}

Sobald ein Entwickler mit der zurückgegebenen Abfrage und Daten zufrieden ist, besteht der nächste Schritt darin, die Abfrage zu speichern oder zu AEM. [Beständige Abfragen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html) ist der bevorzugte Mechanismus zur Bereitstellung der GraphQL-API für Client-Anwendungen. Nachdem eine Abfrage persistiert wurde, kann sie mithilfe einer GET-Anfrage angefordert und in den Dispatcher- und CDN-Ebenen zwischengespeichert werden. Die Leistung persistenter Abfragen ist viel besser. Zusätzlich zu den Leistungsvorteilen stellen persistente Abfragen sicher, dass zusätzliche Daten nicht versehentlich Client-Anwendungen zur Verfügung gestellt werden. Weitere Informationen [Persistierte Abfragen finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html).

Speichern Sie anschließend zwei einfache Abfragen, die im nächsten Kapitel verwendet werden.

1. Geben Sie in die GraphiQL IDE die folgende Abfrage ein:

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   Überprüfen Sie, ob die Abfrage funktioniert.

1. Nächstes Tippen **Speichern unter** und eingeben `all-teams` als **Abfragename**.

   Die Abfrage sollte jetzt unter **Beständige Abfragen** in der linken Leiste.

   ![Alle Teams - beständige Abfrage](assets/explore-graphql-api/all-teams-persisted-query.png)
1. Tippen Sie anschließend auf die Sendungen **...** neben der persistenten Abfrage und tippen Sie auf **URL kopieren** , um den Pfad in die Zwischenablage zu kopieren.

   ![Persistente Abfrage-URL kopieren](assets/explore-graphql-api/copy-persistent-query-url.png)

1. Öffnen Sie eine neue Registerkarte und fügen Sie den kopierten Pfad in Ihren Browser ein:

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   Sie sollte dem obigen Pfad ähnlich aussehen. Sie sollten die JSON-Ergebnisse der zurückgegebenen Abfrage sehen.

   Aufschlüsseln der URL:

   | Name | Beschreibung |
   | ---------|---------- |
   | `/graphql/execute.json` | Persistenter Abfrageendpunkt |
   | `/my-project` | Projektkonfiguration für `/conf/my-project` |
   | `/all-teams` | Name der persistenten Abfrage |

1. Kehren Sie zur GraphiQL-IDE zurück und verwenden Sie die Plusschaltfläche . **+** um die NEU-Abfrage zu beibehalten

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. Speichern Sie die Abfrage als: **person-by-name**.
1. Sie sollten zwei persistente Abfragen speichern:

   ![Abgeschlossene persistente Abfragen](assets/explore-graphql-api/final-persisted-queries.png)

## Lösungsdateien {#solution-files}

Laden Sie den Inhalt, die Modelle und die persistenten Abfragen herunter, die in den letzten drei Kapiteln erstellt wurden: [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip)

## Persistente WKND-Abfragen durchsuchen (optional) {#explore-wknd-content-fragments}

Wenn Sie [WKND Shared Sample Content installiert](./overview.md#install-sample-content) Sie können persistente Abfragen wie Abenteuer-alle, Abenteuer-für-Aktivität, Abenteuer-Pfade etc. überprüfen und ausführen.

![WKND - persistente Abfragen](assets/explore-graphql-api/wknd-persisted-queries.png)


## Zusätzliche Ressourcen

Weitere Beispiele für GraphQL-Abfragen finden Sie unter: [Verwendung von GraphQL mit AEM - Beispielinhalt und Abfragen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gerade mehrere GraphQL-Abfragen erstellt und ausgeführt!

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [React-App erstellen](./graphql-and-react-app.md)werden Sie untersuchen, wie eine externe Anwendung GraphQL-Endpunkte abfragen AEM und diese beiden beibehaltenen Abfragen nutzen kann. Außerdem werden Sie mit der grundlegenden Fehlerbehandlung vertraut gemacht.

## Installieren des GraphiQL-Tools (optional) {#install-graphiql}

Für einige Versionen von AEM muss das GraphiQL IDE Tool manuell installiert werden. Befolgen Sie die folgenden Anweisungen, um manuell zu installieren:

1. Gehen Sie zum **[Software-Verteilungsportal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Suchen Sie nach „GraphiQL“ (stellen Sie sicher, dass Sie das **i** in **GraphiQL** einschließen.
1. Laden Sie die neueste Version **GraphiQL Content Package v.x.x.x** herunter.

   ![Herunterladen des GraphiQL-Pakets](assets/explore-graphql-api/software-distribution.png)

   Die ZIP-Datei ist ein AEM Paket, das direkt installiert werden kann.

1. Gehen Sie im **AEM-Startmenü** zu **Tools** > **Implementierung** > **Pakete**.
1. Klicken Sie auf **Paket hochladen** und wählen Sie das im vorherigen Schritt heruntergeladene Paket aus. Klicken Sie auf **Installieren**, um das Paket zu installieren.

   ![Installieren des GraphiQL-Pakets](assets/explore-graphql-api/install-graphiql-package.png)
