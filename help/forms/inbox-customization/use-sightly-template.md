---
title: Verwenden einer Sightly-Vorlage zur Anzeige von Posteingangsdaten
description: Hinzufügen benutzerdefinierter Spalten, um mithilfe einer Sightly-Vorlage zusätzliche Daten aus einem Workflow anzuzeigen
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 83
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# Verwenden einer Sightly-Vorlage zur Anzeige von Posteingangsdaten

Sie können eine Sightly-Vorlage verwenden, um die Daten zu formatieren, die in Posteingangsspalten angezeigt werden sollen. In diesem Beispiel werden je nach Wert der Einkommensspalte Coral-UI-Symbole angezeigt. Der folgende Screenshot zeigt die Verwendung von Symbolen in der Einkommensspalte
![income-icons](assets/income-column.PNG)

[Die Sightly-Vorlage](assets/sightly-template.zip) zur Anzeige der benutzerdefinierten Coral-UI-Symbole wird als Teil dieses Artikels bereitgestellt.

## Sightly-Vorlage

Im Folgenden finden Sie die Sightly-Vorlage. Der Code in der Vorlage zeigt je nach Einkommen ein Symbol an. Die Symbole sind als Teil der [Coral-UI-Symbolbibliothek](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) verfügbar, die AEM enthält.

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

Zeile 12 ordnet die Spalte der Sightly-Vorlage zu

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
>In diesem Artikel wird davon ausgegangen, dass Sie den [Beispiel-Workflow](assets/review-workflow.zip) und das [Beispielformular](assets/snap-form.zip) vom [vorherigen Artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html?lang=de) in dieser Serie installiert haben.

* [Melden Sie sich bei CRX als Admin an](http://localhost:4502/crx/de/index.jsp)
* [Importieren einer Sightly-Vorlage](assets/sightly-template.zip)
* [Melden Sie sich bei der AEM Web-Konsole an](http://localhost:4502/system/console/bundles)
* [Stellen Sie das Anpassungspaket für den Posteingang bereit und starten Sie es.](assets/income-column-customization.jar)
* [Öffnen Sie Ihren Posteingang](http://localhost:4502/aem/inbox)
* Öffnen Sie Admin Control, indem Sie auf die Listenansicht neben der Schaltfläche „Erstellen“ klicken.
* Fügen Sie eine Einkommensspalte zum Posteingang hinzu und speichern Sie Ihre Änderungen.
* [Zeigen Sie das Formular in der Vorschau an](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled). 
* Wählen Sie den _Familienstand_ aus und übermitteln Sie das Formular.
* [Zeigen Sie den Posteingang an](http://localhost:4502/aem/inbox).

Durch Übermittlung des Formulars wird der Workflow ausgelöst und der Admin-Benutzerin bzw. dem Admin-Benutzer eine Aufgabe zugewiesen. Unter der Einkommensspalte sollte das entsprechende Symbol angezeigt werden.
