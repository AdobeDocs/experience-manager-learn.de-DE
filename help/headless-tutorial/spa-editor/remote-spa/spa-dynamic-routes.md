---
title: Hinzufügen bearbeitbarer Komponenten zu Remote-SPA dynamischen Routen
description: Erfahren Sie, wie Sie in einem Remote-SPA bearbeitbare Komponenten zu dynamischen Routen hinzufügen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 1%

---

# Dynamische Routen und bearbeitbare Komponenten

In diesem Kapitel aktivieren wir zwei dynamische Abenteuer-Detailrouten, um bearbeitbare Komponenten zu unterstützen. __Bali Surf Camp__ und __Beervana in Portland__.

![Dynamische Routen und bearbeitbare Komponenten](./assets/spa-dynamic-routes/intro.png)

Die SPA Route Abenteuer Detail ist definiert als `/adventure:path` where `path` ist der Pfad zum WKND-Abenteuer (Inhaltsfragment), zu dem Details angezeigt werden.

## Ordnen Sie die SPA URLs AEM Seiten zu

In den vorherigen beiden Kapiteln haben wir bearbeitbaren Komponenteninhalt aus der SPA-Startansicht der entsprechenden Remote-SPA-Stammseite in AEM unter `/content/wknd-app/us/en/`.

Die Definition des Mappings für bearbeitbare Komponenten für die SPA dynamischen Routen ist ähnlich. Es muss jedoch ein 1:1-Zuordnungsschema zwischen Instanzen der Route und AEM Seiten erstellt werden.

In diesem Tutorial nehmen wir den Namen des WKND Adventure Content Fragments, das das letzte Segment des Pfads ist, und ordnen ihn einem einfachen Pfad unter `/content/wknd-app/us/en/adventure`.

| Remote SPA Route | AEM Seitenpfad |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventures:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventures/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

Auf Grundlage dieses Mappings müssen wir also zwei neue AEM Seiten erstellen unter:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Remote SPA

Das Mapping für Anfragen, die aus der Remote-SPA ausscheiden, wird über das `setupProxy` -Konfiguration durchgeführt in [Bootstrap des SPA](./spa-bootstrap.md).

## SPA Editor-Zuordnung

Die Zuordnung für SPA Anforderungen, wenn die SPA über den SPA Editor geöffnet wird, wird über die Sling-Zuordnungskonfiguration konfiguriert, die in [AEM konfigurieren](./aem-configure.md).

## Inhaltsseiten in AEM erstellen

Erstellen Sie zunächst den Vermittler. `adventure` Seitensegment:

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND-App > us > en > WKND-App-Homepage__
   + Diese AEM Seite wird als Stamm der SPA zugeordnet. Hier beginnen wir also mit der Erstellung der AEM Seitenstruktur für andere SPA Routen.
1. Tippen __Erstellen__ und wählen Sie __Seite__
1. Wählen Sie die __Remote-SPA__ und tippen Sie auf __Nächste__
1. Füllen Sie die Seiteneigenschaften aus.
   + __Titel__: Abenteuer
   + __Name__: `adventure`
      + Dieser Wert definiert die URL der AEM Seite und muss daher mit dem Routensegment der SPA übereinstimmen.
1. Tippen Sie auf __Fertig__

Erstellen Sie dann die AEM Seiten, die den einzelnen SPA URLs entsprechen, die bearbeitbare Bereiche erfordern.

1. Navigieren Sie zum neuen __Abenteuer__ Seite in der Site-Admin
1. Tippen __Erstellen__ und wählen Sie __Seite__
1. Wählen Sie die __Remote-SPA__ und tippen Sie auf __Nächste__
1. Füllen Sie die Seiteneigenschaften aus.
   + __Titel__: Bali Surf Camp
   + __Name__: `bali-surf-camp`
      + Dieser Wert definiert die URL der AEM Seite und muss daher mit dem letzten Segment der SPA übereinstimmen
1. Tippen Sie auf __Fertig__
1. Wiederholen Sie die Schritte 3 bis 6, um die __Beervana in Portland__ Seite mit:
   + __Titel__: Beervana in Portland
   + __Name__: `beervana-in-portland`
      + Dieser Wert definiert die URL der AEM Seite und muss daher mit dem letzten Segment der SPA übereinstimmen

