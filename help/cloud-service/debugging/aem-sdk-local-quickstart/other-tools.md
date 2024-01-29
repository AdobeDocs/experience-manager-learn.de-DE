---
title: Weitere Tools zum Debugging des AEM SDK
description: Eine Vielzahl anderer Tools kann beim Debugging über den lokalen Schnellstart des AEM SDK hilfreich sein.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
duration: 141
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '514'
ht-degree: 100%

---

# Weitere Tools zum Debugging des AEM SDK

Eine Vielzahl anderer Tools kann beim Debugging Ihrer Anwendung über den lokalen Schnellstart des AEM SDK hilfreich sein.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite ist eine Web-basierte Schnittstelle für die Interaktion mit dem JCR (Java Content Repository), dem Daten-Repository von AEM. CRXDE Lite sorgt für vollständige Sichtbarkeit in Bezug auf das JCR, einschließlich seiner Knoten, Eigenschaften, Eigenschaftswerte und Berechtigungen.

CRXDE Lite ist abrufbar unter:

+ Tools > Allgemein > CRXDE Lite
+ oder direkt über [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Debugging von Inhalten

CRXDE Lite bietet direkten Zugriff auf das JCR. Der über CRXDE Lite sichtbare Inhalt ist durch die Berechtigungen beschränkt, die den jeweiligen Benutzenden erteilt wurden. Abhängig von Ihren Zugriffsrechten können Sie also möglicherweise nicht alle Elemente im JCR anzeigen oder ändern.

+ Die JCR-Struktur ist über den linken Navigationsbereich navigier- und bearbeitbar.
+ Wenn Sie einen Knoten im linken Navigationsbereich auswählen, werden die Knoteneigenschaften im unteren Bereich angezeigt.
   + Die Eigenschaften können über diesen Bereich hinzugefügt, entfernt oder geändert werden.
+ Durch Doppelklicken auf einen Dateiknoten im linken Navigationsbereich wird der Dateiinhalt im oberen rechten Bereich geöffnet.
+ Klicken Sie oben links auf die Schaltfläche „Alle speichern“, um Änderungen beizubehalten, oder auf den Abwärtspfeil neben „Alle speichern“, um nicht gespeicherte Änderungen wiederherzustellen.

![CRXDE Lite – Debugging von Inhalten](./assets/other-tools/crxde-lite__debugging-content.png)

Änderungen, die direkt über CRXDE Lite am AEM SDK vorgenommen werden, sind mitunter schwierig zu verfolgen und zu verwalten. Stellen Sie bei Bedarf sicher, dass über CRXDE Lite vorgenommene Änderungen in den veränderlichen Inhaltspaketen des AEM-Projekts (`ui.content`) widergespiegelt und an Git übergeben werden. Idealerweise stammen alle Änderungen am Anwendungsinhalt aus der Code-Basis, sodass sie über Implementierungen in das AEM SDK einfließen, anstatt das AEM SDK direkt über CRXDE Lite zu ändern.

### Debugging von Zugriffssteuerelementen

CRXDE Lite bietet eine Möglichkeit, die Zugriffssteuerung auf einem bestimmten Knoten für eine bestimmte Benutzerin bzw. einen bestimmten Benutzer oder eine bestimmte Gruppe (auch Prinzipal genannt) zu testen und auszuwerten.

Um in CRXDE Lite auf die Konsole „Zugriffssteuerung testen“ zuzugreifen, navigieren Sie zu:

+ CRXDE Lite > Tools > Zugriffssteuerung testen…

![CRXDE Lite – Zugriffssteuerung testen](./assets/other-tools/crxde-lite__test-access-control.png)

1. Wählen Sie im Feld „Pfad“ einen JCR-Pfad aus, der ausgewertet werden soll.
1. Wählen Sie im Feld „Prinzipal“ die Person oder Gruppe aus, für die der Pfad ausgewertet werden soll.
1. Klicken Sie auf die Schaltfläche „Testen“.

Die Ergebnisse werden unten angezeigt:

+ __Pfad__ wiederholt den ausgewerteten Pfad
+ __Prinzipal__ wiederholt die Person oder Gruppe, für die der Pfad ausgewertet wurde
+ __Prinzipale__ listet alle Prinzipale auf, zu denen der ausgewählte Prinzipal gehört.
   + Dies ist hilfreich, um die transitiven Gruppenmitgliedschaften zu verstehen, die durch Vererbung Berechtigungen ermöglichen können.
+ __Berechtigungen am Pfad__ listet alle JCR-Berechtigungen auf, über die der ausgewählte Prinzipal am ausgewerteten Pfad verfügt.

## Abfrage erläutern

![Abfrage erläutern](./assets/other-tools/explain-query.png)

Das Web-basierte Tool zur Abfrageerläuterung im lokalen Schnellstart des AEM SDK liefert wichtige Erkenntnisse dazu, wie Abfragen durch AEM interpretiert und ausgeführt werden. Es handelt sich um ein wertvolles Tool, das eine leistungsstarke Ausführung von Abfragen durch AEM sicherstellt.

„Abfrage erläutern“ ist abrufbar unter:

+ Tools > Diagnose > Abfrageleistung > Registerkarte „Abfrage erläutern“
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Registerkarte „Abfrage erläutern“

## QueryBuilder-Debugger

![QueryBuilder-Debugger](./assets/other-tools/query-debugger.png)

Der QueryBuilder-Debugger ist ein Web-basiertes Tool, über das Sie Suchanfragen mit der [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=de)-Syntax von AEM debuggen und verstehen können.

Der QueryBuilder-Debugger ist abrufbar unter:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
