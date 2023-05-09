---
title: Erstellen einer Komponente zum Auflisten der Formulardaten
description: Tutorial zum Erstellen einer Zusammenfassungskomponente zum Überprüfen von Formulardaten vor der Übermittlung.
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# Erstellen Sie eine Komponente, um die Formulardaten zusammenzufassen.

Eine einfache Komponente wurde erstellt, um die Formulardaten zur Überprüfung aufzulisten. Die [Besuchsfunktion der guidebridge-API](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) wurde verwendet, um durch die Formularfelder zu navigieren. Der mit dieser Komponente verknüpfte Code in der Client-Bibliothek enthält die Bedienfeld-/Tabellenkomponenten des Formulars. Aus den untergeordneten Elementen dieser Bedienfeld-/Tabellenkomponenten werden die Formularfelder Titel, Wert und der SOM-Ausdruck mithilfe der GuidBridge-API-Methoden extrahiert. Eine einfache HTML-Tabelle wird dann mit dem Titel, dem Wert und dem SOM-Ausdruck erstellt, damit der Endbenutzer die Formulardaten vor dem Senden des Formulars überprüfen/bearbeiten kann.

Der folgende Screenshot zeigt beispielsweise die Tabelle, die erstellt wurde, um die Felder und deren Werte der **YourDetails**. Der letzte TD im TR wird verwendet, um den Feldwert mithilfe der Felder SOM-Ausdruck zu bearbeiten.

![visit-func](assets/visit-function.png)

## Nächste Schritte

[Testen Sie die Lösung auf Ihrem lokalen System.](./deploy-on-your-system.md)
