---
title: Acroforms mit AEM Forms
seo-title: Zusammenführen adaptiver Formulardaten mit Acroform
description: Teil 1 der Integration von Acroforms mit AEM Forms. Erstellen eines adaptiven Formulars mit Acroform und Zusammenführen der Daten zum Abrufen einer PDF-Datei.
feature: adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '226'
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
>Wenn Sie das ausfüllbare Acrobat zum Signieren mit Adobe Sign senden möchten, benennen Sie die Felder entsprechend. Sie können beispielsweise ein Feld **Sig_es_:signer1:signature** benennen. Dies ist die Syntax, die Adobe Sign versteht.

>[!NOTE]
>
>Wenn Sie ein XFA-basiertes Dokument senden, müssen Sie das Dokument reduzieren und die Adobe Sign-Signatur-Tags müssen als statischer Text im Dokument vorhanden sein.

[Adobe Sign Text Tags-Dokument](https://helpx.adobe.com/sign/using/text-tag.html_de)

>[!NOTE]
>
>Achten Sie darauf, dass der Dateiname des Formulars keine Leerzeichen enthält. Der aktuelle Beispielcode behandelt keine Leerzeichen.
>
>Die Formularfeldnamen dürfen nur Folgendes enthalten:
>
>* Einzelraum
>* einzelner Unterstrich
>* alphanumerische Zeichen

