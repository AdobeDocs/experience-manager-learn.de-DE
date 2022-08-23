---
title: Verwenden von Transaktionsberichten in AEM Forms
description: Mit Transaktionsberichten in AEM Forms können Sie alle Transaktionen zählen, die seit einem bestimmten Datum in Ihrer AEM Forms-Bereitstellung stattgefunden haben.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 29%

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

oder Sie können den Transaktionsbericht anzeigen, indem Sie auf [here](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransctionReporting](assets/transactionreporting.gif)

Im Screenshot oben ist Document Processed die Anzahl der Dokumente, die mit Document Services generiert wurden. Die gerenderten Dokumente entsprechen der Anzahl der wiedergegebenen interaktiven Kommunikationsdokumente (Web und Druck). Forms Submitted ist die Anzahl der Übermittlungen adaptiver Formulare.

Eine Transaktion verbleibt für einen bestimmten Zeitraum im Puffer (Leerlauf-Pufferzeit + Rückwärtsreplikationszeit). Standardmäßig dauert es ungefähr 90 Sekunden, bis die Anzahl der Transaktionen im Transaktionsbericht angezeigt wird.

Aktionen wie das Senden eines PDF-Formulars, die Verwendung der Agent-Benutzeroberfläche zur Vorschau einer interaktiven Kommunikation oder die Verwendung nicht standardmäßiger Methoden zur Formularübermittlung werden nicht als Transaktionen gezählt. AEM Forms bietet eine API zum Aufzeichnen solcher Transaktionen. Rufen Sie die API aus Ihren benutzerdefinierten Implementierungen auf, um eine Transaktion aufzuzeichnen.

Wenn Sie den Transaktionsbericht auf der Autoreninstanz anzeigen, stellen Sie sicher, dass die Rückwärtsreplikation auf allen Veröffentlichungsinstanzen konfiguriert ist.

Weitere Informationen zur Transaktionsberichterstellung [Bitte klicken Sie hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
