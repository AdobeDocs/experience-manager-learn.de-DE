---
title: Hinzufügen benutzerdefinierter Spalten
description: Fügen Sie benutzerdefinierte Spalten hinzu, um zusätzliche Daten des Workflows anzuzeigen
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
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 8%

---

# Hinzufügen benutzerdefinierter Spalten

Um Workflow-Daten im Posteingang anzuzeigen, müssen wir Variablen im Workflow definieren und ausfüllen. Der Wert der Variablen muss festgelegt werden, bevor eine Aufgabe einem Benutzer zugewiesen wird. Um Ihnen einen Vorsprung zu verschaffen, haben wir einen Beispiel-Workflow bereitgestellt, der für die Bereitstellung auf Ihrem AEM-Server bereit ist.

* [Bei AEM anmelden](http://localhost:4502/crx/de/index.jsp)
* [Importieren des Überprüfungs-Workflows](assets/review-workflow.zip)
* [Workflow überprüfen](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Für diesen Workflow sind zwei Variablen definiert (isVerheiratet und Einkommen). Die Werte werden mithilfe der Komponente set variable festgelegt. Diese Variablen werden als Spalten zur Verfügung gestellt, die AEM Posteingang hinzugefügt werden können

## Dienst erstellen

Für jede Spalte, die wir in unserem Posteingang anzeigen müssen, müssten wir einen Dienst schreiben. Mit dem folgenden Dienst können wir eine Spalte hinzufügen, um den Wert der Variablen isMarry anzuzeigen

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
>Sie müssen AEM 6.5.5 Uber.jar in Ihr Projekt einbeziehen, damit der obige Code funktioniert

![uber-jar](assets/uber-jar.PNG)

## Testen auf Ihrem Server

* [Melden Sie sich bei AEM Web Console an](http://localhost:4502/system/console/bundles)
* [Posteingang-Anpassungs-Bundle bereitstellen und starten](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Posteingang öffnen](http://localhost:4502/aem/inbox)
* Öffnen Sie Admin Control durch Klicken auf _Listenansicht_ Symbol neben _Erstellen_ button
* Spalte &quot;Verheiratet&quot;zum Posteingang hinzufügen und Ihre Änderungen speichern
* [Navigieren Sie zur Benutzeroberfläche &quot;FormsAndDocuments&quot;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importieren des Beispielformulars](assets/snap-form.zip) durch Auswahl _Datei-Upload_ von _Erstellen_ Menü
* [Nutzen Sie die Funktion zur Formularvorschau.](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Wählen Sie die _Familienstand_ und senden Sie das Formular
   [Posteingang anzeigen](http://localhost:4502/aem/inbox)

Durch das Senden des Formulars wird der Workflow Trigger und eine Aufgabe wird &quot;admin&quot;-Benutzern zugewiesen. In diesem Screenshot sollte unter der Spalte Verheiratet ein Wert angezeigt werden

![verheiratete Spalte](assets/married-column.PNG)