Diese beiden AEM enthalten den jeweils erstellten Inhalt für die entsprechenden SPA Routen. Wenn andere SPA Routen Authoring erfordern, müssen neue AEM Seiten unter ihrer SPA-URL unter der Stammseite der Remote-SPA-Seite (`/content/wknd-app/us/en/home`) in AEM.

## WKND-App aktualisieren

Platzieren wir die `<AEMResponsiveGrid...>` Komponente, die in der [letztes Kapitel](./spa-container-component.md)in `AdventureDetail` SPA Komponente, Erstellen eines bearbeitbaren Containers.

### Platzieren Sie die AEMResponsiveGrid-SPA

Platzieren der `<AEMResponsiveGrid...>` im `AdventureDetail` -Komponente erstellt einen bearbeitbaren Container in dieser Route. Der Trick ist, dass mehrere Routen die `AdventureDetail` Komponente, die gerendert werden soll, müssen wir die  `<AEMResponsiveGrid...>'s pagePath` -Attribut. Die `pagePath` müssen so abgeleitet werden, dass sie auf die entsprechende AEM Seite verweisen, je nachdem, welches Abenteuer die Route-Instanz anzeigt.

1. Öffnen und Bearbeiten `react-app/src/components/AdventureDetail.js`
1. Fügen Sie die folgende Zeile vor `AdventureDetail(..)'s` second `return(..)` -Anweisung, die den Abenteuernamen vom Inhaltsfragmentpfad ableitet.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = _path.split('/').pop();
   ...
   ```

1. Importieren Sie die `AEMResponsiveGrid` -Komponente und platzieren Sie sie über dem `<h2>Itinerary</h2>` -Komponente.
1. Legen Sie die folgenden Attribute für die `<AEMResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Dies weist die `AEMResponsiveGrid` -Komponente, um ihren Inhalt aus der AEM-Ressource abzurufen:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Aktualisieren `AdventureDetail.js` mit den folgenden Zeilen:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = _path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

Die `AdventureDetail.js` sollte wie folgt aussehen:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Erstellen Sie den Container in AEM

Mit dem `<AEMResponsiveGrid...>` und `pagePath` dynamisch festgelegt, basierend auf dem gerenderten Abenteuer, versuchen wir, Inhalte darin zu erstellen.

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND App > us > en__
1. __Bearbeiten__ die __WKND-App-Startseite__ page
   + Navigieren Sie zum __Bali Surf Camp__ Route in der SPA, um sie zu bearbeiten
1. Auswählen __Vorschau__ über die Modusauswahl oben rechts
1. Tippen Sie auf die __Bali Surf Camp__ -Karte im SPA, um zu seiner Route zu navigieren
1. Auswählen __Bearbeiten__ über die Modusauswahl
1. Suchen Sie die __Layout-Container__ bearbeitbarer Bereich direkt über dem __Route__
1. Öffnen Sie die __Seitenleiste des Seiteneditors__ und wählen Sie die __Komponentenansicht__
1. Ziehen Sie einige der aktivierten Komponenten in den __Layout-Container__
   + Bild
   + Text
   + Titel

   Erstellen Sie Werbematerial. Es könnte ungefähr so aussehen:

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Vorschau__ Ihre Änderungen in AEM Seiteneditor
1. Aktualisieren Sie die WKND-App, die lokal ausgeführt wird auf [http://localhost:3000](http://localhost:3000), navigieren Sie zum __Bali Surf Camp__ Route, um die erstellten Änderungen zu sehen!

   ![Remote SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

Wenn Sie zu einer Abenteuerdetailroute navigieren, der keine zugeordnete AEM Seite zugeordnet ist, gibt es auf dieser Routeninstanz keine Authoring-Möglichkeit. Um das Authoring auf diesen Seiten zu aktivieren, erstellen Sie einfach eine AEM Seite mit dem entsprechenden Namen unter __Abenteuer__ Seite!

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben die Authoring-Fähigkeit zu dynamischen Routen in der SPA hinzugefügt!

+ Die Komponente &quot;ResponsiveGrid&quot;der AEM React Editable-Komponente wurde zu einer dynamischen Route hinzugefügt.
+ Erstellt AEM Seiten zur Unterstützung der Erstellung von zwei bestimmten Routen im SPA (Bali Surf Camp und Beervana in Portland)
+ Verfasst Inhalt auf der dynamischen Bali Surf Camp Route!

Sie haben nun die ersten Schritte zur Verwendung AEM SPA Editors zum Hinzufügen bestimmter bearbeitbarer Bereiche zu einer Remote-SPA abgeschlossen!
