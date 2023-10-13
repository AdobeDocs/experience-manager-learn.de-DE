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
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 3%

---

# Einführung

In diesem Tutorial implementieren wir einen einfachen Anwendungsfall, der es den Formularausfüllern ermöglicht, den Formularausfüllvorgang zu speichern und fortzusetzen. Der Ablauf des Anwendungsfalls sieht wie folgt aus:

* Benutzer beginnt mit dem Ausfüllen eines adaptiven Formulars.
* Der Benutzer speichert das Formular und möchte das Formular zu einem späteren Zeitpunkt ausfüllen.
* Der Benutzer erhält eine E-Mail-Benachrichtigung mit einem Link zum gespeicherten Formular.

## Voraussetzungen

* Erlebnis mit AEM Forms CS, insbesondere Erstellen von Formulardatenmodellen.
* Erfahrung bei der Bereitstellung von Code mithilfe von Cloud Manager.
* Zugriff auf Cloud-fähige Instanz von AEM Forms CS.

Zur Implementierung des oben genannten Anwendungsfalls in AEM Forms CS benötigen Sie Folgendes

* [AEM Forms CS-Cloud-fähige Instanz](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Azure Portal-Konto](https://portal.azure.com/)
* [SendGrid-Konto](https://sendgrid.com/)

### Nächste Schritte

[Seitenkomponente erstellen](./page-component.md)
