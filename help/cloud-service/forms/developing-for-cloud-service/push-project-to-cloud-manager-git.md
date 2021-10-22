---
title: AEM Projekt in das Cloud Manager-Repository pushen
description: Lokales Git-Repository in das Cloud Manager-Repository pushen
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---


# Push AEM Projekt an Cloud Manager Git Repo

Im vorherigen Schritt haben wir unser AEM-Projekt mit der adaptiven Forms und in der AEM erstellten Designs synchronisiert.
Jetzt müssen wir diese Änderungen zu unserem lokalen Git-Repository hinzufügen und dann das lokale Git-Repository in das Git-Repository von Cloud Manager übertragen.
Öffnen Sie die Eingabeaufforderung und navigieren Sie zu c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .**
```

Dadurch werden die neuen Dateien zur Staging-Verzweigung des lokalen Git-Repositorys hinzugefügt.

```
git commit -m "My First AF"
```

Dadurch werden die Dateien in die Übergeordnete Verzweigung unseres lokalen Git-Repositorys übertragen.

```
git push -f bankingapp master:"MyFirstAF"
```

Im obigen Befehl schieben wir unsere Übergeordnete Verzweigung von unserem lokalen Git-Repository in die MyFirstAF-Verzweigung des Cloud Manager-Repositorys, der durch den benutzerfreundlichen Namen der Bankingapp identifiziert wird.



