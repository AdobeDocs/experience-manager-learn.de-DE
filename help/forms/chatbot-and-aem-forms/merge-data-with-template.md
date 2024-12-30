---
title: Fusionieren von Daten mit der XDP-Vorlage
description: Erstellen eines PDF-Dokuments durch Fusionieren von Daten mit der Vorlage
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 100%

---

# Fusionieren von Daten mit der XDP-Vorlage

Der nächste Schritt besteht darin, die XML-Daten mit der Vorlage zu fusionieren, um die PDF zu generieren. Diese PDF wird dann zur Signatur mit Adobe Sign gesendet.

## Verwenden von OutputService zum Generieren der PDF

Die Methode [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html?lang=de#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) des OutputService wurde zum Generieren der PDF verwendet.
Die generierte PDF wurde dann zur Signatur mit der REST-API von Adobe Sign gesendet.
