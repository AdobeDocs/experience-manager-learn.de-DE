---
title: Installieren und Einrichten von Git
description: Lokales Git-Repository initialisieren
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Git installieren


[Git installieren](https://git-scm.com/downloads). Sie können die Standardeinstellungen auswählen und den Installationsprozess abschließen.
Wechseln Sie zur Eingabeaufforderung Navigieren Sie zu c:\cloudmanager\aem-banking-app type in git —version . Sie sollten die Version von GIT sehen, die auf Ihrem System installiert ist.

## Lokales Git-Repository initialisieren

Vergewissern Sie sich, dass Sie sich unter c:\cloudmanager\aem-banking-app folder befinden.

```
git init
```

Der obige Befehl initialisiert das Projekt als Git-lokales Repository.

```
git add .
```

Dadurch werden alle Projektdateien zum Git-Repository hinzugefügt, das zum Übertragen in das Git-Repository bereit ist.

```
git commit -m "initial commit"
```

Dadurch werden die Dateien in das Git-Repository übertragen.



## Registrieren des Cloud Manager-Repositorys für unser lokales Git-Repository

Zugriff auf das Cloud Manager-Repository
![Zugriff auf die Rep-Informationen](assets/cloud-manager-repo.png)
Rufen Sie die Repo-Anmeldeinformationen des Cloud Manager ab.
![get-credentials](assets/cloud-manager-repo1.png)

Speichern Sie den Benutzernamen in der Konfigurationsdatei.

```java
git config --global credential.username "gbedekar-adobe-com"
```

das Kennwort in der Konfigurationsdatei speichern

```java
git config --global user.password "bqwxfvxq2akawtqx3oztacb5wax5a7"
```

(Das Kennwort ist Ihr Git-Repository-Kennwort für den Cloud Manager.)

Registrieren Sie das Cloud Manager-Git-Repository bei Ihrem lokalen Git-Repository. Der folgende Befehl verknüpft **adobe** mit dem Remote-Git-Repository des Cloud Manager. Sie könnten einen beliebigen Namen anstelle von **adobe**


```java
git remote add adobe https://git.cloudmanager.adobe.com/techmarketingdemos/Program2-p24107/
```

(Stellen Sie sicher, dass Sie Ihre Repository-URL verwenden.)

Überprüfen Sie, ob das Remote-Repository registriert ist.

```java
git remote -v
```



