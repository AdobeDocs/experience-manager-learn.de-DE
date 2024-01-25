---
title: Acroforms mit AEM Forms
description: Teil 3 eines Tutorials zur Integration von Acroforms mit AEM Forms. Testen Sie den Workflow und das adaptive Formular auf Ihrem System.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 49
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 100%

---


# Testen Sie diese Funktion auf Ihrem System

[Laden Sie dieses Paket herunter und importieren Sie es in AEM](assets/acro-form-aem-form.zip)
Dieses Paket enthält den Beispiel-Workflow und die HTML-Seite, mit der Sie das Schema aus dem hochgeladenen Acroform erstellen können.

## Konfigurieren des Workflows

1. [Öffnen Sie das Workflow-Modell im Bearbeitungsmodus](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Öffnen Sie die Konfigurationseigenschaften des Schritts „MergeAcroformData“.
3. Klicken Sie auf die Registerkarte „Prozess“.
4. Stellen Sie sicher, dass die Argumente, die Sie übergeben, ein gültiger Ordner auf Ihrem Server sind.
5. Speichern Sie die Änderungen.

## Erstellen eines adaptiven Formulars

1. Erstellen Sie ein adaptives Formular mit dem Schema, das Sie im vorherigen Schritt erstellt haben.
2. Ziehen Sie einige Schemaelemente in das adaptive Formular.
3. Konfigurieren Sie die Sendeaktion des adaptiven Formulars, um sie an den AEM-Workflow (MergeAcroformData) zu senden.
4. **Stellen Sie sicher, dass Sie den Datendateipfad als „Data.xml“ angeben. Dies ist sehr wichtig, da der Beispiel-Code in der Payload des Workflows nach einer Datei namens „Data.xml“ sucht.**
5. Lassen Sie sich die Vorschau des adaptiven Formulars anzeigen, füllen Sie das Formular aus und senden Sie es ab.
6. Sie sollten die PDF mit den zusammengeführten Daten in dem Ordner sehen, der in Schritt 4 unter dem Workflow „Konfigurieren“ angegeben wurde.

>[!NOTE]
>
>Das durch Zusammenführen von Daten mit dem Acrobat generierte PDF-Dokument wird als pdfdocument.pdf im Payload-Ordner des Workflows gespeichert. Dieses Dokument kann dann im Rahmen des Workflows für die weitere Verarbeitung verwendet werden
