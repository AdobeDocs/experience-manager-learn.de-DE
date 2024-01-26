---
title: Installieren und Einrichten von Git
description: Initialisieren Ihres lokalen Git-Repositorys
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 59
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 100%

---

# Installieren von Git


[Installieren von Git](https://git-scm.com/downloads). Sie können die Standardeinstellungen auswählen und den Installationsprozess abschließen.
Wechseln Sie zur Eingabeaufforderung
Navigieren Sie zu c:\cloudmanager\aem-banking-app
geben Sie „git -- version“ ein. Sie sollten die Version von GIT sehen, die auf Ihrem System installiert ist

## Initialisieren des lokalen Git-Repositorys

Stellen Sie sicher, dass Sie sich im Ordner c:\cloudmanager\aem-banking-app befinden.

```
git init
```

Der obige Befehl initialisiert das Projekt als lokales Git-Repository.

```
git add .
```

Dadurch werden alle Projektdateien zum Git-Repository hinzugefügt, die zum Übertragen in das Git-Repository bereit sind

```
git commit -m "initial commit"
```

Dadurch werden die Dateien in das Git-Repository übertragen.



## Registrieren des Cloud Manager-Repositorys in unserem lokalen Git-Repository

Zugriff auf das Cloud Manager-Repository
![Zugriff auf die Repository-Informationen](assets/cloud-manager-repo.png)
Aufrufen der Repository-Anmeldeinformationen von Cloud Manager
![get-credentials](assets/cloud-manager-repo1.png)

Speichern des Benutzernamen in der Konfigurationsdatei

```java
git config --global credential.username "gbedekar-adobe-com"
```

Speichern des Kennworts in der Konfigurationsdatei

```java
git config --global user.password "XXXX"
```

(Das Kennwort ist Ihr Git-Repository-Passwort für Cloud Manager.)

Registrieren Sie das Cloud Manager-Git-Repository bei Ihrem lokalen Git-Repository. Der folgende Befehl verknüpft **bankingapp** mit dem Remote-Git-Repository von Cloud Manager. Sie könnten einen beliebigen Namen anstelle von **bankingapp** nutzen.


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Stellen Sie sicher, dass Sie Ihre Repository-URL verwenden.)

Überprüfen Sie, ob das Remote-Repository registriert ist

```java
git remote -v
```

## Nächste Schritte

[Synchronisieren von AEM mit AEM-Projekt in IntelliJ](./intellij-and-aem-sync.md)
