---
title: Pushen von AEM-Projekten in das Cloud Manager-Repository
description: Pushen von lokalen Git-Repositorys in das Cloud Manager-Repository
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 100%

---

# Pushen von AEM-Projekten in das Cloud Manager-Git-Repo

Im vorherigen Schritt haben wir unser AEM-Projekt mit den in der AEM-Instanz erstellten adaptiven Formularen und Designs synchronisiert.
Jetzt müssen wir diese Änderungen zu unserem lokalen Git-Repository hinzufügen und dann das lokale Git-Repository in das Git-Repository von Cloud Manager übertragen.
Öffnen Sie die Eingabeaufforderung und navigieren Sie zu „c:\cloudmanager\aem-banking-app“
Führen Sie die folgenden Befehle aus:

```
git add .
```

Dadurch werden die neuen Dateien zur Staging-Verzweigung des lokalen Git-Repositorys hinzugefügt.

```
git commit -m "My First AF"
```

Dadurch werden die Dateien in die Master-Verzweigung unseres lokalen Git-Repositorys übertragen.

```
git push -f bankingapp master:"MyFirstAF"
```

Im obigen Befehl schieben wir unsere Master-Verzweigung von unserem lokalen Git-Repository in die MyFirstAF-Verzweigung des Cloud Manager-Repositorys, das durch den Anzeigenamen der Bankingapp identifiziert wird.

## Nächste Schritte

[Bereitstellen des Projekts in der Entwicklungsumgebung](./deploy-to-dev-environment.md)
