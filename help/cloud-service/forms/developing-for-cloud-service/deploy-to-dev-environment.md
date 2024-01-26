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
jira: KT-8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
duration: 31
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
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
