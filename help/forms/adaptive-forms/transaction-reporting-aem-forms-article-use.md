---
title: Verwenden von Transaktionsberichten in AEM Forms
description: Mit Transaktionsberichten in AEM Forms können Sie alle Transaktionen zählen, die seit einem bestimmten Datum in Ihrer AEM Forms-Bereitstellung stattgefunden haben.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 96
source-git-commit: 4b88045a626b5e7bd1386e62ee54ac6fe2ce9282
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 87%

---

# Verwenden von Transaktionsberichten in AEM Forms{#using-transaction-reporting-in-aem-forms}

Mit AEM Forms 6.4.1 wurden Transaktionsberichte eingeführt, um die Anzahl der Formularübermittlungen, die Darstellung von Dokumenten mithilfe von Document Services und die Darstellung interaktiver Kommunikation (Web- und Druckkanäle) zu erfassen. Diese Funktion ist derzeit nur für AEM Forms OSGI-Stack verfügbar.

## Aktivieren von Transaktionsberichten {#enabling-transaction-reporting}

Standardmäßig ist das Aufzeichnen von Transaktionen deaktiviert. Gehen Sie wie folgt vor, um die Transaktionsaufzeichnung zu aktivieren:

* [Öffnen Sie configMgr.](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach „Forms-Transaktionsberichte“.
* Aktivieren Sie das Kontrollkästchen „Transaktionen aufzeichnen“
* Speichern Sie Ihre Änderungen.

Sobald die Transaktionsberichterstellung aktiviert ist, können Sie adaptive Formulare senden, Dokumente mithilfe von Dokumentendiensten generieren oder Dokumente zur interaktiven Kommunikation rendern, um Transaktionsberichte in Aktion zu sehen.

## Anzeigen des Transaktionsberichts {#viewing-transaction-report}

Um den Transaktionsbericht anzuzeigen, melden Sie sich bei AEM Forms als Admin an. Nur Mitglieder der Gruppe fd-Administrator können Transaktionsberichte anzeigen.

Tools auswählen | Formulare | Transaktionsbericht anzeigen

Oder Sie können den Transaktionsbericht durch einen Klick [hierauf](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html) anzeigen.

![Transaktionsbericht](assets/transactionreporting.gif)

Im Screenshot oben ist „Verarbeitete Dokumente“ die Anzahl der Dokumente, die mit Dokumentendiensten generiert wurden. „Gerenderte Dokumente“ entspricht der Anzahl der wiedergegebenen interaktiven Kommunikationsdokumente (Web und Druck). „Übermittelte Formulare“ ist die Anzahl der Übermittlungen adaptiver Formulare.

Eine Transaktion verbleibt für einen bestimmten Zeitraum im Puffer (Leerlauf-Pufferzeit + Rückwärtsreplikationszeit). Standardmäßig dauert es ungefähr 90 Sekunden, bis die Anzahl der Transaktionen im Transaktionsbericht angezeigt wird.

Aktionen wie das Senden eines PDF-Formulars, die Verwendung der Agent-Benutzeroberfläche zur Vorschau einer interaktiven Kommunikation oder die Verwendung nicht standardmäßiger Methoden zur Formularübermittlung werden nicht als Transaktionen gezählt. AEM Forms bietet eine API zum Aufzeichnen solcher Transaktionen. Rufen Sie die API aus Ihren benutzerdefinierten Implementierungen auf, um eine Transaktion aufzuzeichnen.

Wenn Sie den Transaktionsbericht auf der Autoreninstanz anzeigen, stellen Sie sicher, dass die Rückwärtsreplikation auf allen Veröffentlichungsinstanzen konfiguriert ist.

Für weitere Informationen zu Transaktionsberichten [klicken Sie hier](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/transaction-reports-overview.html)
