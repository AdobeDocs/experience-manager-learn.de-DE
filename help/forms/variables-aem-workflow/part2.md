---
title: Variablen im AEM-Workflow [Teil 2]
description: Verwenden von Variablen des Typs „XML“, „JSON“, „ArrayList“ und „Document“ in einem AEM-Workflow
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '281'
ht-degree: 100%

---

# Variablen vom Typ JSON im AEM-Workflow

Ab AEM Forms 6.5 können nun Variablen vom Typ JSON im AEM-Workflow erstellt werden. In der Regel erstellen Sie Variablen vom Typ JSON, wenn Sie adaptive Formulare basierend auf einem JSON-Schema an einen AEM-Workflow senden oder die Ergebnisse eines Aufrufvorgangs für ein Formulardatenmodell speichern möchten. Das folgende Video führt Sie durch die Schritte, die zum Erstellen und Verwenden einer Variablen vom Typ JSON im AEM-Workflow erforderlich sind.

**Bei Verwendung von AEM Forms 6.5.0**

Wenn Sie eine Variable vom Typ JSON erstellen, um die übermittelten Daten in Ihrem Workflow-Modell zu erfassen, achten Sie darauf, das JSON-Schema nicht mit der Variablen zu verknüpfen. Denn beim Senden des auf dem JSON-Schema basierenden adaptiven Formulars sind die übermittelten Daten sonst nicht mit dem JSON-Schema konform. Die mit dem JSON-Schema konformen Daten sind im Datenelement „afData.afBoundData.data“ eingeschlossen.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Bei Verwendung von AEM Forms 6.5.1 und höher**

Sie können das Schema der Variablen vom Typ JSON in Ihrem Workflow-Modell zuordnen. Sie können dann den Schema-Browser verwenden, um die Schemaelemente den Zeichenfolgen-/Zahlenvariablen in Ihrem Workflow-Modell zuzuordnen

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Gehen Sie wie folgt vor, um die Assets auf Ihrem System verwenden zu können:

* [Laden Sie die Assets herunter und importieren Sie sie mithilfe von Package Manager in AEM.](assets/jsonandstringvariable.zip)
* [Untersuchen Sie das Workflow-Modell](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html), um die im Workflow verwendeten Variablen zu verstehen.
* [Konfigurieren Sie den E-Mail-Dienst.](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Öffnen Sie das adaptive Formular.](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und übermitteln Sie das Formular.
