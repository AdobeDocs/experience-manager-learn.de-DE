---
title: Vertikale Registerkarten in AEM Forms Cloud Service verwenden
description: Erstellen eines adaptiven Formulars mit vertikalen Registerkarten
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
source-git-commit: f3f5c4c4349c8d02c88e1cf91dbf18f58db1e67e
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 5%

---

# Zwischen den Registerkarten navigieren

Sie können zwischen den Registerkarten navigieren, indem Sie auf die einzelnen Registerkarten klicken oder die Schaltflächen &quot;Zurück&quot;und &quot;Weiter&quot;im Formular verwenden.
Um mithilfe von Schaltflächen zu navigieren, fügen Sie dem Formular zwei Schaltflächen hinzu und benennen Sie sie &quot;Zurück&quot;und &quot;Weiter&quot;. Verknüpfen Sie die folgende benutzerdefinierte Funktion mit dem click -Ereignis der Schaltfläche, um zwischen den Registerkarten zu navigieren.

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

Im Folgenden finden Sie den Regeleditor für die Schaltflächen &quot;Weiter&quot;und &quot;Zurück&quot;

**Nächste Schaltfläche**

![next-button](assets/next-button.png)

**Zurück-Schaltfläche**

![prev-button](assets/prev-button.png)

