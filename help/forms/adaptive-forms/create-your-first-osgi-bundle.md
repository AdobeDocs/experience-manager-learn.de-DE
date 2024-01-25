---
title: Erstellen Ihres ersten OSGi-Bundles mit AEM Forms
description: Erstellen Ihres ersten OSGi-Bundles mit Maven und Eclipse
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
duration: 203
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 100%

---


# Erstellen Ihres ersten OSGi-Bundles

Ein OSGi-Bundle ist eine Java™-Archivdatei, die Java-Code, Ressourcen und ein Manifest enthält, das das Paket und seine Abhängigkeiten beschreibt. Bei diesem Bundle handelt es sich um die Bereitstellungseinheit für eine Anwendung. Dieser Artikel richtet sich an Entwicklerinnen und Entwickler, die einen OSGi-Dienst oder ein Servlet mit AEM Forms 6.4 oder 6.5 erstellen möchten. Gehen Sie wie folgt vor, um Ihr erstes OSGi-Bundle zu erstellen:


## Installieren des JDK 

Installieren Sie die unterstützte JDK-Version. Für dieses Beispiel wurde JDK 1.8 verwendet. Stellen Sie sicher, dass Sie **JAVA_HOME** in Ihren Umgebungsvariablen hinzugefügt haben und diese Variable auf den Stammordner Ihrer JDK-Installation verweist.
Fügen Sie %JAVA_HOME%/bin zum Pfad hinzu.

![data-source](assets/java-home.JPG)

>[!NOTE]
> Verwenden Sie nicht JDK 15. Diese Version wird von AEM nicht unterstützt.

### Testen Ihrer JDK-Version

Öffnen Sie ein neues Eingabeaufforderungsfenster und geben Sie Folgendes ein: `java -version`. Es sollte die durch die Variable `JAVA_HOME` identifizierte JDK-Version zurückgegeben werden.

![data-source](assets/java-version.JPG)

## Installieren von Maven

Maven ist ein Tool zur Build-Automatisierung, das hauptsächlich für Java-Projekte verwendet wird. Gehen Sie wie folgt vor, um Maven auf Ihrem lokalen System zu installieren.

