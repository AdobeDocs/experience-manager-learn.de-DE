---
title: Verwenden des Stilsystems in AEM Forms
description: Definieren der Stilrichtlinie für die Schaltflächenkomponente
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 52205a93-d03c-430c-a707-b351ab333939
source-git-commit: e764e573fba00a3b12bc60ec7dcc11e42ab96ed2
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 100%

---

# Definieren des Stils in der Richtlinie für die Komponente

* Melden Sie sich bei Ihrer lokalen Cloud-fähigen AEM-Instanz an und navigieren Sie zu Tools | Allgemein | Vorlagen | Ihr Projektname.

* Wählen Sie die Vorlage **Leer mit Kernkomponenten** aus und öffnen Sie sie im Bearbeitungsmodus.
* Klicken Sie auf das Richtliniensymbol der Schaltflächenkomponente, um den Richtlinieneditor zu öffnen.

* ![button-policy](assets/button-policy.png)

Definieren Sie die Richtlinie wie unten dargestellt.

![button-policy-details](assets/styling-policy.png)

Wir haben zwei Stile/Varianten mit den Bezeichnungen „Marketing“ und „Unternehmen“ definiert. Diese Varianten sind mit entsprechenden CSS-Klassen verknüpft.**Stellen Sie sicher, dass vor und nach den CSS-Klassennamen kein Leerzeichen vorhanden ist**.
Speichern Sie Ihre Änderungen.

| Stil | CSS-Klasse |
|-----------|------------------------------------|
| Marketing | cmp-adaptiveform-button--marketing |
| Unternehmen | cmp-adaptiveform-button--corporate |

Diese CSS-Klassen werden in der SCSS-Datei der Komponente (_button.scss) definiert.

## Nächste Schritte

[Definieren von CSS-Klassen](./create-variations.md)
