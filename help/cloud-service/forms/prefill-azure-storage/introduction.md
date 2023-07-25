---
title: Implementieren der Funktion zum Speichern und Fortsetzen für ein adaptives Formular
description: Erfahren Sie, wie Sie adaptive Formulardaten aus dem Azure-Speicherkonto speichern und abrufen.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 3%

---

# Einführung

In diesem Tutorial implementieren wir einen einfachen Anwendungsfall, der es den Formularausfüllern ermöglicht, den Formularausfüllvorgang zu speichern und fortzusetzen. Der Ablauf des Anwendungsfalls sieht wie folgt aus:

* Benutzer beginnt mit dem Ausfüllen eines adaptiven Formulars.
* Der Benutzer speichert das Formular und möchte das Ausfüllen des Formulars zu einem späteren Zeitpunkt fortsetzen.
* Der Benutzer erhält eine E-Mail-Benachrichtigung mit einem Link zum gespeicherten Formular.

## Voraussetzungen

* Erlebnis mit AEM Forms CS, insbesondere Erstellen von Formulardatenmodellen.
* Erfahrung bei der Bereitstellung von Code mithilfe von Cloud Manager.
* Zugriff auf Cloud-fähige Instanz von AEM Forms CS.

Um das oben genannte Anwendungsbeispiel in AEM Forms CS zu implementieren, benötigen Sie Folgendes

* [AEM Forms CS-Cloud-fähige Instanz](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Azure Portal-Konto](https://portal.azure.com/)
* [SendGrid-Konto](https://sendgrid.com/)

### Nächste Schritte

[Seitenkomponente erstellen](./page-component.md)


