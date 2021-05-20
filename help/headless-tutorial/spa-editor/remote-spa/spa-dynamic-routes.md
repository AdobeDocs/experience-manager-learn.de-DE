---
title: Hinzufügen bearbeitbarer Komponenten zu Remote-SPA dynamischen Routen
description: Erfahren Sie, wie Sie in einem Remote-SPA bearbeitbare Komponenten zu dynamischen Routen hinzufügen.
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Kernkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 1%

---


# Dynamische Routen und bearbeitbare Komponenten

In diesem Kapitel aktivieren wir zwei dynamische Abenteuer-Detailrouten, um bearbeitbare Komponenten zu unterstützen. __Bali Surf Camp__ und __Beervana in Portland__.

![Dynamische Routen und bearbeitbare Komponenten](./assets/spa-dynamic-routes/intro.png)

Die Route Abenteuer-Detail SPA ist definiert als `/adventure:path`, wobei `path` der Pfad zum WKND-Abenteuer (Inhaltsfragment) ist, um Details anzuzeigen.

## Ordnen Sie die SPA URLs AEM Seiten zu

In den vorherigen beiden Kapiteln haben wir bearbeitbaren Komponenteninhalt aus der SPA-Startseite der entsprechenden Remote-SPA-Stammseite in AEM `/content/wknd-app/us/en/` zugeordnet.

Die Definition des Mappings für bearbeitbare Komponenten für die SPA dynamischen Routen ist ähnlich. Es muss jedoch ein 1:1-Zuordnungsschema zwischen Instanzen der Route und AEM Seiten erstellt werden.

In diesem Tutorial nehmen wir den Namen des WKND Adventure Content Fragments, das das letzte Segment des Pfads ist, und ordnen ihn einem einfachen Pfad unter `/content/wknd-app/us/en/adventure` zu.

| Remote SPA Route | AEM Seitenpfad |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventures:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

Auf Grundlage dieses Mappings müssen wir also zwei neue AEM Seiten erstellen unter:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Remote SPA

Die Zuordnung von Anforderungen, die die Remote-SPA verlassen, wird über die `setupProxy` -Konfiguration konfiguriert, die in [Bootstrap der SPA](./spa-bootstrap.md) vorgenommen wird.

## SPA Editor-Zuordnung

Die Zuordnung für SPA Anforderungen, wenn die SPA über den SPA Editor geöffnet wird, wird über die Sling-Zuordnungskonfiguration konfiguriert, die unter [Konfigurieren von AEM](./aem-configure.md) durchgeführt wird.

## Inhaltsseiten in AEM erstellen

Erstellen Sie zunächst das Zwischensegment `adventure` Seite :

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND-App > us > en > WKND-App-Homepage__
   + Diese AEM Seite wird als Stamm der SPA zugeordnet. Hier beginnen wir also mit der Erstellung der AEM Seitenstruktur für andere SPA Routen.
1. Tippen Sie auf __Erstellen__ und wählen Sie __Seite__ aus.
1. Wählen Sie die Vorlage __Remote SPA Page__ aus und tippen Sie auf __Weiter__
1. Füllen Sie die Seiteneigenschaften aus.
   + __Titel__: Abenteuer
   + __Name__: `adventure`
      + Dieser Wert definiert die URL der AEM Seite und muss daher mit dem Routensegment der SPA übereinstimmen.
1. Tippen Sie auf __Fertig__

Erstellen Sie dann die AEM Seiten, die den einzelnen SPA URLs entsprechen, die bearbeitbare Bereiche erfordern.

1. Navigieren Sie zur neuen Seite __Adventure__ im Site-Admin.
1. Tippen Sie auf __Erstellen__ und wählen Sie __Seite__ aus.
1. Wählen Sie die Vorlage __Remote SPA Page__ aus und tippen Sie auf __Weiter__
1. Füllen Sie die Seiteneigenschaften aus.
   + __Titel__: Bali Surf Camp
   + __Name__:  `bali-surf-camp`
      + Dieser Wert definiert die URL der AEM Seite und muss daher mit dem letzten Segment der SPA übereinstimmen
1. Tippen Sie auf __Fertig__
1. Wiederholen Sie die Schritte 3-6, um die Seite __Beervana in Portland__ zu erstellen, mit:
   + __Titel__: Beervana in Portland
   + __Name__:  `beervana-in-portland`
      + Dieser Wert definiert die URL der AEM Seite und muss daher mit dem letzten Segment der SPA übereinstimmen

Diese beiden AEM enthalten den jeweiligen erstellten Inhalt für die entsprechenden SPA Routen. Wenn andere SPA Routen Authoring erfordern, müssen neue AEM Seiten unter ihrer SPA URL unter der Stammseite der Remote-SPA-Seite (`/content/wknd-app/us/en/home`) in AEM erstellt werden.

