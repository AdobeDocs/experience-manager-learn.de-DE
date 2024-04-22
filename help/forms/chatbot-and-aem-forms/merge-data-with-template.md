---
title: Zusammenführen von Daten mit der XDP-Vorlage
description: Erstellen eines PDF-Dokuments durch Zusammenführen von Daten mit der Vorlage
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 15%

---

# Zusammenführen von Daten mit der XDP-Vorlage

Der nächste Schritt besteht darin, die XML-Daten mit der Vorlage zusammenzuführen, um die PDF zu generieren. Diese PDF wird dann zur Signatur mit Adobe Sign gesendet.

## Verwenden von OutputService zum Generieren der PDF

Die [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) -Methode des OutputService zum Generieren der PDF verwendet wurde.
Die generierte PDF wurde dann mithilfe der Adobe Sign REST API zur Signatur gesendet.

