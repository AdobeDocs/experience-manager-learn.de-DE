---
title: Tomcat installieren und konfigurieren
seo-title: Tomcat installieren und konfigurieren
description: Dies ist Teil 1 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil werden wir TOMCAT installieren und die sampleRest.war-Datei in TOMCAT bereitstellen. Der REST-Endpunkt, der von dieser WAR-Datei offen gelegt wird, wird die Grundlage für unser Datenquellen- und Formulardatenmodell bilden.
seo-description: Dies ist Teil 1 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikations-Dokuments. In diesem Teil werden wir TOMCAT installieren und die sampleRest.war-Datei in TOMCAT bereitstellen. Der REST-Endpunkt, der von dieser WAR-Datei offen gelegt wird, wird die Grundlage für unser Datenquellen- und Formulardatenmodell bilden.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# Tomcat {#install-and-configure-tomcat} installieren und konfigurieren

In diesem Teil werden wir TOMCAT installieren und die Datei sampleRest.war in TOMCAT bereitstellen. Der REST-Endpunkt, der von dieser WAR-Datei offen gelegt wird, wird die Grundlage für unser Datenquellen- und Formulardatenmodell bilden.

Um Tomcat einzurichten, befolgen Sie bitte die folgenden Anweisungen:

1. Laden Sie JDK1.8 herunter und installieren Sie es.
2. Setzen Sie JAVA_HOME so, dass er auf JDK1.8 verweist.
3. Laden Sie [tomcat](https://tomcat.apache.org/) herunter. Diese Kriegsdatei wurde mit den Tomcat-Versionen 8.5.x und 9.0.x getestet.
4. Laden Sie die Tomcat-Version Ihrer Präferenz herunter. Sie können die ZIP-Datei für 64-Bit-Fenster unter dem Hauptabschnitt herunterladen.
5. Dekomprimieren Sie die Inhalte auf Ihre c:\tomcat.
6. Je nach der Version Ihres Tomcat sollten Sie ungefähr Folgendes in Ihrem c-Laufwerk **c:\tomcat\apache-tomcat-8.5.27** sehen
7. Erstellen Sie eine Variable &quot;Umgebung&quot;mit dem Namen &quot;CATALINA_HOME&quot;und legen Sie als Wert c:\tomcat\apache- tomcat-8.5.27 den Beispielordner &quot;tomcat&quot;fest.
8. Kopieren Sie die Datei &quot;SampleRest.war&quot;in den Ordner &quot;webapps&quot;Ihrer Tomcat-Installation
9. Beginn neues Fenster mit Eingabeaufforderung.
10. Navigieren Sie zu &quot;\bin&quot;und führen Sie die Datei &quot;startup.bat&quot;aus.
11. Nachdem der Tomcat gestartet wurde, testen Sie den von der WAR-Datei offen gelegten Endpunkt, indem Sie [hier ](http://localhost:8080/SampleRest/webapi/getStatement/9586) klicken
12. Als Ergebnis dieses Aufrufs sollten Sie Musterdaten abrufen.

Herzlichen Glückwunsch !!!. Sie haben die Datei SampleRest.war eingerichtet und bereitgestellt.
