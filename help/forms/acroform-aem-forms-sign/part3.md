---
title: Acroforms mit AEM Forms
seo-title: Adaptive Formulardaten mit Acroform zusammenführen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 1%

---


# Testen Sie diese Funktion auf Ihrem System

[Laden Sie dieses Paket herunter und importieren Sie es in AEM](assets/acro-form-aem-form.zip)Dieses Paket enthält den Beispielarbeitsablauf und die HTML-Seite, auf der Sie das Schema aus der hochgeladenen Acroform erstellen können.

## Arbeitsablauf konfigurieren

1. [Öffnen Sie das Workflow-Modell im Bearbeitungsmodus](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Öffnen Sie die Konfigurationseigenschaften des MergeAcroformData-Schritts.
3. Klicken Sie auf die Registerkarte Prozess.
4. Stellen Sie sicher, dass die übergebenen Argumente ein gültiger Ordner auf Ihrem Server sind.
5. Speichern Sie die Änderungen.

## Adaptives Formular erstellen

1. Erstellen Sie ein adaptives Formular mit dem im vorherigen Schritt erstellten Schema.
2. Ziehen Sie ein paar Schema-Elemente auf das adaptive Formular.
3. Konfigurieren Sie die Übermittlungsaktion des adaptiven Formulars, um sie an AEM Workflow (MergeAcroformData) zu senden.
4. **Stellen Sie sicher, dass Sie den Datendateipfad als &quot;Data.xml&quot;angeben. Dies ist sehr wichtig, da der Beispielcode in der Workflow-Nutzlast nach einer Datei namens &quot;Data.xml&quot;sucht.**
5. Adaptives Formular für die Vorschau ausfüllen und senden.
6. Sie sollten die PDF-Datei mit den zusammengeführten Daten in dem in Schritt 4 angegebenen Ordner unter &quot;Arbeitsablauf für die Konfiguration&quot;anzeigen

>[!NOTE]
>
>Die durch Zusammenführen der Daten mit dem acroform generierte PDF-Datei wird als pdfdocument.pdf im Payload-Ordner des Workflows gespeichert. Dieses Dokument kann dann als Teil des Workflows für die weitere Verarbeitung verwendet werden
