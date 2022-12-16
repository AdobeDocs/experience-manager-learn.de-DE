---
title: Testen der Willkommenspaket-Lösung
description: Bereitstellen der Lösungs-Assets zum Testen der Lösung
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Bereitstellen und Testen der Beispiel-Assets

[Installieren des Willkommenspakets](assets/welcomekit.zip). Dieses Paket enthält die Seitenvorlage, die benutzerdefinierte Komponente zum Auflisten der Assets, den Beispiel-Workflow, die E-Mail-Vorlage und die PDF-Musterdokumente, die in das Begrüßungs-Kit aufgenommen werden sollen.
[Installieren Sie das Paket &quot;welcome-kit&quot;](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Dieses Bundle enthält den Code zum Erstellen der Seite und die Java-Klasse zum Zurückgeben der Assets, die auf der Webseite angezeigt werden sollen.
[Installieren des adaptiven Beispielformulars](assets/account-openeing-form.zip)
Konfigurieren Sie den Day CQ Mail Service. Der Workflow sendet den Begrüßungs-Kit-Link an den Formular-Übermittler, der ordnungsgemäß SMTP konfiguriert werden muss.
Konfigurieren Sie die Komponente E-Mail senden des Workflows gemäß Ihren Anforderungen.
[Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Geben Sie Ihre Details ein und wählen Sie einen oder mehrere Investmentfonds aus und senden Sie das Formular. Sie sollten eine E-Mail mit einem Link zum Begrüßungs-Kit erhalten


