---
title: Verwenden des Stilsystems in AEM Forms
description: Erstellen von Stilvarianten für die Schaltflächenkomponente
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: ht
source-wordcount: '231'
ht-degree: 100%

---

# Einführung

Mit dem Stilsystem in Adobe Experience Manager (AEM) können Benutzende mehrere visuelle Varianten einer Komponente erstellen und dann auswählen, welcher Stil beim Erstellen eines Formulars verwendet werden soll. Dadurch werden Komponenten flexibler und wiederverwendbarer, ohne dass für jeden Stil benutzerdefinierte Komponenten erstellt werden müssen.

Dieser Artikel hilft Ihnen beim Erstellen von Varianten der Schaltflächenkomponente und beim Testen der Varianten in Ihrer lokalen Cloud-fähigen Umgebung, bevor Sie die Änderungen mithilfe von Cloud Manager in die Cloud-Instanz übertragen.

Der Screenshot zeigt die zwei Stilvarianten für die Schaltflächenkomponente, die der Erstellerin bzw. dem Ersteller des Formulars zur Verfügung stehen.


![button-variations](assets/button-variations.png)

## Voraussetzungen

* Cloud-fähige Instanz mit Kernkomponenten in AEM Forms
* Klonen eines Designs: Sie müssen mit dem Klonen eines Designs vertraut sein. Für diese Anleitung haben wir das [easel-Design](https://github.com/adobe/aem-forms-theme-easel) geklont. Sie können alle verfügbaren Designs nach Ihren Bedürfnissen klonen.

* Installieren Sie die neueste Version von Apache Maven. Apache Maven ist ein Tool zur Automatisierung von Builds, das häufig für Java™-Projekte verwendet wird. Durch die Installation der neuesten Version stellen Sie sicher, dass Sie über die erforderlichen Abhängigkeiten für die Design-Anpassung verfügen.
* Installieren Sie einen Nur-Text-Editor. Beispielsweise Microsoft® Visual Studio Code. Die Verwendung eines Texteditors wie Microsoft® Visual Studio Code bietet eine benutzerfreundliche Umgebung zum Bearbeiten und Ändern von Design-Dateien.



## Nächste Schritte

[Erstellen einer Stilrichtlinie](./style-policy.md)