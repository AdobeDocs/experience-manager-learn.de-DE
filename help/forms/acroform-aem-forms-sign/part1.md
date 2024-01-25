---
title: Acroforms mit AEM Forms
description: Teil 1 der Integration von Acroforms in AEM Forms. Erstellen eines adaptiven Formulars mit Acroform und Zusammenführen der Daten zum Abrufen einer PDF-Datei.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 100%

---


# Erstellen von Acroforms

Acroforms sind mit Acrobat erstellte Formulare. Sie können mit Acrobat ein neues Formular von Grund auf neu erstellen oder ein vorhandenes, in Microsoft Word erstelltes Formular verwenden und es mit Acrobat in ein Acroform konvertieren. Die folgenden Schritte müssen befolgt werden, um ein in Microsoft Word erstelltes Formular in ein Acroform zu konvertieren.

* Öffnen Sie ein Word-Dokument mit Acrobat.
* Verwenden Sie das Acrobat-Werkzeug „Formular vorbereiten“, um die Formularfelder im Formular zu identifizieren.
* Speichern Sie die PDF-Datei. Stellen Sie sicher, dass der Dateiname keine Leerzeichen enthält.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Wenn Sie das ausfüllbare Acroform zum Signieren mit Acrobat Sign senden möchten, benennen Sie die Felder entsprechend. Sie können beispielsweise ein Feld **Sig_es_:signer1:signature** nennen. Dies ist die Syntax, die Acrobat Sign versteht.

>[!NOTE]
>
>Wenn Sie ein XFA-basiertes Dokument senden, müssen Sie das Dokument reduzieren und die Signatur-Tags für Acrobat Sign müssen als statischer Text im Dokument vorhanden sein.

[Text-Tag-Dokument für Acrobat Sign](https://helpx.adobe.com/de/sign/using/text-tag.html)

>[!NOTE]
>
>Achten Sie darauf, dass der Acroform-Dateiname keine Leerzeichen enthält. Der aktuelle Beispiel-Code verarbeitet keine Leerzeichen.
>
>Die Formularfeldnamen dürfen nur Folgendes enthalten:
>
>* einzelnes Leerzeichen
>* einzelnen Unterstrich
>* alphanumerische Zeichen
