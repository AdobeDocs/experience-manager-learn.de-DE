---
title: Einführung in das Auslösen eines AEM-Workflows für die Übermittlung von HTML5-Formularen
description: Auslösen eines AEM-Workflows bei der Übermittlung eines Formulars für Mobilgeräte
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 100%

---

# Auslösen eines AEM-Workflows bei der Übermittlung eines Formulars für Mobilgeräte

Ein gängiger Anwendungsfall besteht darin, die XDP als HTML für Datenerfassungsaktivitäten zu rendern. Bei Übermittlung dieses Formulars kann es erforderlich sein, einen AEM-Workflow auszulösen. Im AEM-Workflow können Sie die Daten mit der XDP-Vorlage fusionieren und die generierte PDF zur Überprüfung und Genehmigung vorlegen. Das Formular wird auf einer Veröffentlichungsinstanz gerendert und der Workflow wird auf einer AEM-Verarbeitungsinstanz ausgelöst.

Die folgenden Schritte sind Teil des Anwendungsfalls

* Benutzende füllen ein HTML5-Formular (HTML5-Ausgabedarstellung von XDP) aus und übermittlen es.
* Die Übermittlung wird von einem Servlet in der Veröffentlichungsinstanz verarbeitet.
* Das Servlet speichert die Daten in einem vordefinierten Ordner im Repository der AEM-Verarbeitungsinstanz.
* Der Workflow-Starter ist so konfiguriert, dass jedes Mal ein AEM-Workflow ausgelöst wird, wenn eine neue Datei unter einem bestimmten Ordner hinzugefügt wird.

In diesem Tutorial werden die Schritte erläutert, die zum Erreichen des oben genannten Anwendungsbeispiels erforderlich sind. Beispiel-Code und -Assets im Zusammenhang mit diesem Tutorial sind [hier verfügbar.](./deploy-assets.md)


## Nächste Schritte

[Durchführen der Formularübermittlung](./handle-form-submission.md)
