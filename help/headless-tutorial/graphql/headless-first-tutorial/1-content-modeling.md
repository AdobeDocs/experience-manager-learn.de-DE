---
title: Inhaltsmodellierung - AEM erstes Tutorial ohne Headless
description: Erfahren Sie, wie Sie Inhaltsfragmente nutzen, Fragmentmodelle erstellen und GraphQL-Endpunkte in AEM verwenden.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 6e5e3cb4-9a47-42af-86af-da33fd80cb47
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---

# Inhaltsmodellierung

Willkommen beim Tutorial-Kapitel zu Inhaltsfragmenten und GraphQL-Endpunkten in Adobe Experience Manager (AEM). Wir behandeln die Nutzung von Inhaltsfragmenten, das Erstellen von Fragmentmodellen und die Verwendung von GraphQL-Endpunkten in AEM.

Inhaltsfragmente bieten einen strukturierten Ansatz für die kanalübergreifende Verwaltung von Inhalten, der Flexibilität und Wiederverwendbarkeit bietet. Die Aktivierung von Inhaltsfragmenten in AEM ermöglicht die Erstellung modularer Inhalte, wodurch Konsistenz und Anpassungsfähigkeit verbessert werden.

Zunächst führen wir Sie durch die Aktivierung von Inhaltsfragmenten in AEM, die die erforderlichen Konfigurationen und Einstellungen für eine nahtlose Integration abdecken.

Als Nächstes werden wir die Erstellung von Fragmentmodellen behandeln, die Struktur und Attribute definieren. Erfahren Sie, wie Sie Modelle entwerfen, die auf Ihre Inhaltsanforderungen abgestimmt sind, und sie effektiv verwalten.

Anschließend werden wir die Erstellung von Inhaltsfragmenten aus den Modellen demonstrieren und schrittweise Anleitungen zum Authoring und Publishing geben.

Darüber hinaus werden wir die Definition AEM GraphQL-Endpunkte untersuchen. GraphQL ruft effizient Daten aus AEM ab. Wir richten Endpunkte ein und konfigurieren sie, um die gewünschten Daten verfügbar zu machen. Beständige Abfragen optimieren die Leistung und Zwischenspeicherung.

Während des Tutorials werden wir Erklärungen, Codebeispiele und praktische Tipps bereitstellen. Am Ende verfügen Sie über die Fertigkeiten, Inhaltsfragmente zu aktivieren, Fragmentmodelle zu erstellen, Fragmente zu generieren und AEM GraphQL-Endpunkte und persistente Abfragen zu definieren. Fangen wir an!

## Kontextabhängige Konfiguration

1. Navigieren Sie zu __Tools > Konfigurationsbrowser__ , um eine Konfiguration für das Headless-Erlebnis zu erstellen.

   ![Ordner erstellen](./assets/1/create-configuration.png)

   Stellen Sie eine __title__ und __name__ und aktivieren Sie __Persistente GraphQL-Abfragen__ und __Inhaltsfragmentmodelle__.


## Inhaltsfragmentmodelle

1. Navigieren Sie zu __Tools > Inhaltsfragmentmodelle__ und wählen Sie den Ordner mit dem Namen der in Schritt 1 erstellten Konfiguration aus.

   ![Modellordner](./assets/1/model-folder.png)

1. Wählen Sie im Ordner die Option __Erstellen__ und benennen Sie das Modell. __Teaser__. Fügen Sie die folgenden Datentypen zum __Teaser__ -Modell.

   | Datentyp | Name | Erforderlich | Optionen |
   |----------|------|----------|---------|
   | Inhaltsreferenz | Asset | ja | Fügen Sie bei Bedarf ein Standardbild hinzu. Beispiel: /content/dam/wknd-headless/assets/AdobeStock_307513975.mp4 |
   | Einzeilentext | Titel | ja |
   | Einzeilentext | Vortitel | nein |
   | Mehrzeilentext | Beschreibung | nein | Stellen Sie sicher, dass der Standardtyp Rich-Text ist. |
   | Aufzählung | Stil | ja | Als Dropdown-Liste rendern. Die Optionen sind Hero -> Hero und Vorgestellt -> Vorgestellt |

   ![Teaser-Modell](./assets/1/teaser-model.png)

