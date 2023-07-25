---
title: Acroforms mit AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Teil 3 eines Tutorials zur Integration von Acroforms mit AEM Forms. Testen Sie den Workflow und das adaptive Formular auf Ihrem System.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.5
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 3%

---


# Testen Sie diese Funktion auf Ihrem System.

[Laden Sie dieses Paket herunter und importieren Sie es in AEM](assets/acro-form-aem-form.zip)
Dieses Paket enthält den Beispiel-Workflow und die HTML-Seite, mit der Sie das Schema aus der hochgeladenen Acroform erstellen können.

## Workflow konfigurieren

1. [Öffnen Sie das Workflow-Modell im Bearbeitungsmodus.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Öffnen Sie die Konfigurationseigenschaften des Schritts &quot;MergeAcroformData&quot;.
3. Klicken Sie auf die Registerkarte Prozess .
4. Stellen Sie sicher, dass die Argumente, die Sie übergeben, ein gültiger Ordner auf Ihrem Server sind.
5. Speichern Sie die Änderungen.

## Erstellen eines adaptiven Formulars

1. Erstellen Sie ein adaptives Formular mit dem Schema, das Sie im vorherigen Schritt erstellt haben.
2. Ziehen Sie einige Schemaelemente in das adaptive Formular.
3. Konfigurieren Sie die Sendeaktion des adaptiven Formulars, um an AEM Workflow (MergeAcroformData) zu senden.
4. **Stellen Sie sicher, dass Sie den Datendateipfad als &quot;Data.xml&quot;angeben. Dies ist sehr wichtig, da der Beispielcode in der Workflow-Payload nach einer Datei namens Data.xml sucht.**
5. Vorschau des adaptiven Formulars anzeigen, Formular ausfüllen und senden.
6. Sie sollten die PDF mit den zusammengeführten Daten in dem Ordner sehen, der in Schritt 4 unter dem Workflow &quot;Konfigurieren&quot;angegeben ist.

>[!NOTE]
>
>Das durch Zusammenführen von Daten mit dem Acrobat generierte PDF-Dokument wird als pdfdocument.pdf im Payload-Ordner des Workflows gespeichert. Dieses Dokument kann dann im Rahmen des Workflows für die weitere Verarbeitung verwendet werden
