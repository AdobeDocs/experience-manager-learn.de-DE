---
title: Implementieren der Funktion zum Speichern und Fortsetzen für ein adaptives Formular
description: Erfahren Sie, wie Sie adaptive Formulardaten aus dem Azure Storage-Konto speichern und abrufen.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '157'
ht-degree: 100%

---

# Einführung

In diesem Tutorial implementieren wir einen einfachen Anwendungsfall, der es den Personen, die ein Formular ausfüllen, ermöglicht, den Formularausfüllvorgang zu speichern und fortzusetzen. Der Ablauf des Anwendungsfalls sieht wie folgt aus

* Die Person beginnt mit dem Ausfüllen eines adaptiven Formulars.
* Sie speichert das Formular und möchte das Formular zu einem späteren Zeitpunkt ausfüllen.
* Sie erhält eine E-Mail-Benachrichtigung mit einem Link zum gespeicherten Formular.

## Voraussetzungen

* Erlebnis mit AEM Forms CS, insbesondere beim Erstellen von Formulardatenmodellen.
* Erlebnis bei der Bereitstellung von Code mithilfe von Cloud Manager.
* Zugriff auf Cloud-fähige Instanz von AEM Forms CS.

Zum Implementieren des oben genannten Anwendungsfalls in AEM Forms CS benötigen Sie Folgendes

* [AEM Forms CS-Cloud-fähige Instanz](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=de#set-up-aem-author-instance)
* [Azure Portal-Konto](https://portal.azure.com/)
* [SendGrid-Konto](https://sendgrid.com/)

### Nächste Schritte

[Seitenkomponente erstellen](./page-component.md)
