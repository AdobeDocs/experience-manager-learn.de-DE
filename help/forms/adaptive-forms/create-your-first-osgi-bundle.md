---
title: Erstellen Ihres ersten OSGi-Bundles mit AEM Formularen
description: Erstellen Sie Ihr erstes OSGi-Bundle mit Maven und Eclipse
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 3%

---


# Erstellen des ersten OSGi-Bundles

Ein OSGi-Bundle ist eine Java™-Archivdatei, die Java-Code, Ressourcen und ein Manifest enthält, das das Bundle und seine Abhängigkeiten beschreibt. Das Bundle ist die Einheit der Bereitstellung für eine Anwendung. Dieser Artikel richtet sich an Entwickler, die einen OSGi-Dienst oder ein Servlet mit AEM Forms 6.4 oder 6.5 erstellen möchten. Um Ihr erstes OSGi-Bundle zu erstellen, gehen Sie wie folgt vor:


## JDK installieren

Installieren Sie die unterstützte Version von JDK. Ich habe JDK1.8 verwendet. Vergewissern Sie sich, dass Sie **JAVA_HOME** in Ihren Umgebungsvariablen hinzugefügt haben und auf den Stammordner Ihrer JDK-Installation verweisen.
Fügen Sie den Pfad %JAVA_HOME%/bin hinzu

![data-source](assets/java-home.JPG)

>[!NOTE]
> Verwenden Sie JDK 15 nicht. Sie wird von AEM nicht unterstützt.

### JDK-Version testen

Öffnen Sie ein neues Eingabeaufforderungsfenster und geben Sie Folgendes ein: `java -version`. Sie sollten die Version von JDK wiederherstellen, die durch die Variable `JAVA_HOME` identifiziert wird.

![data-source](assets/java-version.JPG)

## Maven installieren

Maven ist ein Tool zur Automatisierung von Builds, das hauptsächlich für Java-Projekte verwendet wird. Führen Sie die folgenden Schritte aus, um Maven auf Ihrem lokalen System zu installieren.

