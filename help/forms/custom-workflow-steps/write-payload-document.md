---
title: Schreiben des Payload-Dokuments in das Dateisystem
description: Benutzerdefinierter Prozessschritt zum Hinzufügen eines Schreibdokuments zum Dateisystem, das sich unter dem Payload-Ordner befindet
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
duration: 33
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '140'
ht-degree: 100%

---

# Schreiben des Dokuments in das Dateisystem

Ein üblicher Anwendungsfall besteht darin, die generierten Dokumente im Workflow in das Dateisystem zu schreiben.
Dieser benutzerdefinierte Workflow-Prozessschritt erleichtert das Schreiben der Workflow-Dokumente in das Dateisystem.
Der benutzerdefinierte Prozess akzeptiert die folgenden kommagetrennten Argumente

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Das erste Argument ist der Name des Dokuments, das Sie im Dateisystem speichern möchten. Das zweite Argument ist der Speicherort des Ordners, in dem Sie das Dokument speichern möchten. Im obigen Anwendungsfall wird das Dokument beispielsweise in `c:\confirmation\ChangeBeneficiary.pdf` geschrieben

Der folgende Screenshot zeigt die Argumente, die Sie an den benutzerdefinierten Prozessschritt übergeben müssen
![write-payload-file-system](assets/write-payload-file-system.png)

[Das benutzerdefinierte Bundle kann von hier aus heruntergeladen werden:](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
