---
title: Einschließen von Drittanbieter-JARs
description: Erfahren Sie, wie Sie JAR-Dateien von Drittanbietern in Ihrem AEM-Projekt verwenden
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 61
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 100%

---

# Einbinden von Drittanbieter-Bundles in Ihr AEM-Projekt

In diesem Artikel werden wir die Schritte durchgehen, die erforderlich sind, um ein OSGi-Bundle von Drittanbietern in Ihr AEM-Projekt aufzunehmen. Für die Zwecke dieses Artikels werden wir die [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) in unser AEM-Projekt einbeziehen. Wenn das OSGi im Maven-Repository verfügbar ist, fügen Sie die Abhängigkeit des Bundles in die POM.xml-Datei des Projekts ein.

>[!NOTE]
> Es wird angenommen, dass die JAR-Datei eines Drittanbieters ein OSGi-Bundle ist

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Wenn sich Ihr OSGi-Bundle auf Ihrem Dateisystem befindet, erstellen Sie einen Ordner mit dem Namen **localjar** im Basisverzeichnis Ihres Projekts (C:\aemformsbundles\AEMFormsProcessStep\localjar). Die Abhängigkeit sieht etwa so aus:

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Erstellen der Ordnerstruktur

Wir fügen dieses Bundle zu unserem AEM-Projekt **AEMFormsProcessStep** hinzu, das sich im Ordner **c:\aemformsbundles** befindet

* Öffnen Sie die Datei **filter.xml** im Ordner C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault Ihres Projekts
Notieren Sie sich das Stammattribut des Filterelements.

* Erstellen Sie die folgende Ordnerstruktur C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* Das **apps/AEMFormsProcessStep-vendor-packages** ist der Stammattributwert in der filter.xml
* Aktualisieren Sie den Abschnitt „Abhängigkeiten“ der POM.xml des Projekts.
* Öffnen Sie eine Eingabeaufforderung. Navigieren Sie zum Ordner Ihres Projekts (in meinem Fall C:\aemformsbundles\AEMFormsProcessStep). Führen Sie den folgenden Befehl aus

```java
mvn clean install -PautoInstallSinglePackage
```

Wenn alles gut geht, wird das Paket zusammen mit dem Drittanbieter-Bundle in Ihrer AEM-Instanz installiert. Sie können das Bundle mithilfe der [Felix-Web-Konsole](http://localhost:4502/system/console/bundles) prüfen. Das Drittanbieter-Bundle ist im Ordner /apps des `crx`-Repositorys wie unten dargestellt verfügbar
![third-party](assets/custom-bundle1.png)
