---
title: Variablen in AEM Workflow[Teil 2]
description: Verwenden von Variablen des Typs XML, JSON, ArrayList, Document in einem AEM Workflow
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# Variablen vom Typ JSON im AEM Workflow

Ab AEM Forms 6.5 können wir jetzt Variablen vom Typ JSON im AEM Workflow erstellen. In der Regel erstellen Sie Variablen des Typs JSON, wenn Sie die adaptive Forms basierend auf einem JSON-Schema an einen AEM Workflow senden oder die Ergebnisse eines Aufrufvorgangs für ein Formulardatenmodell speichern möchten. Das folgende Video führt Sie durch die Schritte, die zum Erstellen und Verwenden einer Variablen vom Typ JSON im AEM Workflow erforderlich sind

**Bei Verwendung von AEM Forms 6.5.0**

Wenn Sie eine Variable des Typs JSON erstellen, um die gesendeten Daten in Ihrem Workflow-Modell zu erfassen, verknüpfen Sie das JSON-Schema nicht mit der -Variablen. Dies liegt daran, dass beim Senden des JSON-Schema-basierten adaptiven Formulars die gesendeten Daten nicht mit dem JSON-Schema konform sind. Die JSON-Schema-Beschreibungsdaten sind im Datenelement afData.afBoundData.data eingeschlossen.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Bei Verwendung von AEM Forms 6.5.1 und höher**

Sie können das Schema der Variablen vom Typ JSON in Ihrem Workflow-Modell zuordnen. Sie können dann den Schema-Browser verwenden, um die Schemaelemente Ihren String-/Zahlenvariablen in Ihrem Workflow-Modell zuzuordnen

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Gehen Sie wie folgt vor, um die Assets auf Ihrem System verwenden zu können:

* [Herunterladen und Importieren von Assets in AEM mit Package Manager](assets/jsonandstringvariable.zip)
* [Workflow-Modell durchsuchen](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) um die im Workflow verwendeten Variablen zu verstehen.
* [E-Mail-Dienst konfigurieren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Öffnen Sie das adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und senden Sie das Formular
