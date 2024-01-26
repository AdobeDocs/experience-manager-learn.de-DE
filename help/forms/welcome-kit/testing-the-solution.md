---
title: Testen der Begrüßungs-Kit-Lösung
description: Bereitstellen der Lösungs-Assets zum Testen der Lösung
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
duration: 36
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 100%

---

# Bereitstellen und Testen der Beispiel-Assets

[Installieren Sie das Begrüßungspaket](assets/welcomekit.zip). Dieses Paket enthält die Seitenvorlage, die benutzerdefinierte Komponente zum Auflisten der Assets, den Beispiel-Workflow, die E-Mail-Vorlage und die PDF-Musterdokumente, die in das Begrüßungs-Kit aufgenommen werden sollen.
[Installieren Sie das Bundle für das Begrüßungs-Kit](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Dieses Bundle enthält den Code zum Erstellen der Seite und die Java-Klasse zum Zurückgeben der auf der Web-Seite anzuzeigenden Assets.
[Installieren Sie das adaptive Beispielformular](assets/account-openeing-form.zip).
Konfigurieren Sie den Day CQ-Mail-Service. Der Workflow sendet den Link zum Begrüßungs-Kit an die Person, die das Formular einreicht, wofür das SMTP korrekt konfiguriert sein muss.
Konfigurieren Sie die Komponente „E-Mail senden“ des Workflows gemäß Ihren Anforderungen.
[Vorschau des Formulars](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Geben Sie Ihre Daten ein, wählen Sie einen oder mehrere Anlagefonds aus und senden Sie das Formular ab.
Sie sollten eine E-Mail mit einem Link zum Begrüßungs-Kit erhalten.
