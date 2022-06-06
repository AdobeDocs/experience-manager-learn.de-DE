---
title: Erstellen Ihres ersten OSGi-Bundles mit AEM Forms
description: Erstellen Sie Ihr erstes OSGi-Bundle mit Maven und Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '674'
ht-degree: 3%

---

# Erstellen des ersten OSGi-Bundles

Ein OSGi-Bundle ist eine Java™-Archivdatei, die Java-Code, Ressourcen und ein Manifest enthält, das das Bundle und seine Abhängigkeiten beschreibt. Das Bundle ist die Einheit der Bereitstellung für eine Anwendung. Dieser Artikel richtet sich an Entwickler, die einen OSGi-Dienst oder ein Servlet mit AEM Forms 6.4 oder 6.5 erstellen möchten. Um Ihr erstes OSGi-Bundle zu erstellen, gehen Sie wie folgt vor:


## JDK installieren

Installieren Sie die unterstützte Version von JDK. Ich habe JDK1.8 verwendet. Stellen Sie sicher, dass Sie hinzugefügt haben. **JAVA_HOME** in Ihren Umgebungsvariablen und verweist auf den Stammordner Ihrer JDK-Installation.
Fügen Sie den Pfad %JAVA_HOME%/bin hinzu

![data-source](assets/java-home.JPG)

>[!NOTE]
> Verwenden Sie JDK 15 nicht. Sie wird von AEM nicht unterstützt.

### JDK-Version testen

Öffnen Sie ein neues Eingabeaufforderungsfenster und geben Sie Folgendes ein: `java -version`. Sie sollten die JDK-Version wiederherstellen, die durch die `JAVA_HOME` Variable

![data-source](assets/java-version.JPG)

## Maven installieren

Maven ist ein Tool zur Automatisierung von Builds, das hauptsächlich für Java-Projekte verwendet wird. Führen Sie die folgenden Schritte aus, um Maven auf Ihrem lokalen System zu installieren.

* Erstellen Sie einen Ordner mit dem Namen `maven` im C-Laufwerk
* Laden Sie die [binäres ZIP-Archiv](https://maven.apache.org/download.cgi)
* Extrahieren Sie den Inhalt des ZIP-Archivs in `c:\maven`
* Umgebungsvariable mit dem Namen `M2_HOME` mit dem Wert `C:\maven\apache-maven-3.6.0`. In meinem Fall wird die **mvn** -Version ist 3.6.0. Zum Zeitpunkt der Erstellung dieses Artikels ist die neueste Maven-Version 3.6.3
* Fügen Sie die `%M2_HOME%\bin` zu Ihrem Pfad
* Speichern Sie Ihre Änderungen
* Öffnen Sie eine neue Eingabeaufforderung und geben Sie ein in `mvn -version`. Sie sollten die **mvn** -Version aufgeführt, wie im Screenshot unten dargestellt

![data-source](assets/mvn-version.JPG)


## Installieren von Eclipse

Installieren Sie die neueste Version von [Eclipse](https://www.eclipse.org/downloads/)

## Erstellen des ersten Projekts

Archetyp ist ein Maven-Projektvorlagen-Toolkit. Ein Archetyp ist definiert als ein ursprüngliches Muster oder Modell, aus dem alle anderen Elemente derselben Art hergestellt werden. Der Name passt zu dem, was wir versuchen, ein System bereitzustellen, das eine konsistente Möglichkeit zur Generierung von Maven-Projekten bietet. Archetyp hilft Autoren beim Erstellen von Maven-Projektvorlagen für Benutzer und bietet Benutzern die Möglichkeit, parametrierte Versionen dieser Projektvorlagen zu generieren.
Gehen Sie wie folgt vor, um Ihr erstes Maven-Projekt zu erstellen:

* Erstellen Sie einen neuen Ordner mit dem Namen `aemformsbundles` im C-Laufwerk
* Öffnen Sie eine Eingabeaufforderung und navigieren Sie zu `c:\aemformsbundles`
* Führen Sie in der Eingabeaufforderung den folgenden Befehl aus

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Nach erfolgreichem Abschluss sollte in Ihrem Befehlsfenster eine Build-Erfolgsmeldung angezeigt werden.

## Erstellen eines Eclipse-Projekts aus Ihrem Maven-Projekt

* Ändern Sie das Arbeitsverzeichnis in `mysite`
* Ausführen `mvn eclipse:eclipse` über die Befehlszeile. Der Befehl liest Ihre Pom-Datei und erstellt Eclipse-Projekte mit korrekten Metadaten, sodass Eclipse die Projektarten, Beziehungen, Klassenpfad usw. versteht.

## Importieren des Projekts in Eclipse

Launch **Eclipse**

Navigieren Sie zu **Datei -> Importieren** und wählen Sie **Bestehende Maven-Projekte** wie hier gezeigt

![data-source](assets/import-mvn-project.JPG)

Klicken Sie auf Weiter

Wählen Sie c:\aemformsbundles\mysite by clicking the aus. **Durchsuchen** button

![data-source](assets/mysite-eclipse-project.png)

>[!NOTE]
>Sie können je nach Bedarf die gewünschten Module importieren. Wählen Sie nur das Kernmodul aus und importieren Sie es, wenn Sie nur Java-Code in Ihrem Projekt erstellen möchten.

Klicken **Beenden** um den Importvorgang zu starten

Das Projekt wird in Eclipse importiert und es wird eine Reihe von `mysite.xxxx` Ordner

Erweitern Sie die `src/main/java` unter `mysite.core` Ordner. Dies ist der Ordner, in den Sie den Großteil Ihres Codes schreiben werden.

![data-source](assets/mysite-core-project.png)

## AEMFD Client SDK einschließen

Sie müssen das AEMFD-Client-SDK in Ihr Projekt einbeziehen, um verschiedene Dienste zu nutzen, die mit AEM Forms bereitgestellt werden. Siehe [AEMFD Client SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) , um das entsprechende Client-SDK in Ihr Maven-Projekt aufzunehmen. Sie müssen das AEM FD Client SDK in den Abhängigkeitsabschnitt von `pom.xml` des Kernprojekts, wie unten dargestellt.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Gehen Sie wie folgt vor, um Ihr Projekt zu erstellen:

* Öffnen **Eingabeaufforderungsfenster**
* Navigieren Sie zu `c:\aemformsbundles\mysite\core`
* Ausführen des Befehls `mvn clean install -PautoInstallBundle`
Der obige Befehl erstellt und installiert das Bundle auf dem AEM Server, auf dem ausgeführt wird `http://localhost:4502`. Das Bundle ist auch im Dateisystem unter
   `C:\AEMFormsBundles\mysite\core\target` und können mithilfe von bereitgestellt werden [Felix-Webkonsole](http://localhost:4502/system/console/bundles)
