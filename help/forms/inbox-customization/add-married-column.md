---
title: Anpassung des Postfachs
description: hinzufügen benutzerdefinierter Spalten zur Anzeige zusätzlicher Daten des Workflows
feature: adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 7%

---


# Hinzufügen benutzerdefinierter Spalten 

Um Workflow-Daten im Posteingang anzuzeigen, müssen wir Variablen im Workflow definieren und füllen. Der Variablenwert muss festgelegt werden, bevor eine Aufgabe einem Benutzer zugewiesen wird. Um Ihnen einen Beginn zu geben, haben wir einen Beispielarbeitsablauf bereitgestellt, der auf Ihrem AEM bereitgestellt werden kann.

* [Bei AEM anmelden](http://localhost:4502/crx/de/index.jsp)
* [Review-Arbeitsablauf importieren](assets/review-workflow.zip)
* [Workflow überprüfen](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Dieser Arbeitsablauf umfasst zwei Variablen (isVerheiratet und Einkommen) und seine Werte werden mithilfe der Komponente set variable festgelegt. Diese Variablen stehen als Spalten zur Verfügung, die AEM Posteingang hinzugefügt werden sollen

## Dienst erstellen

Für jede Spalte, die wir in unserem Posteingang anzeigen müssen, müssten wir einen Dienst schreiben. Mit dem folgenden Dienst können wir eine Spalte hinzufügen, um den Wert der Variablen isMarried anzuzeigen

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
>Sie müssen AEM 6.5.5 Uber.jar in Ihr Projekt aufnehmen, damit der oben genannte Code funktioniert

![uber-jar](assets/uber-jar.PNG)

## Testen auf dem Server

* [Bei AEM Webkonsole anmelden](http://localhost:4502/system/console/bundles)
* [Anpassungspaket für Beginn und Posteingang bereitstellen](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Posteingang öffnen](http://localhost:4502/aem/inbox)
* Öffnen Sie die Admin-Steuerung, indem Sie auf das Symbol _Liste-Ansicht_ neben _Erstellen_ klicken
* hinzufügen Spalte &quot;Verheiratet&quot;in &quot;Posteingang&quot;und speichern Sie Ihre Änderungen
* [Wechseln zur Benutzeroberfläche &quot;FormsAndDocuments&quot;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importieren Sie das Beispielformular, ](assets/snap-form.zip) indem Sie im Menü &quot; _Erstellen&quot;die Option_ &quot;Datei  __ hochladen&quot;wählen
* [Nutzen Sie die Funktion zur Formularvorschau.](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Wählen Sie _Familienstand_ und senden Sie das Formular
   [Ansicht-Posteingang](http://localhost:4502/aem/inbox)

Beim Senden des Formulars wird der Arbeitsablauf Trigger und dem Benutzer &quot;admin&quot;wird eine Aufgabe zugewiesen. Sie sollten einen Wert unter der Spalte Verheiratet sehen, wie in diesem Screenshot dargestellt

![heiratet-column](assets/married-column.PNG)
