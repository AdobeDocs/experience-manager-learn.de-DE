---
title: Einführung in das Auslösen eines AEM-Workflows für die Übermittlung von HTML5-Formularen
description: Trigger und AEM Workflow bei der Übermittlung mobiler Formulare
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 20%

---

# Trigger AEM Workflow bei der Übermittlung von Mobile Forms

Ein gängiges Nutzungsszenario besteht darin, die XDP als HTML für Datenerfassungsaktivitäten wiederzugeben. Bei Übermittlung dieses Formulars kann es erforderlich sein, einen AEM Workflow Trigger. Im AEM Workflow können wir die Daten mit der XDP-Vorlage zusammenführen und die generierte PDF zur Überprüfung und Genehmigung präsentieren. Das Formular wird auf einer Veröffentlichungsinstanz wiedergegeben und der Workflow wird auf einer AEM Verarbeitungsinstanz ausgelöst.
Die folgenden Schritte sind im Anwendungsfall erforderlich

* Der Benutzer füllt ein HTML5-Formular (HTML5-Ausgabe von XDP) aus und sendet es.
* Die Übermittlung wird von einem Servlet in der Veröffentlichungsinstanz verarbeitet.
* Das Servlet speichert die Daten in einem vordefinierten Ordner im Repository der AEM-Verarbeitungsinstanz.
* Der Workflow-Starter ist so konfiguriert, dass jedes Mal, wenn eine neue Datei unter einem bestimmten Ordner hinzugefügt wird, ein AEM-Workflow Trigger wird.

In diesem Tutorial werden die Schritte erläutert, die zum Erreichen des oben genannten Anwendungsbeispiels erforderlich sind. Beispiel-Code und -Assets im Zusammenhang mit diesem Tutorial sind [hier verfügbar.](./deploy-assets.md)


## Nächste Schritte

[Durchführen der Formularübermittlung](./handle-form-submission.md)