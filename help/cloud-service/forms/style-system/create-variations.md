---
title: Verwenden des Stilsystems in AEM Forms
description: Definieren Sie die CSS-Klassen für die Varianten
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 3%

---

# Erstellen von Varianten für die Schaltflächenkomponente

Nachdem das Design geklont wurde, öffnen Sie das Projekt mit visuellem Studio-Code. Sie sollten eine ähnliche Ansicht sehen
im visuellen Studio-Code
![Projekt-Explorer](assets/easel-theme.png)

Öffnen Sie die Datei src->components->button->_button.scss . Wir werden unsere benutzerdefinierten Varianten in dieser Datei definieren.

## Variation von Unternehmen

```css
.cmp-adaptiveform-button-corporate {
  @include container;
  .cmp-adaptiveform-button {
    &__widget {
      @include primary-button;
      background: $brand-red;
      text-transform: uppercase;
      border-radius: 0px;
      color: yellow;
    }
  }
}
```

## Erklärung

* **cmp-adaptiveform-button—corporate**: Dies ist die Haupt-Wrapper- oder Container-Klasse für die Komponente &quot;cmp-adaptiveform-button—corporate&quot;.
Alle Stile oder Mixins innerhalb dieses Blocks gelten für Elemente innerhalb dieser Klasse.
* **@include container**: Verwendet ein Mixin namens &quot;container&quot;, das in _mixins.scss definiert ist. Der Mixin-Container wendet normalerweise Layout-bezogene Stile wie das Einrichten von Rändern, Abständen oder andere Strukturstile an, um sicherzustellen, dass sich der Container konsistent verhält.
* **.cmp-adaptiveform-button**: Innerhalb des Schaltflächenblocks &quot;Corporate-style-button&quot;zielen Sie auf das untergeordnete Element mit der Klasse .cmp-adaptiveform-button ab.
* **&amp;__widget**: Das Symbol &amp; bezieht sich auf den übergeordneten Selektor, der in diesem Fall .cmp-adaptiveform-button lautet.
Das bedeutet, dass die endgültige Zielklasse .cmp-adaptiveform-button__widget ist, eine BEM-Style-Klasse (Block Element Modifier), die eine Unterkomponente (das Element __widget ) innerhalb des Bausteins .cmp-adaptiveform-button darstellt.
* **@include primary-button**: Dies umfasst ein Primärschaltflächen-Mixin, das in _mixin.scss definiert ist, und fügt Stile für die Schaltfläche hinzu (wie Abstand, Farben, Hover-Effekte usw.). Die Eigenschaften background,text-transform,border-radius,color, die in der primären Schaltfläche des Mixins definiert sind, werden überschrieben.

Die Datei _mixins.scss wird unter src->site definiert, wie im Screenshot unten dargestellt

![mixin.scss](assets/mixins.png)

## Marketing-Variante

```css
.cmp-adaptiveform-button--marketing {
  
  @include container;
  .cmp-adaptiveform-button {
  &__widget {
    @include primary-button;
    background-color: #3498db;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 50px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: all 0.3s ease;
    outline: none;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    &:hover:not([disabled]) {
      position: relative;
      scale: 102%;
      transition: box-shadow 0.1s ease-out, transform 0.1s ease-out;
      background-color: #2980b9;
      box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
      transform: translateY(-3px);
    }
  }
}
  
}
```

## Nächste Schritte

[Varianten testen](./build.md)


