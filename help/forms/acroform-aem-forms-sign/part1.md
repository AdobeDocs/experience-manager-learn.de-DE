---
title: Acroforms mit AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Teil 1 der Integration von Acroforms mit AEM Forms. Erstellen eines adaptiven Formulars mit Acroform und Zusammenführen der Daten, um eine PDF zu erhalten.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 3%

---


# Acroform erstellen

Acroforms sind Formulare, die mit Acrobat erstellt werden. Sie können ein neues Formular mit Acrobat von Grund auf neu erstellen oder ein in Microsoft Word erstelltes vorhandenes Formular verwenden und es mithilfe von Acrobat in Acroform konvertieren. Die folgenden Schritte müssen befolgt werden, um ein in Microsoft Word erstelltes Formular in Acroform zu konvertieren.

* Öffnen von Word-Dokumenten mit Acrobat
* Verwenden Sie das Acrobat Prepare-Formular-Tool, um die Formularfelder im Formular zu identifizieren.
* Speichern Sie die PDF. Stellen Sie sicher, dass der Dateiname keine Leerzeichen enthält.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Wenn Sie das ausfüllbare Acrobat zum Signieren mit Acrobat Sign senden möchten, benennen Sie die Felder entsprechend. Sie können beispielsweise ein Feld benennen **Sig_es_:signer1:signature**. Dies ist die Syntax, die Acrobat Sign versteht.

>[!NOTE]
>
>Wenn Sie ein XFA-basiertes Dokument senden, müssen Sie das Dokument reduzieren und die Acrobat Sign-Signatur-Tags müssen als statischer Text im Dokument vorhanden sein.

[Acrobat Sign Text Tags-Dokument](https://helpx.adobe.com/de/sign/using/text-tag.html)

>[!NOTE]
>
>Achten Sie darauf, dass der Dateiname des Formulars keine Leerzeichen enthält. Der aktuelle Beispielcode behandelt keine Leerzeichen.
>
>Die Formularfeldnamen dürfen nur Folgendes enthalten:
>
>* Einzelraum
>* einzelner Unterstrich
>* alphanumerische Zeichen

