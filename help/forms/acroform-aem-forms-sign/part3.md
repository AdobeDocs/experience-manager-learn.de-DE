---
title: Acroforms mit AEM Forms
seo-title: Zusammenführen adaptiver Formulardaten mit Acroform
description: Teil 3 eines Tutorials zur Integration von Acroforms mit AEM Forms. Testen Sie den Workflow und das adaptive Formular auf Ihrem System.
feature: adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# Testen Sie diese Funktion auf Ihrem System.

[Laden Sie dieses Paket herunter und importieren Sie es in ](assets/acro-form-aem-form.zip)
AEMT. Dieses Paket enthält den Beispielworkflow und die HTML-Seite, mit der Sie das Schema aus der hochgeladenen Acroform erstellen können.

## Workflow konfigurieren

1. [Öffnen Sie das Workflow-Modell im Bearbeitungsmodus](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Öffnen Sie die Konfigurationseigenschaften des Schritts &quot;MergeAcroformData&quot;.
3. Klicken Sie auf die Registerkarte Prozess .
4. Stellen Sie sicher, dass die Argumente, die Sie übergeben, ein gültiger Ordner auf Ihrem Server sind.
5. Speichern Sie die Änderungen.

## Adaptives Formular erstellen

1. Erstellen Sie ein adaptives Formular mit dem Schema, das Sie im vorherigen Schritt erstellt haben.
2. Ziehen Sie einige Schemaelemente in das adaptive Formular.
3. Konfigurieren Sie die Sendeaktion des adaptiven Formulars, um an AEM Workflow (MergeAcroformData) zu senden.
4. **Stellen Sie sicher, dass Sie den Datendateipfad als &quot;Data.xml&quot;angeben. Dies ist sehr wichtig, da der Beispielcode in der Workflow-Nutzlast nach einer Datei namens Data.xml sucht.**
5. Vorschau des adaptiven Formulars anzeigen, Formular ausfüllen und senden.
6. Sie sollten die PDF-Datei mit den zusammengeführten Daten in dem Ordner sehen, der in Schritt 4 unter dem Workflow &quot;Konfigurieren&quot;angegeben ist.

>[!NOTE]
>
>Das durch Zusammenführen von Daten mit dem Acrobat generierte PDF-Dokument wird als pdfdocument.pdf im Payload-Ordner des Workflows gespeichert. Dieses Dokument kann dann im Rahmen des Workflows für die weitere Verarbeitung verwendet werden
