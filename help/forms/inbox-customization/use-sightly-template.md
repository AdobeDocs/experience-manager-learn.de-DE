---
title: Anpassung des Posteingangs
description: Fügen Sie benutzerdefinierte Spalten hinzu, um mithilfe einer Vorlage zusätzliche Daten aus einem Workflow anzuzeigen
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Development
role: Developer
level: Experienced
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 8%

---

# Sichtbare Vorlage zur Anzeige von Posteingangsdaten verwenden

Sie können eine Sichtvorlage verwenden, um die Daten zu formatieren, die in Posteingangsspalten angezeigt werden sollen. In diesem Beispiel werden je nach Wert der Einkommensspalte Symbole für &quot;coral-ui&quot;angezeigt. Der folgende Screenshot zeigt die Verwendung von Symbolen in der Einkommensspalte
![Einkommen-Symbole](assets/income-column.PNG)

[Die visuelle ](assets/sightly-template.zip) Vorlage zum Anzeigen der benutzerdefinierten Coral-Ui-Symbole wird als Teil dieses Artikels bereitgestellt.

## Sightly-Vorlage

Im Folgenden finden Sie die visuelle Vorlage. Der Code in der Vorlage zeigt je nach Einkommen ein Symbol an. Die Symbole sind als Teil der [coral ui icon library](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) verfügbar, die mit AEM geliefert wird.

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

Zeile 12 ordnet die Spalte der Sichtvorlage zu

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

## Testen auf Ihrem Server

>[!NOTE]
>
>In diesem Artikel wird davon ausgegangen, dass Sie den [Beispiel-Workflow](assets/review-workflow.zip) und das [Beispielformular](assets/snap-form.zip) aus [vorherigen Artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html) in dieser Reihe installiert haben.

* [Melden Sie sich bei crx als Administrator an](http://localhost:4502/crx/de/index.jsp)
* [Sichtbare Importvorlage](assets/sightly-template.zip)
* [Melden Sie sich bei AEM Web Console an](http://localhost:4502/system/console/bundles)
* [Posteingang-Anpassungs-Bundle bereitstellen und starten](assets/income-column-customization.jar)
* [Posteingang öffnen](http://localhost:4502/aem/inbox)
* Öffnen Sie Admin Control, indem Sie auf die Listenansicht neben der Schaltfläche Erstellen klicken.
* Einkommensspalte zum Posteingang hinzufügen und Ihre Änderungen speichern
* [Nutzen Sie die Funktion zur Formularvorschau.](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Wählen Sie den _Familienstand_ aus und senden Sie das Formular
* [Posteingang anzeigen](http://localhost:4502/aem/inbox)

Durch das Senden des Formulars wird der Workflow Trigger und eine Aufgabe wird &quot;admin&quot;-Benutzern zugewiesen. Unter der Einkommensspalte sollte das entsprechende Symbol angezeigt werden.