* Erstellen Sie einen Ordner mit dem Namen `maven` in Ihrem C-Laufwerk.
* Laden Sie das Archiv [binäre ZIP-Datei](http://maven.apache.org/download.cgi) herunter.
* Extrahieren Sie den Inhalt des ZIP-Archivs in `c:\maven`
* Erstellen Sie eine Umgebungsvariable mit dem Namen `M2_HOME` mit dem Wert `C:\maven\apache-maven-3.6.0`. In meinem Fall ist die **mvn**-Version 3.6.0. Zum Zeitpunkt der Erstellung dieses Artikels ist die neueste Maven-Version 3.6.3
* Fügen Sie `%M2_HOME%\bin` Ihrem Pfad hinzu.
* Speichern Sie Ihre Änderungen
* Öffnen Sie eine neue Eingabeaufforderung und geben Sie `mvn -version` ein. Die **mvn**-Version sollte aufgelistet sein, wie im Screenshot unten dargestellt

![data-source](assets/mvn-version.JPG)

## Settings.xml

Eine Maven-Datei `settings.xml` definiert Werte, die die Ausführung von Maven auf verschiedene Arten konfigurieren. In der Regel wird sie verwendet, um einen lokalen Repository-Speicherort, alternative Remote-Repository-Server und Authentifizierungsinformationen für private Repositorys zu definieren.

Navigieren Sie zu `C:\Users\<username>\.m2 folder` .
Extrahieren Sie den Inhalt der Datei [settings.zip](assets/settings.zip) und legen Sie sie im Ordner `.m2` ab.

## Installieren von Eclipse

Installieren Sie die neueste Version von [eclipse](https://www.eclipse.org/downloads/)

## Erstellen des ersten Projekts

Archetyp ist ein Maven-Projektvorlagen-Toolkit. Ein Archetyp ist definiert als ein ursprüngliches Muster oder Modell, aus dem alle anderen Elemente derselben Art hergestellt werden. Der Name passt zu dem, was wir versuchen, ein System bereitzustellen, das eine konsistente Möglichkeit zur Generierung von Maven-Projekten bietet. Archetyp hilft Autoren beim Erstellen von Maven-Projektvorlagen für Benutzer und bietet Benutzern die Möglichkeit, parametrierte Versionen dieser Projektvorlagen zu generieren.
Gehen Sie wie folgt vor, um Ihr erstes Maven-Projekt zu erstellen:

* Erstellen Sie einen neuen Ordner mit dem Namen `aemformsbundles` in Ihrem C-Laufwerk.
* Öffnen Sie eine Eingabeaufforderung und navigieren Sie zu `c:\aemformsbundles`
* Führen Sie in der Eingabeaufforderung den folgenden Befehl aus
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Das Maven-Projekt wird interaktiv generiert und Sie werden aufgefordert, Werte für eine Reihe von Eigenschaften bereitzustellen, z. B.

| Eigenschaftsname | Signifikanz | Wert |
------------------------|---------------------------------------|---------------------
| groupId | groupId identifiziert Ihr Projekt in allen Projekten eindeutig | com.learningaemforms.adobe |
| appsFolderName | Name des Ordners, der Ihre Projektstruktur enthalten wird | learningaemforms |
| artifactId | artifactId ist der Name der JAR-Datei ohne Version. Wenn Sie es erstellt haben, können Sie wählen, welchen Namen Sie mit Kleinbuchstaben und ohne seltsame Symbole wünschen. | learningaemforms |
| Version | Wenn Sie sie verteilen, können Sie eine beliebige typische Version mit Zahlen und Punkten auswählen (1.0, 1.1, 1.0.1, ...). | 1.0 |

Akzeptieren Sie die Standardwerte für die anderen Eigenschaften, indem Sie die Eingabetaste drücken.
Wenn alles gut läuft, sollten Sie in Ihrem Befehlsfenster eine Build-Erfolgsmeldung sehen

## Erstellen eines Eclipse-Projekts aus Ihrem Maven-Projekt

Ändern Sie Ihr Arbeitsverzeichnis in `learningaemforms`.
Ausführen von `mvn eclipse:eclipse` über die Befehlszeile
Der obige Befehl liest Ihre Pom-Datei und erstellt Eclipse-Projekte mit korrekten Metadaten, sodass Eclipse die Projektarten, Beziehungen, Klassenpfad usw. versteht.

## Importieren des Projekts in Eclipse

Starten Sie **Eclipse**

Gehen Sie zu **Datei -> Import** und wählen Sie **Vorhandene Maven-Projekte** aus, wie hier dargestellt

![data-source](assets/import-mvn-project.JPG)

Klicken Sie auf Weiter

Wählen Sie die `c:\aemformsbundles\learningaemform`s aus, indem Sie auf die Schaltfläche **Durchsuchen** klicken

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>Sie können je nach Bedarf die gewünschten Module importieren. Wählen Sie nur das Kernmodul aus und importieren Sie es, wenn Sie nur Java-Code in Ihrem Projekt erstellen möchten.

Klicken Sie auf **Beenden** , um den Importprozess zu starten.

Das Projekt wird in Eclipse importiert und es wird eine Reihe von `learningaemforms.xxxx` Ordnern angezeigt

Erweitern Sie `src/main/java` unter dem Ordner `learningaemforms.core` . Dies ist der Ordner, in den Sie den Großteil Ihres Codes schreiben werden.

![data-source](assets/learning-core.JPG)

## Projekt erstellen

Nachdem Sie Ihren OSGi-Dienst oder Servlet geschrieben haben, müssen Sie Ihr Projekt erstellen, um das OSGi-Bundle zu generieren, das mithilfe der Felix-Webkonsole bereitgestellt werden kann. Bitte lesen Sie [AEMFD Client SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) , um das entsprechende Client-SDK in Ihr Maven-Projekt aufzunehmen. Sie müssen das AEM FD Client SDK wie unten dargestellt in den Abhängigkeitsabschnitt von `pom.xml` des Kernprojekts einbeziehen.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Gehen Sie wie folgt vor, um Ihr Projekt zu erstellen:

* Öffnen Sie **das Befehlszeilenfenster**
* Navigieren Sie zu `c:\aemformsbundles\learningaemforms\core`
* Führen Sie den Befehl `mvn clean install` aus.
Wenn alles gut geht, sollte das Bundle an der folgenden Stelle `C:\AEMFormsBundles\learningaemforms\core\target` angezeigt werden. Dieses Bundle kann jetzt mithilfe der Felix-Webkonsole in AEM bereitgestellt werden.
