---
title: Anpassen der Zusammenfassungskomponente
description: Erweitern Sie die Komponente Zusammenfassungsschritt , um die Funktion zum Navigieren zum nächsten Formular im Paket einzuschließen.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# Übersichtsschritt anpassen

Die Zusammenfassungsschritt-Komponente wird verwendet, um die Zusammenfassung Ihrer Formularübermittlung mit einem Link zum Herunterladen des signierten Formulars anzuzeigen. Der Übersichtsschritt wird normalerweise im letzten Bedienfeld Ihres Formulars platziert.
Für diesen Anwendungsfall haben wir eine neue Komponente erstellt, die auf der vordefinierten Zusammenfassungskomponente basiert, und die Funktion erweitert, um benutzerdefinierte clientlib einzuschließen.

Diese Komponente wird durch die Bezeichnung Sign Multiple Form identifiziert

Der folgende Screenshot zeigt die neue Komponente, die erstellt wurde, um die Nachricht nach Abschluss der Unterzeichnungszeremonie anzuzeigen

![Zusammenfassungskomponente](assets/summary.PNG)

Die neue Komponente basiert auf der vordefinierten Übersichtskomponente .
![component-prop](assets/componentprop.PNG)

Wir haben eine Schaltfläche hinzugefügt, über die Sie zum nächsten Formular zum Signieren navigieren können
![template-code](assets/template-code.PNG)

Die summary.jsp hat den folgenden Code. Sie verweist auf die Client-Bibliothek, die durch die Kategorie-ID identifiziert wird. **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

Die benutzerdefinierte Zusammenfassungskomponente kann [heruntergeladen von hier](assets/custom-summary-step.zip)

## Nächste Schritte

[Nächstes Formular zum Signieren abrufen](./create-client-lib.md)