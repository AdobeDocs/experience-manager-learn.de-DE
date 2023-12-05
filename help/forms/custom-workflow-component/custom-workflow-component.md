---
title: Erstellen einer Workflow-Komponente zum Speichern von Formularanhängen im Dateisystem
description: Schreiben von adaptiven Formularanhängen in das Dateisystem mithilfe einer benutzerdefinierten Workflow-Komponente
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
duration: 95
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 100%

---

# Benutzerdefinierte Workflow-Komponente

Dieses Tutorial richtet sich an AEM Forms-Kundinnen und -Kunden, die eine benutzerdefinierte Workflow-Komponente erstellen müssen. Die Workflow-Komponente wird so konfiguriert, dass der im vorherigen Schritt geschriebene Code ausgeführt wird. Die Workflow-Komponente kann Prozessargumente für den Code angeben. In diesem Artikel werden wir die mit dem Code verknüpfte Workflow-Komponente untersuchen.


[Laden Sie die benutzerdefinierte Workflow-Komponente herunter.](assets/saveFiles.zip)
Importieren Sie die Workflow-Komponente mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

Die benutzerdefinierte Workflow-Komponente befindet sich unter „/apps/AEMFormsDemoListings/workflowcomponent/SaveFiles“.

Wählen Sie den Knoten „SaveFiles“ aus und überprüfen Sie seine Eigenschaften.

Der Wert der Eigenschaft **componentGroup** bestimmt die Kategorie der Workflow-Komponente.

**jcr:Title** ist der Titel der Workflow-Komponente.

Der Wert der Eigenschaft **sling:resourceSuperType** bestimmt die Vererbung dieser Komponente. In diesem Fall erfolgt die Vererbung von der Prozesskomponente.


![component-properties](assets/component-properties1.png)

## cq:dialog

Dialogfelder werden verwendet, um Autorinnen und Autoren die Interaktion mit der Komponente zu ermöglichen. „cq:dialog“ befindet sich unter dem Knoten „SaveFiles“.
![cq-dialog](assets/cq-dialog.png)

Die Knoten unter dem Elementknoten stehen für die Registerkarten der Komponente, durch die Autorinnen und Autoren mit der Komponente interagieren. Die Registerkarten „Allgemein“ und „Prozess“ sind ausgeblendet. Die Registerkarten „Allgemein“ und „Argumente“ werden angezeigt.

Die Prozessargumente für den Prozess befinden sich unter dem Knoten „processargs“.

![process-args](assets/process-arguments.png)

Die Autorin oder der Autor gibt die Argumente an, wie im Screenshot unten dargestellt.
![workflow-component](assets/custom-workflow-component.png)

Die Werte werden als Eigenschaften des Metadatenknotens gespeichert. Beispielsweise wird der Wert **c:\formsattachments** in der Eigenschaft „saveToLocation“ des Metadatenknotens gespeichert.
![save-location](assets/save-to-location.png)

## cq:editConfig

„cq:EditConfig“ ist einfach ein Knoten mit dem primären Typ „cq:EditConfig“ und dem Namen „cq:editConfig“ unter dem Komponentenstamm.
Das Bearbeitungsverhalten einer Komponente wird konfiguriert, indem ein cq:editConfig-Knoten vom Typ „cq:EditConfig“ unter dem Komponentenknoten (vom Typ „cq:Component“) hinzugefügt wird.

![edit-config](assets/cq-edit-config.png)

„cq:formParameters (Knotentyp „nt:unstructured“) definiert zusätzliche Parameter, die zum Dialogfeldformular hinzugefügt werden.


Beachten Sie die Eigenschaften des Knotens „cq:formParameters“.
![from-parameters-properties](assets/form-parameters-properties.png)

Der Wert der Eigenschaft PROCESS gibt den Java-Code an, der mit der Workflow-Komponente verknüpft wird.
