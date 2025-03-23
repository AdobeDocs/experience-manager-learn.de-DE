---
title: Hinzufügen benutzerdefinierter Symbole
description: Hinzufügen benutzerdefinierter Symbole zu vertikalen Registerkarten
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16418
exl-id: 20e44be0-5490-4414-9183-bb2d2a80bdf0
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 100%

---

# Hinzufügen benutzerdefinierter Symbole

Das Hinzufügen benutzerdefinierter Symbole zu Registerkarten kann das Anwendererlebnis und die visuelle Darstellung auf verschiedene Arten verbessern:

* Verbesserte Benutzerfreundlichkeit: Symbole vermitteln den Zweck jeder Registerkarte schnell, sodass Benutzende leichter finden können, wonach sie suchen. Visuelle Hinweise wie Symbole helfen Benutzenden, intuitiver zu navigieren.

* Visuelle Hierarchie und Fokus: Symbole ermöglichen eine deutlichere Trennung zwischen Registerkarten, wodurch die visuelle Hierarchie verbessert wird. Dies kann dazu beitragen, wichtige Registerkarten hervorzuheben und die Aufmerksamkeit der Benutzenden effektiv zu lenken.
Indem Sie diesen Artikel befolgen, sollten Sie die Symbole wie unten gezeigt platzieren können

![Symbole](assets/icons.png)

## Voraussetzungen

