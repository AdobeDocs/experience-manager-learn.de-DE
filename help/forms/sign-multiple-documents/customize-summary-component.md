---
title: Anpassen der Zusammenfassungskomponente
description: Erweitern Sie die Zusammenfassungsschritt-Komponente, um die Funktion zum Navigieren zum nächsten Formular im Paket einzuschließen.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
duration: 48
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 100%

---

# Anpassen des Zusammenfassungsschritts

Die Zusammenfassungsschritt-Komponente wird verwendet, um eine Zusammenfassung der Formularübermittlung mit einem Link zum Herunterladen des signierten Formulars anzuzeigen. Der Zusammenfassungsschritt wird normalerweise im letzten Bedienfeld Ihres Formulars platziert.
Für diesen Anwendungsfall haben wir eine neue Komponente erstellt, die auf der vorkonfigurierten Zusammenfassungskomponente basiert, und die Funktion erweitert, um eine benutzerdefinierte Client-Bibliothek einzuschließen.

Diese Komponente wird anhand der Bezeichnung „Signieren mehrerer Formulare“ identifiziert.

Der folgende Screenshot zeigt die neue Komponente, die erstellt wurde, um die Nachricht zum Abschluss des Signiervorgangs anzuzeigen.

![Zusammenfassungskomponente](assets/summary.PNG)

Die neue Komponente basiert auf der vorkonfigurierten Zusammenfassungskomponente.
![Komponenteneigenschaften](assets/componentprop.PNG)

Wir haben eine Schaltfläche hinzugefügt, um zum nächsten Formular zum Signieren zu navigieren.
![template-code](assets/template-code.PNG)

Die Datei „summary.jsp“ verfügt über folgenden Code. Sie verweist auf die Client-Bibliothek, die durch die Kategorie-ID **getnextform** identifiziert wird.

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

Die benutzerdefinierte Zusammenfassungskomponente kann [hier](assets/custom-summary-step.zip) heruntergeladen werden.

## Nächste Schritte

[Abrufen des nächsten Formulars zum Signieren](./create-client-lib.md)