1. Erstellen Sie im Ordner ein zweites Modell mit dem Namen __Angebot__. Klicken Sie auf Erstellen , geben Sie dem Modell den Namen &quot;Angebot&quot;und fügen Sie die folgenden Datentypen hinzu:

   | Datentyp | Name | Erforderlich | Optionen |
   |----------|------|----------|---------|
   | Inhaltsreferenz | Asset | ja | Fügen Sie ein Standardbild hinzu. z. B.: `/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | Mehrzeilentext | Beschreibung | nein |  |
   | Mehrzeilentext | Artikel | nein |  |

   ![Angebotsmodell](./assets/1/offer-model.png)

1. Erstellen Sie im Ordner ein drittes Modell mit dem Namen __Bildliste__. Klicken Sie auf Erstellen , geben Sie dem Modell den Namen &quot;Bildliste&quot;und fügen Sie die folgenden Datentypen hinzu:

   | Datentyp | Name | Erforderlich | Optionen |
   |----------|------|----------|---------|
   | Fragmentreferenz | Listenelemente | ja | Als mehrere Felder wiedergeben. Zulässiges Inhaltsfragmentmodell ist Angebot. |

   ![Bildlistenmodell](./assets/1/imagelist-model.png)

## Inhaltsfragmente

1. Navigieren Sie jetzt zu Assets und erstellen Sie einen Ordner für die neue Site. Klicken Sie auf Erstellen und benennen Sie den Ordner.

   ![Ordner hinzufügen](./assets/1/create-folder.png)

1. Nachdem der Ordner erstellt wurde, wählen Sie den Ordner aus und öffnen Sie seinen __Eigenschaften__.
1. Im Ordner __Cloud-Konfigurationen__ Registerkarte die Konfiguration auswählen [früher erstellt](#enable-content-fragments-and-graphql).

   ![Asset-Ordner AEM Headless-Cloud-Konfiguration](./assets/1/cloud-config.png)

   Klicken Sie in den neuen Ordner und erstellen Sie einen Teaser. Klicks __Erstellen__ und __Inhaltsfragment__ und wählen Sie die __Teaser__ -Modell. Benennen Sie das Modell __Hero__ und klicken __Erstellen__.

   | Name | Anmerkungen |
   |----------|------|
   | Asset | Behalten Sie den Standardwert bei oder wählen Sie ein anderes Asset (Video oder Bild) aus. |
   | Titel | `Explore. Discover. Live.` |
   | Vortitel | `Join use for your next adventure.` |
   | Beschreibung | Leer lassen |
   | Stil | `Hero` |

   ![Hero-Fragment](./assets/1/teaser-model.png)

## GraphQL-Endpunkte

1. Navigieren Sie zu __Tools > GraphQL__

   ![AEM GraphiQL](./assets/1/endpoint-nav.png)

1. Klicks __Erstellen__ und benennen Sie den neuen Endpunkt und wählen Sie die neu erstellte Konfiguration aus.

   ![AEM Headless-GraphQL-Endpunkt](./assets/1/endpoint.png)

## GraphQL – Persistierte Abfragen

1. Testen wir den neuen Endpunkt. Navigieren Sie zu __Tools > GraphQL Query Editor__ und wählen Sie unseren Endpunkt für die Dropdown-Liste oben rechts im Fenster aus.

1. Erstellen Sie im Abfrageeditor verschiedene Abfragen.


   ```graphql
   {
       teaserList {
           items {
           title
           }
       }
   }
   ```

   Sie sollten eine Liste mit dem einzelnen Fragment erhalten [above](#create-content).

   Erstellen Sie für diese Übung eine vollständige Abfrage, die die AEM Headless-App verwendet. Erstellen Sie eine Abfrage, die einen einzelnen Teaser anhand des Pfads zurückgibt. Geben Sie im Abfrageeditor die folgende Abfrage ein:

   ```graphql
   query TeaserByPath($path: String!) {
   component: teaserByPath(_path: $path) {
       item {
       __typename
       _path
       _metadata {
           stringMetadata {
           name
           value
           }
       }
       title
       preTitle
       style
       asset {
           ... on MultimediaRef {
           __typename
           _authorUrl
           _publishUrl
           format
           }
           ... on ImageRef {
           __typename
           _authorUrl
           _publishUrl
           mimeType
           width
           height
           }
       }
       description {
           html
           plaintext
       }
       }
   }
   }
   ```

   Im __Abfragevariablen__ Eingabe unten:

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > Möglicherweise müssen Sie die Abfragevariable anpassen `path` basierend auf dem Ordner und den Fragmentnamen.


   Führen Sie die Abfrage aus, um die Ergebnisse des zuvor erstellten Inhaltsfragments zu erhalten.

1. Klicks __Speichern__  , um die Abfrage zu speichern und die Abfrage zu benennen __Teaser__. Dadurch können wir die Abfrage anhand des Namens in der Anwendung referenzieren.

## Nächste Schritte

Herzlichen Glückwunsch! Sie haben erfolgreich AEM as a Cloud Service konfiguriert, um die Erstellung von Inhaltsfragmenten und GraphQL-Endpunkten zu ermöglichen. Sie haben auch ein Inhaltsfragmentmodell und ein Inhaltsfragment erstellt, einen GraphQL-Endpunkt und eine persistente Abfrage definiert. Sie können jetzt zum nächsten Tutorial-Kapitel übergehen, in dem Sie erfahren, wie Sie eine AEM Headless-React-Anwendung erstellen, die die in diesem Kapitel erstellten Inhaltsfragmente und GraphQL-Endpunkte verwendet.

[Nächstes Kapitel: AEM Headless-APIs und React](./2-aem-headless-apis-and-react.md)
