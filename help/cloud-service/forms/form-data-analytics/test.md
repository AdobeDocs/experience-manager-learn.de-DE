---
title: Berichte zu gesendeten Formulardatenfeldern mit Adobe Analytics
description: Integrieren von AEM Forms CS mit Adobe Analytics für Berichte zu Formulardatenfeldern
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Testen der Lösung

Zeigen Sie eine Vorschau an und senden Sie das Formular mit mehreren Kombinationen von Formularwerten. Es dauert einige bis 30 Minuten, bis Ihre Daten in Adobe Analytics-Berichten angezeigt werden. Auf Props festgelegte Daten werden in Berichten früher angezeigt als Datensätze, die auf eVars gesetzt wurden.

## Report Suite

Die in Adobe Analytics erfassten Formulardaten werden im Ringformat angezeigt

**Übermittlung nach Staat**

![applicantsbystate](assets/donut.png)

Feldvalidierungsfehler

![field-validation-error](assets/donut-field-validation.png)

## Debugging

Stellen Sie sicher, dass das adaptive Formular denselben Konfigurationscontainer verwendet, der die Adobe Launch-Konfiguration enthält.

Gehen Sie wie folgt vor, um zu bestätigen, dass das Formular Daten an Adobe Analytics sendet:

* Öffnen Sie die Entwicklertools in Ihrem Browser.
* Geben Sie im Konsolenbedienfeld den folgenden Text ein.

```javascript
_satellite.setDebug(true)
```

Interagieren Sie mit Ihrem Formular, während Sie das Konsolenfenster geöffnet halten. Sie sollten so etwas sehen

![console-debug](assets/debug.png)

## Adobe Experience Platform Debugger verwenden

Fügen Sie die [AEP-Debugger-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) zum Browser (Sie müssen sich anmelden), um weitere Debugging-Informationen zu erhalten

![platform-debugger](assets/platform-debugger.png)