## WKND-App aktualisieren

Platzieren wir die `<AEMResponsiveGrid...>`-Komponente, die im [letzten Kapitel](./spa-container-component.md) erstellt wurde, in unserer `AdventureDetail`-SPA und erstellen Sie einen bearbeitbaren Container.

### Platzieren Sie die AEMResponsiveGrid-SPA

Wenn Sie `<AEMResponsiveGrid...>` in die Komponente `AdventureDetail` platzieren, wird ein bearbeitbarer Container in dieser Route erstellt. Der Trick besteht darin, dass mehrere Routen die Komponente `AdventureDetail` zum Rendern verwenden. Daher müssen wir das Attribut `<AEMResponsiveGrid...>'s pagePath` dynamisch anpassen. Das `pagePath` muss abgeleitet werden, um auf die entsprechende AEM zu verweisen, basierend auf dem Abenteuer, das die Route-Instanz anzeigt.

1. `react-app/src/components/AdventureDetail.js` öffnen und bearbeiten
1. Fügen Sie die folgende Zeile vor `AdventureDetail(..)'s` der zweiten `return(..)` -Anweisung hinzu, die den Abenteuernamen aus dem Inhaltsfragmentpfad ableitet.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. Importieren Sie die Komponente `AEMResponsiveGrid` und platzieren Sie sie über der Komponente `<h2>Itinerary</h2>`.
1. Legen Sie die folgenden Attribute für die Komponente `<AEMResponsiveGrid...>` fest
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Dadurch wird die Komponente `AEMResponsiveGrid` angewiesen, ihren Inhalt aus der AEM Ressource abzurufen:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Aktualisieren Sie `AdventureDetail.js` mit den folgenden Zeilen:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

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

Die Datei `AdventureDetail.js` sollte wie folgt aussehen:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Erstellen Sie den Container in AEM

Wenn das `<AEMResponsiveGrid...>` vorhanden ist und sein `pagePath` dynamisch basierend auf dem abenteuerlichen Abenteuer festgelegt ist, versuchen wir, Inhalte darin zu erstellen.

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND-App > us > en__
1. ____ Bearbeiten der  __WKND-App-__ Startseite
   + Navigieren Sie zur Route __Bali Surf Camp__ im SPA, um sie zu bearbeiten.
1. Wählen Sie __Vorschau__ aus der Modusauswahl oben rechts aus.
1. Tippen Sie auf die Karte __Bali Surf Camp__ im SPA, um zu seiner Route zu navigieren
1. Wählen Sie __Bearbeiten__ aus der Modusauswahl
1. Suchen Sie den bearbeitbaren Bereich __Layout-Container__ direkt über dem __Itinerary__ .
1. Öffnen Sie die Seitenleiste __Seiten-Editor__ und wählen Sie die Ansicht __Komponenten__ aus.
1. Ziehen Sie einige der aktivierten Komponenten in den __Layout-Container__
   + Bild
   + Text
   + Titel

   Erstellen Sie Werbematerial. Es könnte ungefähr so aussehen:

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. ____ Vorschau der Änderungen im AEM Seiteneditor
1. Aktualisieren Sie die WKND-App, die lokal auf [http://localhost:3000](http://localhost:3000) ausgeführt wird, navigieren Sie zur Route __Bali Surf Camp__ , um die erstellten Änderungen anzuzeigen!

   ![Remote SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

Wenn Sie zu einer Abenteuerdetailroute navigieren, der keine zugeordnete AEM Seite zugeordnet ist, gibt es auf dieser Routeninstanz keine Authoring-Möglichkeit. Um das Authoring auf diesen Seiten zu aktivieren, erstellen Sie einfach eine AEM Seite mit dem entsprechenden Namen unter der Seite __Adventure__!

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben die Authoring-Fähigkeit zu dynamischen Routen in der SPA hinzugefügt!

+ Die Komponente &quot;ResponsiveGrid&quot;der AEM React Editable-Komponente wurde zu einer dynamischen Route hinzugefügt.
+ Erstellt AEM Seiten zur Unterstützung der Erstellung von zwei bestimmten Routen im SPA (Bali Surf Camp und Beervana in Portland)
+ Verfasst Inhalt auf der dynamischen Bali Surf Camp Route!

Sie haben nun die ersten Schritte zur Verwendung AEM SPA Editors zum Hinzufügen bestimmter bearbeitbarer Bereiche zu einer Remote-SPA abgeschlossen.


>[!NOTE]
>
>Bleib dran! Dieses Tutorial wird erweitert und umfasst die Best Practices und Empfehlungen der Adobe zur Bereitstellung der SPA-Editor-Lösung in AEM as a Cloud Service und Produktionsumgebungen.