* Erstellen Sie einen Ordner mit dem Namen `maven` auf dem Laufwerk „C:“.
* Laden Sie das [binäre ZIP-Archiv](http://maven.apache.org/download.cgi) herunter.
* Extrahieren Sie den Inhalt des ZIP-Archivs unter `c:\maven`.
* Erstellen Sie eine Umgebungsvariable namens `M2_HOME` mit dem Wert `C:\maven\apache-maven-3.6.0`. Im Beispiel hier wird die **mvn**-Version 3.6.0 verwendet. Zum Zeitpunkt der Erstellung dieses Artikels ist die neueste Maven-Version 3.6.3
* Fügen Sie `%M2_HOME%\bin` zu Ihrem Pfad hinzu.
* Speichern Sie Ihre Änderungen.
* Öffnen Sie eine neue Eingabeaufforderung und geben Sie `mvn -version` ein. Die **mvn**-Version sollte so wie im Screenshot aufgelistet werden.

![data-source](assets/mvn-version.JPG)

## Settings.xml

Eine `settings.xml`-Datei von Maven definiert Werte, die die Ausführung von Maven auf verschiedene Arten konfigurieren. In der Regel wird sie verwendet, um einen lokalen Repository-Speicherort, alternative Remote-Repository-Server und Authentifizierungsinformationen für private Repositorys zu definieren.

Navigieren Sie zu `C:\Users\<username>\.m2 folder`
Extrahieren Sie den Inhalt der [settings.zip](assets/settings.zip)-Datei und legen Sie sie im Ordner `.m2` ab.

## Installieren von Eclipse

Installieren der neuesten Version von [Eclipse](https://www.eclipse.org/downloads/)

## Erstellen Ihres ersten Projekts

Archetype ist ein Maven-Projektvorlagen-Toolkit. Ein Archetyp (so der deutsche Begriff) ist definiert als ein ursprüngliches Muster oder Modell, aus dem alle anderen Elemente derselben Art bestehen. Der Name passt also. Denn wir möchten ein System bereitstellen, das eine konsistente Möglichkeit zur Erstellung von Maven-Projekten bietet. Archetype unterstützt Autorinnen und Autoren beim Erstellen von Maven-Projektvorlagen für Benutzende und bietet ihnen die Möglichkeit, parametrierte Versionen dieser Projektvorlagen zu generieren.
Gehen Sie wie folgt vor, um Ihr erstes Maven-Projekt zu erstellen:

* Erstellen Sie einen neuen Ordner mit dem Namen `aemformsbundles` auf dem Laufwerk „C:“.
* Öffnen Sie eine Eingabeaufforderung und navigieren Sie zu `c:\aemformsbundles`.
* Führen Sie in der Eingabeaufforderung den folgenden Befehl aus:
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Das Maven-Projekt wird interaktiv generiert und Sie werden aufgefordert, Werte für verschiedene Eigenschaften anzugeben, z. B.

| Eigenschaftsname | Bedeutung | Wert  |
|------------------------|---------------------------------------|---------------------|
| groupId | „groupId“ sorgt für eine eindeutige Identifizierung Ihres Projekts über alle Projekte hinweg | com.learningaemforms.adobe |
| appsFolderName | Der Name des Ordners, der Ihre Projektstruktur enthält. | learningaemforms |
| artifactId | „artifactId“ ist der Name der JAR-Datei ohne Version. Wenn Sie die Datei erstellt haben, können Sie einen beliebigen Namen mit Kleinbuchstaben und ohne Sonderzeichen wählen. | learningaemforms |
| Version | Zum Verteilen können Sie eine beliebige typische Version mit Zahlen und Punkten auswählen (1.0, 1.1, 1.0.1, …). | 1.0 |

Akzeptieren Sie die Standardwerte für die anderen Eigenschaften, indem Sie die Eingabetaste drücken.
Wenn alles reibungslos funktioniert, sollte in Ihrem Befehlsfenster eine Build-Erfolgsmeldung angezeigt werden.

## Erstellen eines Eclipse-Projekts aus Ihrem Maven-Projekt

Ändern Sie das Arbeitsverzeichnis in `learningaemforms`.
Ausführen von `mvn eclipse:eclipse` über die Befehlszeile
Der obige Befehl liest Ihre Pom-Datei und erstellt Eclipse-Projekte mit korrekten Metadaten, sodass Eclipse die Projekttypen, die Beziehungen, den Klassenpfad usw. versteht.

## Importieren des Projekts in Eclipse

Starten Sie **Eclipse**.

Navigieren Sie zu **Datei > Importieren** und wählen Sie **Bestehende Maven-Projekte** aus, wie hier gezeigt.

![data-source](assets/import-mvn-project.JPG)

Klicken Sie auf „Weiter“

Wählen Sie `c:\aemformsbundles\learningaemform`s durch Klicken auf die Schaltfläche **Durchsuchen** aus.

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>Sie können je nach Bedarf die gewünschten Module importieren. Wählen und importieren Sie nur das Kernmodul, wenn Sie ausschließlich Java-Code in Ihrem Projekt erstellen möchten.

Klicken Sie auf **Beenden**, um den Importvorgang zu starten.

Das Projekt wird in Eclipse importiert und es sind verschiedene `learningaemforms.xxxx`-Ordner zu sehen.

Erweitern Sie `src/main/java` unter dem Ordner `learningaemforms.core`. Dies ist der Ordner, in den Sie den Großteil Ihres Codes schreiben.

![data-source](assets/learning-core.JPG)

## Erstellen Ihres Projekts

Nachdem Sie Ihren OSGi-Dienst bzw. das entsprechende Servlet geschrieben haben, müssen Sie Ihr Projekt erstellen, um das OSGi-Bundle zu generieren, das mithilfe der Felix-Web-Konsole bereitgestellt werden kann. Siehe [AEMFD Client SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/), um das entsprechende Client-SDK in Ihr Maven-Projekt einzuschließen. Sie müssen das AEM FD Client SDK in den Abschnitt „dependency“ der Datei `pom.xml` des Kernprojekts einschließen, wie unten dargestellt.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Gehen Sie wie folgt vor, um Ihr Projekt zu erstellen:

* Öffnen Sie ein **Eingabeaufforderungsfenster**.
* Navigieren Sie zu `c:\aemformsbundles\learningaemforms\core`
* Führen Sie den Befehl `mvn clean install` aus.
Wenn alles reibungslos funktioniert, sollte das Bundle am folgenden Speicherort angezeigt werden: `C:\AEMFormsBundles\learningaemforms\core\target`. Dieses Bundle kann jetzt mithilfe der Felix-Web-Konsole in AEM bereitgestellt werden.
