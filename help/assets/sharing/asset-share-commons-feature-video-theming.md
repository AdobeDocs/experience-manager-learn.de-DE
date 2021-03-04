---
title: Einführung in das Thema "Asset-Freigabe"-Commons
description: Materialien für das funktionale und technische Verständnis Assets Gemeinsames Komma
version: 6.3, 6.4, 6.5
topic: Content Management
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 4%

---


# Einführung in das Design in Asset-Share-Commons {#asset-share-commons-theme}

Eine kurze Einführung zum Thema in Asset Share Commons. Das Video erläutert die Erstellung eines neuen Designs mit einem benutzerdefinierten Farbschema.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

In diesem Video wird ein neues Design erstellt, das auf dem Design &quot;Asset Sharing Commons Dark&quot;basiert. Das Farbschema wird mit einem benutzerdefinierten Logo übereinstimmen, um der Site ein einheitliches Erscheinungsbild zu verleihen.

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

[Benutzerdefiniertes Client-Bibliotheks-Design](assets/asc-theme-demo.zip) herunterladen

## Zusätzliche Ressourcen{#additional-resources}

* [Versionshinweise zu Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+ - Versionshinweise](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)