---
title: Zusammenfassungskomponente anpassen
description: Erweitern Sie die Komponente für den Übersichtsschritt, um die Funktion zum Navigieren zum nächsten Formular im Paket einzuschließen.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Übersichtsschritt anpassen

Die Komponente Zusammenfassungsschritt wird verwendet, um die Zusammenfassung Ihrer Formularübermittlung mit einem Link zum Herunterladen des signierten Formulars anzuzeigen. Der Übersichtsschritt wird normalerweise im letzten Bereich des Formulars platziert.
Für diesen Verwendungsfall haben wir eine neue Komponente erstellt, die auf der gebrauchsfertigen Zusammenfassungskomponente basiert, und die Funktion erweitert, um benutzerdefinierte clientlib einzubeziehen.

Diese Komponente wird durch die Beschriftung &quot;Mehrere Formulare signieren&quot;gekennzeichnet

Der folgende Screenshot zeigt die neue Komponente, die erstellt wurde, um die Meldung nach Abschluss der Unterzeichnungszeremonie anzuzeigen

![Zusammenfassungskomponente](assets/summary.PNG)

Die neue Komponente basiert auf der sofort einsetzbaren Übersichtskomponente.
![component-prop](assets/componentprop.PNG)

Wir haben eine Schaltfläche hinzugefügt, um zum nächsten Formular zum Unterschreiben zu navigieren
![template-code](assets/template-code.PNG)

Die Datei &quot;summary.jsp&quot;hat den folgenden Code. Es enthält einen Verweis auf die Client-Bibliothek, die durch die Kategorien-ID **getnextform** identifiziert wird.

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

Die Komponente für die benutzerspezifische Zusammenfassung kann [von hier heruntergeladen werden](assets/custom-summary-step.zip)