Um diesen Artikel befolgen zu können, müssen Sie mit Git, dem Erstellen und Bereitstellen eines AEM-Projekts mit Cloud Manager, dem Einrichten einer Frontend-Pipeline in AEM Cloud Manager und auch ein wenig mit CSS vertraut sein. Wenn Sie mit den oben genannten Themen nicht vertraut sind, befolgen Sie den Artikel [Verwenden von Designs zum Formatieren von Kernkomponenten](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/using-themes-in-core-components#rename-env-file-theme-folder).

## Hinzufügen von Symbolen zum Design

Öffnen Sie das Design-Projekt in Visual Studio Code oder einem anderen Editor Ihrer Wahl.
Fügen Sie die von Ihnen ausgewählten Symbole zum Bilderordner hinzu.
Die rot markierten Symbole sind die neuen hinzugefügten Symbole.
![new-icons](assets/newicons.png)

## Erstellen einer Symbolzuordnung zum Speichern der Symbole

Erstellen Sie die Symbolzuordnung in der Datei „_variable.scss“. Die SCSS-Zuordnung $icon-map ist eine Sammlung von Schlüssel-Wert-Paaren, bei denen jeder Schlüssel einen Symbolnamen darstellt (z. B. Startseite, Familie usw.) und jeder Wert der Pfad zur Grafikdatei ist, die mit diesem Symbol verknüpft ist.

![variable-scss](assets/variable_scss.png)

```css
$icon-map: (
    home: "./resources/images/home.png",
    family: "./resources/images/icons8-family-80.png",
    pdf: "./resources/images/pdf.png",
    income: "./resources/images/income.png",
    assets: "./resources/images/assets.png",
    cars: "./resources/images/cars.png"
);
```

## Mixin hinzufügen

Fügen Sie den folgenden Code zu _mixin.scss hinzu.

```css
@mixin add-icon-to-vertical-tab($image-url) {
  display: inline-flex;
  align-self: center;
  &::before {
    content: "";
    display:inline-block;
    background: url($image-url) left center / cover no-repeat;
    margin-right: 8px; /* Space between icon and text */
    height:40px;
    width:40px;
    vertical-align:middle;
    
  }
  
}
```

Das Mixin „add-icon-to-vertical-tab“ fügt ein benutzerdefiniertes Symbol neben dem Text auf einer vertikalen Registerkarte hinzu. Damit können Sie ein Bild einfach als Symbol auf Registerkarten einfügen, es neben dem Text positionieren und so gestalten, dass Konsistenz und Ausrichtung gewährleistet werden.

Aufschlüsselung des Mixins. Die einzelnen Teile des Mixins funktionieren wie folgt:

Parameter:

* $image-url: Die URL des Symbols oder Bildes, das neben dem Registerkartentext angezeigt werden soll. Wenn Sie diesen Parameter übergeben, ist das Mixin vielseitig, da es bei Bedarf verschiedene Symbole zu verschiedenen Registerkarten hinzufügen kann.

* Angewendete Stile:

   * display: inline-flex: Dadurch wird das Element zu einem Flex-Container, der verschachtelte Inhalte (wie etwa das Symbol und den Text) horizontal ausrichtet.
   * align-self: center: Stellt sicher, dass das Element innerhalb seines Containers vertikal zentriert ist.
   * Pseudo-Element (::before):
   * content: &quot;&quot;: Initialisiert das Pseudo-Element ::before, mit dem das Symbol als Hintergrundbild angezeigt wird.
   * display: inline-block: Legt für das Pseudo-Element einen Inline-Block fest, sodass es sich wie ein Symbol verhalten kann, das inline mit dem Text platziert wird.
   * background: url($image-url) left center / cover no-repeat;: Fügt das Hintergrundbild mithilfe der URL hinzu, die über $image-url bereitgestellt wird. Das Symbol wird links ausgerichtet und vertikal zentriert.

## Aktualisieren von _verticaltabs.scss

Für die Zwecke des Artikels wurde eine neue CSS-Klasse (cmp-verticaltabs--marketing) erstellt, um die Registerkartensymbole anzuzeigen. In dieser neuen Klasse wird das Registerkartenelement durch Hinzufügen der Symbole erweitert. Die vollständige Auflistung der CSS-Klasse lautet wie folgt:

```css
.cmp-verticaltabs--marketing
{
  .cmp-verticaltabs
    {
      &__tab 
        {
          cursor:pointer;
            @each $name, $url in $icon-map {
            &[data-icon-name="#{$name}"]
              {
                  @include add-icon-to-vertical-tab($url);
              }
            }
        }
    }
}
```

## Bearbeiten der Komponente „verticaltabs“

Kopieren Sie die Datei „verticaltabs.html“ aus ```/apps/core/fd/components/form/verticaltabs/v1/verticaltabs/verticaltabs.html``` und fügen Sie sie unter der Komponente „verticaltabs“ Ihres Projekts ein. Fügen Sie die folgende Zeile ```data-icon-name="${tab.name}"``` zur kopierten Datei unter der Rolle „li“ hinzu, wie in der nachfolgenden Abbildung dargestellt
![data-icon](assets/data-icons.png)
Legen Sie ein benutzerdefiniertes Datenattribut namens „data-icon-name“ mit dem Wert des Registerkartennamens fest. Wenn der Registerkartenname mit einem Bildnamen in der Symbolzuordnung übereinstimmt, wird das entsprechende Bild mit der Registerkarte verknüpft.



## Testen des Codes

Stellen Sie die aktualisierte Komponente „verticaltabs“ in Ihrer Cloud-Instanz bereit.
Stellen Sie das aktualisierte Design mithilfe der Frontend-Pipeline bereit.
Erstellen Sie eine Stilvariante für die Komponenten der vertikalen Registerkarte, wie unten dargestellt:
![style-variation](assets/verticaltab-style-variation.png)
Es wurde eine Stilvariante namens „Marketing“ erstellt, die mit der CSS-Klasse _**cmp-verticaltabs--marketing**_ verknüpft ist.
Erstellen Sie ein adaptives Formular mit einer Komponente einer vertikalen Registerkarte. Verknüpfen Sie die Komponente der vertikalen Registerkarte mit der Stilvariante „Marketing“.
Fügen Sie den verticaltabs einige Registerkarten hinzu und benennen Sie sie so, dass sie den in der Symbolzuordnung definierten Bildern entsprechen, z. B. Startseite, Familie.
![home-icon](assets/tab-name.png)

Zeigen Sie die Vorschau des Formulars an, in der die entsprechenden Symbole für die Registerkarte dargestellt werden sollten.
