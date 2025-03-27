---
title: Verwenden des Stilsystems in AEM Forms
description: Definieren der CSS-Klassen für die Varianten
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 68030c25-1674-4a64-9f5f-c1a74a38e3ca
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '245'
ht-degree: 100%

---

# Erstellen von Varianten für die Schaltflächenkomponente

Nachdem Sie das Design geklont haben, öffnen Sie das Projekt mit Visual Studio Code. Sie sollten im
![Projekt-Explorer](assets/easel-theme.png)
in Visual Studio Code eine ähnliche Ansicht sehen

Öffnen Sie „src->Komponenten->Schaltfläche->_button.scss-Datei“. Wir werden unsere benutzerdefinierten Varianten in dieser Datei definieren.

## Variante für Unternehmen

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

* **cmp-adaptiveform-button—corporate**: Dies ist die Haupt-Wrapper- oder Container-Klasse für die Komponente „cmp-adaptiveform-button—corporate“.
Alle Stile oder Mixins innerhalb dieses Blocks gelten für Elemente innerhalb dieser Klasse.
* **@include container**: Verwendet ein Mixin namens „container“, das in „_mixins.scss“ definiert ist. Der Mixin-Container wendet normalerweise Layout-bezogene Stile wie das Einrichten von Rändern, Abständen oder andere Strukturstile an, um sicherzustellen, dass sich der Container konsistent verhält.
* **.cmp-adaptiveform-button**: Innerhalb des Blocks „corporate-style-button“ zielen Sie auf das untergeordnete Element mit der Klasse „.cmp-adaptiveform-button“ ab.
* **&amp;__widget**: Das Symbol „&amp;“ bezieht sich auf den übergeordneten Selektor, der in diesem Fall „.cmp-adaptiveform-button“ lautet.
Das bedeutet, dass die Klasse, auf die endgültig abgezielt wird, „.cmp-adaptiveform-button__widget“ ist, eine Klasse im BEM-Stil (Block Element Modifier), die eine Unterkomponente (das Element „__widget “) innerhalb des Blocks „.cmp-adaptiveform-button“ darstellt.
* **@include primary-button**: Dies umfasst ein Mixin „primary-button“, das in „_mixin.scss definiert“ ist, und fügt Stile für die Schaltfläche hinzu (wie Abstand, Farben und Hover-Effekte). Die Eigenschaften „background“, „text-transform“, „border-radius“ und „color“, die im Mixin „primary-button“ definiert sind, werden überschrieben.

Die Datei „_mixins.scss“ wird unter „src->Standort“ definiert, wie im Screenshot unten dargestellt.

![mixin.scss](assets/mixins.png)

## Variante für Marketing

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

[Testen der Varianten](./build.md)
