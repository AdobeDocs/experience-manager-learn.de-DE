---
title: Variablen im AEM Workflow[Teil 2]
seo-title: Variablen im AEM Workflow[Teil 2]
description: Variablen des Typs xml,json,arraylist,Dokument im AEM-Workflow verwenden
seo-description: Variablen des Typs xml,json,arraylist,Dokument im AEM-Workflow verwenden
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Variablen des Typs JSON in AEM Workflow

Ab AEM Forms 6.5 können wir jetzt Variablen vom Typ JSON im AEM Workflow erstellen. Normalerweise erstellen Sie Variablen vom Typ JSON, wenn Sie adaptives Forms basierend auf JSON-Schema an einen AEM Workflow senden oder die Ergebnisse eines Formulardatenmodellaufrufvorgangs speichern möchten. Im folgenden Video werden die Schritte erläutert, die zum Erstellen und Verwenden einer Variablen vom Typ JSON im Arbeitsablauf AEM erforderlich sind
>[!NOTE]

**Bei Verwendung von AEM Forms 6.5.0**

Wenn Sie eine Variable vom Typ JSON erstellen, um die gesendeten Daten in Ihrem Workflow-Modell zu erfassen, verknüpfen Sie das JSON-Schema nicht mit der Variablen. Dies liegt daran, dass die gesendeten Daten beim Senden des JSON-Schemas-basierten adaptiven Formulars nicht mit dem JSON-Schema konform sind. Die JSON-Schema-Beschreibungsdaten sind in das afData.afBoundData.data-Element eingeschlossen.

**Bei Verwendung von AEM Forms 6.5.1 und höher**

Sie können das Schema der Variablen vom Typ JSON in Ihrem Workflow-Modell zuordnen. Anschließend können Sie mit dem Schema-Browser die Schema-Elemente mit Ihren String-/Zahlenvariablen in Ihrem Workflow-Modell verknüpfen

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)

**Die Möglichkeit zum Drilldown von Schema-Elementen und zum Zuordnen des Schema-Elements zur Workflow-Variablen ist nur ab AEM Forms 6.5.1 verfügbar.**

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Um die Assets auf Ihrem System zu verwenden, führen Sie die folgenden Schritte aus:

* [Herunterladen und Importieren von Assets in AEM mit dem Paketmanager](assets/jsonandstringvariable.zip)
* [Durchsuchen Sie das Workflow-Modell](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) , um die Variablen zu verstehen, die im Workflow verwendet werden.
* [E-Mail-Dienst konfigurieren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Adaptives Formular öffnen](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und senden Sie das Formular
