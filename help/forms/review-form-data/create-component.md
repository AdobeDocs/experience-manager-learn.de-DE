---
title: Erstellen einer Komponente zum Auflisten der Formulardaten
description: Tutorial zum Erstellen einer Zusammenfassungskomponente für die Überprüfung von Formulardaten vor der Übermittlung.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 33
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '169'
ht-degree: 100%

---

# Erstellen einer Komponente zum Zusammenfassen der Formulardaten

Eine einfache Komponente wurde erstellt, um die Formulardaten zwecks Überprüfung aufzulisten. Die [visit-Funktion der GuideBridge-API](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) wurde verwendet, um die Formularfelder zu durchlaufen. Der mit dieser Komponente verknüpfte Code in der Client-Bibliothek ruft die Panel-/Tabellenkomponenten des Formulars ab. Aus den untergeordneten Elementen dieser Panel-/Tabellenkomponenten werden Titel, Wert und SOM-Ausdruck der Formularfelder mithilfe der GuideBridge-API-Methoden extrahiert. Eine einfache HTML-Tabelle wird dann mit dem Titel, Wert und SOM-Ausdruck erstellt, damit die Endbenutzenden die Formulardaten vor der Übermittlung des Formulars überprüfen/bearbeiten können.

Der folgende Screenshot zeigt beispielsweise die Tabelle, die erstellt wurde, um die Felder und deren Werte für **YourDetails** aufzulisten. Das letzte TD-Element in TR wird verwendet, um den Feldwert mit dem SOM-Ausdruck der Felder zu bearbeiten.

![visit-func](assets/visit-function.png)

## Nächste Schritte

[Testen der Lösung auf Ihrem lokalen System](./deploy-on-your-system.md)
