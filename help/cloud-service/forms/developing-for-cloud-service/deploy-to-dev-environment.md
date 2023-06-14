---
title: Bereitstellen für die Entwicklungsumgebung
description: Bereitstellen des Codes von Ihrer Cloud Manager-Repository-Verzweigung
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 8%

---

# Bereitstellen für die Entwicklungsumgebung

Im vorherigen Schritt haben wir unsere Übergeordnete Verzweigung von unserem lokalen Git-Repository in die MyFirstAF-Verzweigung des Cloud Manager-Repositorys verschoben.

Der nächste Schritt besteht darin, den Code in der Entwicklungsumgebung bereitzustellen.
Melden Sie sich bei Cloud Manager an und wählen Sie Ihr Programm aus.

Wählen Sie die Option In Entwicklung bereitstellen wie unten dargestellt aus.


![erster Schritt](assets/deploy-first-step1.png)


Wählen Sie Implementierungs-Pipeline wie folgt aus
![erster Schritt](assets/deploy1.png)

Quellcode und entsprechende Git-Verzweigung auswählen
![erster Schritt](assets/deploy2.png)
Aktualisieren Sie Ihre Änderungen

Pipeline ausführen
![run-pipeline](assets/run-pipeline.png)

Nachdem der Code bereitgestellt wurde, sollten die Änderungen in Ihrer Cloud Service-Instanz von AEM Forms angezeigt werden.

## Nächste Schritte

[Aktualisieren des Maven-Archetypprojekts](./updating-project-archetype.md)
