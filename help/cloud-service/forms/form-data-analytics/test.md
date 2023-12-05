---
title: Reporting zu übermittelten Formulardatenfeldern mit Adobe Analytics
description: Integrieren von AEM Forms CS mit Adobe Analytics für das Reporting zu Formulardatenfeldern
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 100%

---

# Testen Ihrer Lösung

Zeigen Sie Ihr Formular in einer Vorschau an und übermitteln Sie es mit verschiedenen Kombinationen von Formularwerten. Es dauert bis zu 30 Minuten, bis Ihre Daten in Adobe Analytics-Berichten angezeigt werden. Auf Eigenschaften (Props) eingestellte Datensätze werden in Berichten früher angezeigt als auf eVars eingestellte Datensätze.

## Report Suite

Die in Adobe Analytics erfassten Formulardaten werden als Ringdiagramm angezeigt.

**Übermittlungen nach US-Bundesstaat**

![Antragsstellende nach Wohnsitz](assets/donut.png)

Feldvalidierungsfehler

![Feldvalidierungsfehler](assets/donut-field-validation.png)

## Debugging

Stellen Sie sicher, dass das adaptive Formular denselben Konfigurations-Container verwendet, in dem auch die Adobe Experience Platform Launch-Konfiguration enthalten ist.

Gehen Sie wie folgt vor, um zu bestätigen, dass das Formular Daten an Adobe Analytics sendet:

* Öffnen Sie die Entwickler-Tools in Ihrem Browser.
* Geben Sie im Konsolenbedienfeld den folgenden Text ein.

```javascript
_satellite.setDebug(true)
```

Interagieren Sie mit Ihrem Formular, während Sie das Konsolenfenster geöffnet halten. Dies sollte ungefähr so aussehen:

![Konsolen-Debugging](assets/debug.png)

## Verwenden von Adobe Experience Platform Debugger

Fügen Sie die [AEP Debugger-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html?lang=de) zum Browser hinzu (Sie müssen sich anmelden), um weitere Debugging-Informationen zu erhalten.

![Platform Debugger](assets/platform-debugger.png)

## Herzlichen Glückwunsch!

Sie haben AEM Forms as a Cloud Service erfolgreich in Adobe Analytics integriert, um Berichte zu Formulardatenfeldern zu erstellen.