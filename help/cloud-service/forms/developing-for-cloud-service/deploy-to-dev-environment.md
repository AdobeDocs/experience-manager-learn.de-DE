---
title: Bereitstellen für die Entwicklungsumgebung
description: Bereitstellen des Codes von Ihrer Cloud Manager-Repository-Verzweigung
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '118'
ht-degree: 100%

---

# Bereitstellen für die Entwicklungsumgebung

Im vorherigen Schritt haben wir unsere übergeordnete Verzweigung von unserem lokalen Git-Repository in die MyFirstAF-Verzweigung des Cloud Manager-Repositorys verschoben.

Der nächste Schritt besteht darin, den Code in der Entwicklungsumgebung bereitzustellen.
Melden Sie sich bei Cloud Manager an und wählen Sie Ihr Programm aus.

Wählen Sie die Option „Für Entwicklung bereitstellen“ wie unten dargestellt aus.


![first-step](assets/deploy-first-step1.png)


Wählen Sie Bereitstellungs-Pipeline wie folgt aus:
![first-step](assets/deploy1.png)

Wählen Sie den Quell-Code und entsprechende Git-Verzweigung aus:
![first-step](assets/deploy2.png)
Aktualisieren Sie Ihre Änderungen.

Führen Sie die Pipeline aus:
![run-pipeline](assets/run-pipeline.png)

Nachdem der Code bereitgestellt wurde, sollten die Änderungen in Ihrer Cloud Service-Instanz von AEM Forms angezeigt werden.

## Nächste Schritte

[Aktualisieren des Maven-Archetyp-Projekts](./updating-project-archetype.md)
