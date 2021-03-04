---
title: Verwenden von Transaction Berichte in AEM Forms
seo-title: Verwenden von Transaction Berichte in AEM Forms
description: Mit Transaktionsberichten in AEM Forms können Sie alle Transaktionen zählen, die seit einem bestimmten Datum bei Ihrer AEM Forms-Bereitstellung stattgefunden haben.
seo-description: Mit Transaktionsberichten in AEM Forms können Sie alle Transaktionen zählen, die seit einem bestimmten Datum bei Ihrer AEM Forms-Bereitstellung stattgefunden haben.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: Adaptive Formulare
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: Entwicklung
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 2%

---


# Verwenden von Transaction Berichte in AEM Forms{#using-transaction-reporting-in-aem-forms}

Mit AEM Forms 6.4.1 wurde der Transaktions-Berichte eingeführt, um die Anzahl der Formularübermittlungen, die Wiedergabe von Dokumenten mithilfe von Dokument-Diensten und die Wiedergabe interaktiver  (Web- und Print-Kanal) zu erfassen. Diese Funktion richtet sich vor allem an Kunden, die die Software auf der Grundlage der Anzahl der gesendeten Formulare und/oder Dokumente lizenzieren möchten. Diese Funktion ist derzeit nur für AEM Forms OSGI-Stacks verfügbar.

## Aktivieren von Transaction Berichte {#enabling-transaction-reporting}

Standardmäßig ist die Transaktionsaufzeichnung deaktiviert. Gehen Sie wie folgt vor, um die Transaktionsaufzeichnung zu aktivieren:

* [configMgr öffnen](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach &quot;Forms Transaction Berichte&quot;
* Kontrollkästchen &quot;Vorgänge aufzeichnen&quot;
* Speichern Sie Ihre Änderungen

Nachdem Sie den Berichte &quot;transaction&quot;aktiviert haben, können Sie adaptive Forms senden, Dokumente mithilfe von Dokument-Diensten generieren oder interaktive Kommunikationsmodule rendern, um den Berichte von Transaktionen in Aktion zu sehen.

## Ansicht des Transaktionsberichts {#viewing-transaction-report}

Melden Sie sich zur Ansicht des Transaktionsberichts als Administrator bei AEM Forms an. Der Transaktionsbericht kann nur von Mitgliedern der Gruppe &quot;fd-Administrator&quot;Ansicht werden.

Tools auswählen | Forms | Ansicht-Transaktionsbericht

oder Ansicht des Transaktionsberichts durch Klicken auf [hier](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransctionReporting](assets/transactionreporting.gif)

Im Screenshot oben Dokument Verarbeitete ist die Anzahl der Dokumente, die mit Dokument-Diensten generiert wurden. Als gerenderte Dokumente wird die Anzahl der wiedergegebenen interaktiven Dokumente (Web und Druck) bezeichnet. Forms Submitted ist die Anzahl der Übermittlungen adaptiver Formulare.

Eine Transaktion bleibt für einen bestimmten Zeitraum im Puffer (Flush-Pufferzeit + Reverse-Replizierungszeit). Standardmäßig dauert es etwa 90 Sekunden, bis die Transaktionsanzahl im Transaktionsbericht angezeigt wird.

Aktionen wie das Senden eines PDF-Formulars, die Verwendung der Agent-Benutzeroberfläche zur Vorschau einer interaktiven Kommunikation oder die Verwendung nicht standardmäßiger Formularübermittlungsmethoden werden nicht als Transaktionen erfasst. AEM Forms stellt eine API zur Aufzeichnung solcher Transaktionen bereit. Rufen Sie die API Ihrer benutzerdefinierten Implementierungen auf, um eine Transaktion aufzuzeichnen.

Wenn Sie den Transaktionsbericht auf der Autoreninstanz anzeigen, stellen Sie sicher, dass die umgekehrte Replizierung auf allen Veröffentlichungsinstanzen konfiguriert ist.

Um mehr über den Transaktions-Berichte [zu erfahren, klicken Sie bitte hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

