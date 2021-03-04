---
title: Hinzufügen von Komponenten zum Bedienfeld "Einkommen"
seo-title: Hinzufügen von Komponenten zum Bedienfeld "Einkommen"
description: Wir fügen eine Tabelle zum Einkommensbedienfeld hinzu. Konfigurieren Sie die Tabellenzeilen und verwenden Sie den Regeleditor, um die Gesamtsumme zu berechnen.
seo-description: Wir fügen eine Tabelle zum Einkommensbedienfeld hinzu. Konfigurieren Sie die Tabellenzeilen und verwenden Sie den Regeleditor, um die Gesamtsumme zu berechnen.
uuid: d5c98561-c559-4624-976a-7a1486da7e69
feature: Adaptive Formulare
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
discoiquuid: fa483260-38ff-40d8-96a7-1de11d8b792b
topic: Entwicklung
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 1%

---


# Hinzufügen von Komponenten zum Einkommensbedienfeld {#adding-components-to-income-panel}

Wir fügen eine Tabelle zum Einkommensbedienfeld hinzu. Konfigurieren Sie die Tabellenzeilen und verwenden Sie den Regeleditor, um die Gesamtsumme zu berechnen.

**hinzufügen und Konfigurieren der Tabellenkomponente**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=9&learn=on)



## Tabelle mit Einkommen dynamisch machen {#make-the-income-table-dynamic}

**Stellen Sie sicher, dass Sie sich im Bearbeitungsmodus befinden. Die Schaltfläche &quot;Bearbeiten&quot;befindet sich oben rechts im Browser.**

* Wenn Sie eine Tabelle in ein adaptives Formular einfügen, ist die Tabelle standardmäßig nicht dynamisch. Es ist also nicht möglich, der Tabelle zur Laufzeit neue Zeilen hinzuzufügen.

* Aktualisieren Sie Ihren Browser.

* Wählen Sie in der Inhaltshierarchie die Option &quot;Zeile1&quot;aus.

* Klicken Sie auf das Schraubenschlüsselsymbol, um das Eigenschaftenblatt zu öffnen.

* Legen Sie unter Wiederholungseinstellungen die Werte für Minimum und Maximum auf 1 und 5 fest und speichern Sie Ihre Änderungen, indem Sie auf das blaue Häkchen klicken. Das bedeutet, dass die Tabelle maximal 5 Zeilen enthalten kann. Um eine unbegrenzte Anzahl von Zeilen festzulegen, setzen Sie die maximale Anzahl auf -1.

## Regel zur Berechnung der Gesamtsumme {#create-rule-to-calculate-grand-total} erstellen


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=9&learn=on)


