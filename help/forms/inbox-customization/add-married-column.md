---
title: Hinzufügen benutzerdefinierter Spalten
description: Hinzufügen benutzerdefinierter Spalten, um zusätzliche Daten des Workflows anzuzeigen
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 100%

---

# Hinzufügen benutzerdefinierter Spalten

Um Workflow-Daten im Posteingang anzuzeigen, müssen wir Variablen im Workflow definieren und befüllen. Der Wert der Variablen muss festgelegt werden, bevor eine Aufgabe einer Benutzerin oder einem Benutzer zugewiesen wird. Damit Sie sofort loslegen können, haben wir Ihnen einen Beispiel-Workflow zur Verfügung gestellt, der direkt auf Ihrem AEM-Server bereitgestellt werden kann.

* [Melden Sie sich bei AEM an.](http://localhost:4502/crx/de/index.jsp)
* [Importieren Sie den Workflow für die Überprüfung.](assets/review-workflow.zip)
* [Überprüfen Sie den Workflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Für diesen Workflow sind zwei Variablen definiert („isMarried“ und „income“). Die zugehörigen Werte werden mithilfe der Komponente zum Festlegen einer Variablen angegeben. Diese Variablen werden als Spalten bereitgestellt, die dem AEM-Posteingang hinzugefügt werden können.

## Erstellen eines Dienstes

Für jede Spalte, die im Posteingang angezeigt werden soll, müssen wir einen Dienst erstellen. Mit dem folgenden Dienst können wir eine Spalte hinzufügen, um den Wert der Variablen „isMarried“ anzuzeigen.

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>Sie müssen „AEM 6.5.5 Uber.jar“ in Ihr Projekt einbeziehen, damit der obige Code funktioniert.

![uber-jar](assets/uber-jar.PNG)

## Testen auf Ihrem Server

* [Melden Sie sich bei der AEM-Web-Konsole an.](http://localhost:4502/system/console/bundles)
* [Stellen Sie das Anpassungspaket für den Posteingang bereit und starten Sie es.](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Öffnen Sie Ihren Posteingang](http://localhost:4502/aem/inbox)
* Öffnen Sie die Admin-Steuerung durch Klicken auf das Symbol _Listenansicht_ neben der Schaltfläche _Erstellen_.
* Fügen Sie die Spalte „Verheiratet“ zum Posteingang hinzu und speichern Sie Ihre Änderungen.
* [Navigieren Sie zur Benutzeroberfläche „FormsAndDocuments“.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importieren Sie das Beispielformular](assets/snap-form.zip), indem Sie im Menü _Erstellen_ die Option _Datei hochladen_ auswählen.
* [Vorschau des Formulars](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Wählen Sie den _Familienstand_ aus und übermitteln Sie das Formular.
  [Posteingang anzeigen](http://localhost:4502/aem/inbox)

Durch Übermittlung des Formulars wird der Workflow ausgelöst und der Admin-Benutzerin bzw. dem Admin-Benutzer eine Aufgabe zugewiesen. In diesem Screenshot sollte unter der Spalte „Verheiratet“ ein Wert angezeigt werden.

![Spalte „Verheiratet“](assets/married-column.PNG)

## Nächste Schritte

[Anzeigen der Spalte „Verheiratet“](./use-sightly-template.md)
