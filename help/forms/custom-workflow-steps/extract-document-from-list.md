---
title: Extrahieren eines Dokuments aus einer Dokumentenliste in einen AEM-Workflow
description: Benutzerdefinierte Workflow-Komponente zum Extrahieren eines bestimmten Dokuments aus einer Dokumentenliste
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
exl-id: b0baac71-3074-49d5-9686-c9955b096abb
duration: 72
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '278'
ht-degree: 100%

---

# Extrahieren eines Dokuments aus einer Dokumentenliste

Ein gängiger Anwendungsfall besteht darin, die Formulardaten und den Formularanhang mithilfe des Schritts „Formulardatenmodell aufrufen“ eines AEM-Workflows an ein externes System zu senden. Wenn Sie z. B. in ServiceNow einen Fall erstellen, können Sie die Falldetails mit einem unterstützenden Dokument senden. Die Anlagen, die zum adaptiven Formular hinzugefügt werden, werden in einer Variablen des Typs „arraylist von Dokumenten“ gespeichert. Um ein bestimmtes Dokument aus dieser arraylist zu extrahieren, müssen Sie benutzerdefinierten Code schreiben.

In diesem Artikel werden Sie durch die Schritte geführt, die zur Verwendung der benutzerdefinierten Workflow-Komponente zum Extrahieren und Speichern des Dokuments in einer Dokumentvariablen erforderlich sind.

## Erstellen eines Workflows

Zur Übermittlung des Formulars muss ein Workflow erstellt werden. Für den Workflow müssen folgende Variablen definiert werden:

* Eine Variable vom Typ „ArrayList von Dokumenten“ (Diese Variable enthält die von der Benutzerin bzw. vom Benutzer hinzugefügten Formularanhänge)
* Eine Variable vom Typ „Dokument“.(Diese Variable enthält das aus der ArrayList extrahierte Dokument)

* Fügen Sie eine benutzerdefinierte Komponente zum Workflow hinzu und konfigurieren Sie deren Eigenschaften
  ![extract-item-workflow](assets/extract-document-array-list.png)

## Konfigurieren eines adaptiven Formulars

* Konfigurieren Sie die Übermittlungsaktion des adaptiven Formulars, um den AEM-Workflow auszulösen
  ![submit-action](assets/store-attachments.png)

## Testen der Lösung

[Stellen Sie das benutzerdefinierte Bundle über die OSGi-Web-Konsole bereit](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[Importieren Sie die Workflow-Komponente mit Package Manager](assets/Extract-item-from-documents-list.zip)

[Importieren Sie den Beispiel-Workflow](assets/extract-item-sample-workflow.zip)

[Importieren Sie das adaptive Formular](assets/test-attachment-extractions-adaptive-form.zip)

[Vorschau des Formulars](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)

Fügen Sie einen Anhang zum Formular hinzu und senden Sie es ab.

>[!NOTE]
>
>Das extrahierte Dokument kann dann in jedem anderen Workflow-Schritt, wie „E-Mail senden“ oder „FDM-Schritt aufrufen“ verwendet werden
