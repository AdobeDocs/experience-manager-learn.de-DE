---
title: CRXDE Lite
description: 'CRXDE Lite ist ein klassisches, aber leistungsstarkes Tool zum Debugging von AEM als Cloud Service-Entwicklungsumgebungen. CRXDE Lite bietet eine Reihe von Funktionen, die das Debugging von der Überprüfung aller Ressourcen und Eigenschaften, der Bearbeitung der veränderlichen Teile des JCR und der Prüfung von Berechtigungen unterstützen. '
feature: Entwickler-Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# Debugging von AEM als Cloud Service mit CRXDE Lite

Die CRXDE Lite ist __NUR__ verfügbar in AEM as a Cloud Service Development-Umgebungen (sowie im lokalen AEM SDK).

## Zugriff auf die CRXDE Lite in der AEM-Autoreninstanz

Die CRXDE Lite ist __nur__, auf AEM als Cloud Service-Entwicklungsumgebung zugegriffen werden kann, und ist __nicht__ in Staging- oder Produktionsumgebungen verfügbar.

So greifen Sie auf die CRXDE Lite in der AEM-Autoreninstanz zu:

1. Melden Sie sich beim AEM als Cloud Service beim AEM-Autorendienst an.
1. Navigieren Sie zu Tools > Allgemein > CRXDE Lite .

Dadurch wird die CRXDE Lite mit den Anmeldeinformationen und Berechtigungen geöffnet, die zum Anmelden bei der AEM-Autoreninstanz verwendet werden.

## Debugging von Inhalten

CRXDE Lite bietet direkten Zugriff auf das JCR. Der in der CRXDE Lite sichtbare Inhalt ist durch die Berechtigungen beschränkt, die Ihrem Benutzer erteilt wurden. Das bedeutet, dass Sie je nach Zugriff möglicherweise nicht alle Elemente im JCR anzeigen oder ändern können.

Beachten Sie, dass `/apps`, `/libs` und `/oak:index` unveränderlich sind, d. h. sie können zur Laufzeit von keinem Benutzer geändert werden. Diese Speicherorte im JCR können nur über Code-Bereitstellungen geändert werden.

+ Die JCR-Struktur wird mithilfe des linken Navigationsbereichs navigiert und bearbeitet
+ Wenn Sie einen Knoten im linken Navigationsbereich auswählen, wird der Knoten der Knoteneigenschaft im unteren Bereich angezeigt.
   + Eigenschaften können aus dem Bereich hinzugefügt, entfernt oder geändert werden
+ Durch Doppelklicken auf einen Dateiknoten im linken Navigationsbereich wird der Inhalt der Datei im oberen rechten Bereich geöffnet
+ Tippen Sie oben links auf die Schaltfläche Alle speichern , um Änderungen beizubehalten, oder auf den Abwärtspfeil neben Alle speichern , um nicht gespeicherte Änderungen wiederherzustellen.

![CRXDE Lite - Debugging von Inhalten](./assets/crxde-lite/debugging-content.png)

Änderungen an veränderlichen Inhalten zur Laufzeit in AEM als Cloud Service-Entwicklungsumgebungen über CRXDE Lite müssen mit Vorsicht vorgenommen werden.
Änderungen, die direkt über die CRXDE Lite an AEM vorgenommen werden, können schwierig zu verfolgen und zu steuern sein. Stellen Sie bei Bedarf sicher, dass über CRXDE Lite vorgenommene Änderungen zu den veränderlichen Inhaltspaketen des AEM-Projekts (`ui.content`) zurückkehren und an Git gebunden sind, um sicherzustellen, dass das Problem behoben wird. Idealerweise stammen alle Änderungen am Anwendungsinhalt aus der Codebasis und fließen über Implementierungen in AEM, anstatt direkt Änderungen an der AEM über die CRXDE Lite vorzunehmen.

### Debuggen von Zugriffssteuerelementen

CRXDE Lite bietet eine Möglichkeit, die Zugriffskontrolle auf einem bestimmten Knoten für einen bestimmten Benutzer oder eine bestimmte Gruppe (auch Prinzipal genannt) zu testen und auszuwerten.

Um in CRXDE Lite auf die Konsole &quot;Zugriffskontrolle testen&quot;zuzugreifen, navigieren Sie zu:

+ CRXDE Lite > Tools > Zugriffskontrolle testen ...

![CRXDE Lite - Zugriffskontrolle testen](./assets/crxde-lite/permissions__test-access-control.png)

1. Wählen Sie im Feld Pfad einen JCR-Pfad aus, der ausgewertet werden soll
1. Wählen Sie im Feld Prinzipal den Benutzer oder die Gruppe aus, anhand dessen der Pfad bewertet werden soll
1. Tippen Sie auf die Schaltfläche Test .

Die Ergebnisse werden unten angezeigt:

+ ____ Pathreiteriert den ausgewerteten Pfad
+ ____ Prinzipal wiederholt den Benutzer oder die Gruppe, für den/die der Pfad ausgewertet wurde.
+ ____ Prinzipal enthält alle Prinzipale, zu denen der ausgewählte Prinzipal gehört.
   + Dies ist hilfreich, um die transitiven Gruppenmitgliedschaften zu verstehen, die Berechtigungen durch Vererbung bereitstellen können.
+ __Die Berechtigungen unter__ Pathlistet alle JCR-Berechtigungen auf, die der ausgewählte Prinzipal im ausgewerteten Pfad hat

### Nicht unterstützte Debugging-Aktivitäten

Im Folgenden werden Debugging-Aktivitäten beschrieben, die in CRXDE Lite __nicht__ ausgeführt werden können.

### Debuggen von OSGi-Konfigurationen

Bereitgestellte OSGi-Konfigurationen können nicht über CRXDE Lite überprüft werden. OSGi-Konfigurationen werden im `ui.apps` -Codepaket des AEM-Projekts unter `/apps/example/config.xxx` verwaltet. Bei der Bereitstellung in AEM als Cloud Service-Umgebungen werden die OSGi-Konfigurationsressourcen jedoch nicht im JCR persistiert und daher nicht über die CRXDE Lite sichtbar.

Verwenden Sie stattdessen [Entwicklerkonsole > Konfigurationen](./developer-console.md#configurations) , um bereitgestellte OSGi-Konfigurationen zu überprüfen.
