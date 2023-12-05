---
title: CRXDE Lite
description: CRXDE Lite ist ein klassisches, leistungsstarkes Tool zum Debuggen von AEM as a Cloud Service-Entwicklungsumgebungen. CRXDE Lite bietet eine Reihe von Funktionen, die das Debugging der Überprüfung aller Ressourcen und Eigenschaften, der Bearbeitung der veränderlichen Teile des JCR und der Prüfung von Berechtigungen unterstützen.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 168
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 100%

---

# Debugging von AEM as a Cloud Service mit CRXDE Lite

CRXDE Lite ist __NUR__ verfügbar in AEM as a Cloud Service-Entwicklungsumgebungen (sowie dem lokalen AEM SDK).

## Öffnen von CRXDE Lite in AEM Author

CRXDE Lite ist __nur__ in AEM as a Cloud Service-Entwicklungsumgebungen zugänglich; in Staging- oder Produktionsumgebungen ist es __nicht__ verfügbar.

So greifen Sie in AEM Author auf CRXDE Lite zu:

1. Melden Sie sich beim AEM Author-Service in AEM as a Cloud Service an.
1. Navigieren Sie zu Tools > Allgemein > CRXDE Lite

Dadurch wird CRXDE Lite mit den Anmeldeinformationen und Berechtigungen geöffnet, die zum Anmelden bei AEM Author verwendet werden.

## Debugging von Inhalten

CRXDE Lite bietet direkten Zugriff auf das JCR. Der über CRXDE Lite sichtbare Inhalt ist durch die Berechtigungen beschränkt, die den jeweiligen Benutzenden erteilt wurden. Abhängig von Ihren Zugriffsrechten können Sie also möglicherweise nicht alle Elemente im JCR anzeigen oder ändern.

Beachten Sie, dass `/apps`, `/libs` und `/oak:index` unveränderlich sind, d. h. sie können zur Laufzeit nicht von Benutzenden geändert werden. Diese Speicherorte im JCR können nur über Code-Bereitstellungen geändert werden.

+ Die JCR-Struktur ist über den linken Navigationsbereich navigier- und bearbeitbar.
+ Wenn Sie einen Knoten im linken Navigationsbereich auswählen, werden die Knoteneigenschaften im unteren Bereich angezeigt.
   + Die Eigenschaften können über diesen Bereich hinzugefügt, entfernt oder geändert werden.
+ Durch Doppelklicken auf einen Dateiknoten im linken Navigationsbereich wird der Dateiinhalt im oberen rechten Bereich geöffnet.
+ Klicken Sie oben links auf die Schaltfläche „Alle speichern“, um Änderungen beizubehalten, oder auf den Abwärtspfeil neben „Alle speichern“, um nicht gespeicherte Änderungen wiederherzustellen.

![CRXDE Lite – Debugging von Inhalten](./assets/crxde-lite/debugging-content.png)

Änderungen an veränderlichen Inhalten zur Laufzeit in AEM as a Cloud Service-Entwicklungsumgebungen über CRXDE Lite müssen mit Vorsicht vorgenommen werden.
Änderungen, die direkt über CRXDE Lite in AEM vorgenommen werden, können schwierig zu verfolgen und zu steuern sein. Stellen Sie bei Bedarf sicher, dass über CRXDE Lite vorgenommene Änderungen in den veränderlichen Inhaltspaketen des AEM-Projekts (`ui.content`) widergespiegelt und an Git übergeben werden, damit das Problem gelöst wird. Idealerweise stammen alle Änderungen am Anwendungsinhalt aus der Code-Basis und fließen über Implementierungen in AEM, statt dass direkt Änderungen in AEM über CRXDE Lite vorgenommen werden.

### Debugging von Zugriffssteuerelementen

CRXDE Lite bietet eine Möglichkeit, die Zugriffssteuerung auf einem bestimmten Knoten für eine bestimmte Benutzerin bzw. einen bestimmten Benutzer oder eine bestimmte Gruppe (auch Prinzipal genannt) zu testen und auszuwerten.

Um in CRXDE Lite auf die Konsole „Zugriffssteuerung testen“ zuzugreifen, navigieren Sie zu:

+ CRXDE Lite > Tools > Zugriffssteuerung testen…

![CRXDE Lite – Zugriffssteuerung testen](./assets/crxde-lite/permissions__test-access-control.png)

1. Wählen Sie im Feld „Pfad“ einen JCR-Pfad aus, der ausgewertet werden soll.
1. Wählen Sie im Feld „Prinzipal“ die Person oder Gruppe aus, für die der Pfad ausgewertet werden soll.
1. Klicken Sie auf die Schaltfläche „Testen“.

Die Ergebnisse werden unten angezeigt:

+ __Pfad__ wiederholt den ausgewerteten Pfad
+ __Prinzipal__ wiederholt die Person oder Gruppe, für die der Pfad ausgewertet wurde
+ __Prinzipale__ listet alle Prinzipale auf, zu denen der ausgewählte Prinzipal gehört.
   + Dies ist hilfreich, um die transitiven Gruppenmitgliedschaften zu verstehen, die durch Vererbung Berechtigungen ermöglichen können.
+ __Berechtigungen am Pfad__ listet alle JCR-Berechtigungen auf, über die der ausgewählte Prinzipal am ausgewerteten Pfad verfügt

### Nicht unterstützte Debugging-Aktionen

Im Folgenden werden Debugging-Aktionen beschrieben, die __nicht__ in CRXDE Lite ausgeführt werden können.

### Debuggen von OSGi-Konfigurationen

Bereitgestellte OSGi-Konfigurationen können nicht über CRXDE Lite überprüft werden. OSGi-Konfigurationen werden in AEM-Projekt im `ui.apps`-Code-Paket unter `/apps/example/config.xxx` verwaltet. Bei der Bereitstellung in AEM as a Cloud Service-Umgebungen werden die OSGi-Konfigurationsressourcen jedoch nicht im JCR persistiert und daher nicht über CRXDE Lite sichtbar.

Verwenden Sie stattdessen [Developer Console > Konfigurationen](./developer-console.md#configurations), um bereitgestellte OSGi-Konfigurationen zu überprüfen.
