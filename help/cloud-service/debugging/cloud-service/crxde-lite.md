---
title: CRXDE Lite
description: 'CRXDE Lite ist ein klassisches und dennoch leistungsfähiges Tool zum Debugging von AEM als Cloud Service Developer-Umgebung. CRXDE Lite bietet eine Reihe von Funktionen, die das Debugging von der Überprüfung aller Ressourcen und Eigenschaften, der Manipulation der veränderlichen Teile der JCR und der Überprüfung der Berechtigungen unterstützen. '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# Debuggen von AEM als Cloud Service mit CRXDE Lite

CRXDE Lite ist __NUR__ auf AEM als Cloud Service Development Umgebung (sowie das lokale AEM SDK) verfügbar.

## Zugriff auf CRXDE Lite auf AEM Author

CRXDE Lite ist __nur__ auf AEM als Umgebung zur Entwicklung von Cloud Services verfügbar und ist auf Stage- oder Produktions-Umgebung __nicht__ verfügbar.

So greifen Sie auf die CRXDE Lite auf AEM Author zu:

1. Melden Sie sich beim AEM als Cloud Service-AEM Author-Dienst an.
1. Navigieren Sie zu Tools > Allgemein > CRXDE Lite

Dadurch wird die CRXDE Lite mit den Anmeldeinformationen und Berechtigungen für die Anmeldung bei AEM Author geöffnet.

## Debugging von Inhalten

CRXDE Lite bietet direkten Zugriff auf die JCR. Der Inhalt, der über die CRXDE Lite sichtbar ist, ist durch die Berechtigungen begrenzt, die Ihrem Benutzer erteilt wurden. Das bedeutet, dass Sie möglicherweise nicht alle Inhalte der JCR je nach Zugriff sehen oder ändern können.

Beachten Sie, dass `/apps`und `/libs` `/oak:index` sind unveränderlich, d. h. sie können zur Laufzeit von keinem Benutzer geändert werden. Diese Speicherorte im JCR können nur über Codebereitstellungen geändert werden.

+ Die JCR-Struktur wird mithilfe des linken Navigationsbereichs navigiert und bearbeitet
+ Wenn Sie eine Node im linken Navigationsbereich auswählen, werden die Node-Eigenschaften im unteren Bereich angezeigt.
   + Eigenschaften können im Bereich hinzugefügt, entfernt oder geändert werden
+ Dublette-Klick auf einen Dateiknoten im linken Navigationsbereich öffnet den Dateiinhalt im oberen rechten Fensterbereich
+ Tippen Sie oben links auf die Schaltfläche &quot;Alle speichern&quot;, um die Änderungen beizubehalten, oder auf den Pfeil nach unten neben &quot;Alle speichern&quot;, um nicht gespeicherte Änderungen wiederherzustellen.

![CRXDE Lite - Debugging von Inhalten](./assets/crxde-lite/debugging-content.png)

Änderungen an veränderbaren Inhalten zur Laufzeit in AEM als Cloud Service-Entwicklungs-Umgebung über CRXDE Lite müssen mit Vorsicht vorgenommen werden.
Änderungen, die direkt an AEM über CRXDE Lite vorgenommen werden, können schwer rückverfolgt und gesteuert werden. Vergewissern Sie sich gegebenenfalls, dass über die CRXDE Lite vorgenommene Änderungen zu den mutbaren Inhaltspaketen des AEM-Projekts (`ui.content`) zurückgehen und sich zu Git verpflichten, um sicherzustellen, dass das Problem gelöst wird. Idealerweise stammen alle Änderungen am Anwendungsinhalt aus der Codebasis und fließen über Implementierungen in AEM, anstatt direkt über die CRXDE Lite Änderungen an der AEM vorzunehmen.

### Debugging von Zugriffskontrollen

CRXDE Lite bietet eine Möglichkeit, die Zugriffskontrolle auf einem bestimmten Knoten für einen bestimmten Benutzer oder eine bestimmte Gruppe (auch als Prinzipal bezeichnet) zu testen und auszuwerten.

Um auf die Testkonsole in CRXDE Lite zuzugreifen, navigieren Sie zu:

+ CRXDE Lite > Tools > Zugriffskontrolle testen ...

![CRXDE Lite - Zugriffskontrolle testen](./assets/crxde-lite/permissions__test-access-control.png)

1. Wählen Sie im Feld Pfad einen JCR-Pfad aus, der ausgewertet werden soll
1. Wählen Sie im Feld &quot;Prinzipal&quot;den Benutzer oder die Gruppe aus, anhand dessen der Pfad bewertet werden soll
1. Tippen Sie auf die Schaltfläche Test

Die Ergebnisse werden unten angezeigt:

+ __Pfad__ wiederholt den ausgewerteten Pfad
+ __Prinzipal__ wiederholt den Benutzer oder die Gruppe, für die der Pfad ausgewertet wurde
+ __Prinzipale__ Liste alle Prinzipale, die der ausgewählte Prinzipal ist Teil von.
   + Dies ist hilfreich, um die Übergangsgruppenmitgliedschaften zu verstehen, die Berechtigungen über Vererbung bereitstellen können.
+ __Berechtigungen auf Pfad__ -Listen umfassen alle JCR-Berechtigungen, die der ausgewählte Prinzipal auf dem ausgewerteten Pfad hat

### Nicht unterstützte Debugging-Aktivitäten

Im Folgenden werden Debugging-Aktivitäten beschrieben, die in der CRXDE Lite __nicht__ ausgeführt werden können.

### Debuggen von OSGi-Konfigurationen

Bereitgestellte OSGi-Konfigurationen können nicht über CRXDE Lite überprüft werden. OSGi-Konfigurationen werden im `ui.apps` Codepaket des AEM Projekts beibehalten, `/apps/example/config.xxx`jedoch werden die OSGi-Konfigurationsressourcen bei der Bereitstellung auf AEM als Cloud Service-Umgebung nicht auf der JCR-Datei beibehalten und sind daher nicht über die CRXDE Lite sichtbar.

Verwenden Sie stattdessen [Developer Console > Konfigurationen](./developer-console.md#configurations) , um bereitgestellte OSGi-Konfigurationen zu überprüfen.
