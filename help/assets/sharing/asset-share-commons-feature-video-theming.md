---
title: Einführung in das Designing in Asset Share Commons
description: Materialien für das funktionale und technische Verständnis von Assets Share Commons
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 3%

---


# Einführung in das Designing in Asset Share Commons {#asset-share-commons-theme}

Eine kurze Einführung in das Thema in Asset Share Commons. Das Video führt durch den Prozess der Erstellung eines neuen Designs mit einem benutzerdefinierten Farbschema.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

In diesem Video wird ein neues Design basierend auf dem Asset Share Commons Dark-Design erstellt. Das Farbschema wird mit einem benutzerdefinierten Logo übereinstimmen, um der Site ein einheitliches Erscheinungsbild zu verleihen.

## Designvariablen

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

[Benutzerdefiniertes Client-Bibliotheksthema](assets/asc-theme-demo.zip) herunterladen

## Zusätzliche Ressourcen{#additional-resources}

* [Asset Share Commons-Veröffentlichungsdownloads](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+-Versionsdownloads](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)