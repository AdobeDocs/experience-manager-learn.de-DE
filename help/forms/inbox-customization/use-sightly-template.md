---
title: Anpassung des Postfachs
description: hinzufügen benutzerdefinierter Spalten, um zusätzliche Daten des Workflows mit einer Vorlage anzuzeigen
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 8%

---

# Anzeigen von Posteingangsdaten mit einer Vorlage

Sie können eine Vorlage verwenden, um die Daten zu formatieren, die in Posteingangsspalten angezeigt werden sollen. In diesem Beispiel werden je nach Wert der Einkommensspalte Symbole für Koral-Ui angezeigt. Der folgende Screenshot zeigt die Verwendung von Symbolen in der Einkommensspalte
![Einkommen-Symbole](assets/income-column.PNG)

[Die leicht ](assets/sightly-template.zip) zu bedienende Vorlage zum Anzeigen der benutzerdefinierten Coral-Ui-Symbole ist Teil dieses Artikels.

## Sightly-Vorlage

Im Folgenden finden Sie die Vorlage. Der Code in der Vorlage zeigt das Symbol abhängig vom Einkommen an. Die Symbole sind als Teil der [Korallen-UI-Bibliothek](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) verfügbar, die mit AEM geliefert wird.

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## Dienstimplementierung

Der folgende Code ist die Dienstimplementierung zur Anzeige der Einkommensspalte.

Zeile 12 ordnet die Spalte der Vorlage zu

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## Testen auf dem Server

>[!NOTE]
>
>In diesem Artikel wird davon ausgegangen, dass Sie den [Beispielworkflow](assets/review-workflow.zip) und das [Beispielformular](assets/snap-form.zip) aus [vorherigen Artikel](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md) in dieser Reihe installiert haben.

* [Bei CRX als Administrator anmelden](http://localhost:4502/crx/de/index.jsp)
* [Vorlage importieren](assets/sightly-template.zip)
* [Bei AEM Webkonsole anmelden](http://localhost:4502/system/console/bundles)
* [Anpassungspaket für Beginn und Posteingang bereitstellen](assets/income-column-customization.jar)
* [Posteingang öffnen](http://localhost:4502/aem/inbox)
* Öffnen Sie die Admin-Steuerung, indem Sie neben der Schaltfläche Erstellen auf Liste Ansicht klicken
* hinzufügen Einkommensspalte in Inbox und speichern Sie Ihre Änderungen
* [Nutzen Sie die Funktion zur Formularvorschau.](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Wählen Sie _Familienstand_ und senden Sie das Formular
* [Ansicht-Posteingang](http://localhost:4502/aem/inbox)

Beim Senden des Formulars wird der Arbeitsablauf Trigger und dem Benutzer &quot;admin&quot;wird eine Aufgabe zugewiesen. Sie sollten das entsprechende Symbol unter der Einkommensspalte sehen
