---
title: Verwenden von Transaktionsberichten in AEM Forms
description: Mit Transaktionsberichten in AEM Forms können Sie alle Transaktionen zählen, die seit einem bestimmten Datum in Ihrer AEM Forms-Bereitstellung stattgefunden haben.
feature: Adaptive Formulare
version: 6.4.1,6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 2%

---


# Verwenden von Transaktionsberichten in AEM Forms{#using-transaction-reporting-in-aem-forms}

Mit AEM Forms 6.4.1 wurden Transaktionsberichte eingeführt, um die Anzahl der Formularübermittlungen, die Darstellung von Dokumenten mithilfe von Document Services und die Darstellung interaktiver Kommunikation (Web- und Druckkanäle) zu erfassen. Diese Funktion richtet sich in erster Linie an Kunden, die die Software auf der Grundlage der Anzahl der Formularübermittlungen und/oder wiedergegebenen Dokumente lizenzieren möchten. Diese Funktion ist derzeit nur für AEM Forms OSGI-Stack verfügbar.

## Aktivieren der Transaktionsberichterstellung {#enabling-transaction-reporting}

Standardmäßig ist die Transaktionserfassung deaktiviert. Gehen Sie zur Aktivierung der Transaktionsaufzeichnung wie folgt vor:

* [Öffnen Sie configMgr](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach &quot;Forms Transaction Reporting&quot;.
* Aktivieren Sie das Kontrollkästchen &quot;Transaktionen aufzeichnen&quot;
* Speichern Sie Ihre Änderungen

Sobald die Transaktionsberichterstellung aktiviert ist, können Sie adaptive Forms senden, Dokumente mithilfe von Document Services generieren oder Dokumente zur interaktiven Kommunikation rendern, um Transaktionsberichte in Aktion zu sehen.

## Anzeigen des Transaktionsberichts {#viewing-transaction-report}

Um den Transaktionsbericht anzuzeigen, melden Sie sich bei AEM Forms als Administrator an. Nur Mitglieder der Gruppe fd-Administrator können den Transaktionsbericht anzeigen.

Tools auswählen | Forms | Anzeigen des Transaktionsberichts

oder zeigen Sie den Transaktionsbericht an, indem Sie auf [hier](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html) klicken.

![TransctionReporting](assets/transactionreporting.gif)

Im Screenshot oben ist Document Processed die Anzahl der Dokumente, die mit Document Services generiert wurden. Die gerenderten Dokumente entsprechen der Anzahl der wiedergegebenen interaktiven Kommunikationsdokumente (Web und Druck). Forms Submitted ist die Anzahl der Übermittlungen adaptiver Formulare.

Eine Transaktion verbleibt für einen bestimmten Zeitraum im Puffer (Leerlauf-Pufferzeit + Rückwärtsreplikationszeit). Standardmäßig dauert es ungefähr 90 Sekunden, bis die Transaktionsanzahl im Transaktionsbericht angezeigt wird.

Aktionen wie das Senden eines PDF-Formulars, die Verwendung der Benutzeroberfläche für Agenten zur Vorschau einer interaktiven Kommunikation oder die Verwendung nicht standardmäßiger Methoden zur Formularübermittlung werden nicht als Transaktionen berücksichtigt. AEM Forms bietet eine API zum Aufzeichnen solcher Transaktionen. Rufen Sie die API aus Ihren benutzerdefinierten Implementierungen auf, um eine Transaktion aufzuzeichnen.

Wenn Sie den Transaktionsbericht auf der Autoreninstanz anzeigen, stellen Sie sicher, dass die Rückwärtsreplikation auf allen Veröffentlichungsinstanzen konfiguriert ist.

Weitere Informationen zu Transaktionsberichten finden Sie hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)[

