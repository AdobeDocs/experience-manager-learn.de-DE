---
title: Verwenden vertikaler Registerkarten in AEM Forms as a Cloud Service
description: Erstellen eines adaptiven Formulars mit vertikalen Registerkarten
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '111'
ht-degree: 100%

---

# Navigieren zischen den Registerkarten

Sie können zwischen den Registerkarten navigieren, indem Sie auf die einzelnen Registerkarten klicken oder die Schaltflächen „Zurück“ und „Weiter“ im Formular verwenden.
Damit mithilfe von Schaltflächen navigiert werden kann, fügen Sie dem Formular zwei Schaltflächen hinzu und nennen Sie sie „Zurück“ und „Weiter“. Verknüpfen Sie die folgende benutzerdefinierte Funktion mit dem Klickereignis der Schaltfläche, um zwischen den Registerkarten zu navigieren.

Im Folgenden finden Sie die benutzerdefinierte Funktion zum Navigieren zwischen den Registerkarten.



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

Im Folgenden finden Sie den Regeleditor für die Schaltflächen „Weiter“ und „Zurück“.

**Schaltfläche „Weiter“**

![next-button](assets/next-button.png)

**Schaltfläche „Zurück“**

![prev-button](assets/prev-button.png)
