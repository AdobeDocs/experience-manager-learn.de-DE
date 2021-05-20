---
title: Installieren und Konfigurieren von Tomcat
seo-title: Installieren und Konfigurieren von Tomcat
description: Dies ist Teil 1 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil installieren wir TOMCAT und stellen die Datei sampleRest.war in TOMCAT bereit. Der von dieser WAR-Datei angezeigte REST-Endpunkt wird die Grundlage für unsere Datenquelle und unser Formulardatenmodell sein.
seo-description: Dies ist Teil 1 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil installieren wir TOMCAT und stellen die Datei sampleRest.war in TOMCAT bereit. Der von dieser WAR-Datei angezeigte REST-Endpunkt wird die Grundlage für unsere Datenquelle und unser Formulardatenmodell sein.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 1%

---


# Installieren und Konfigurieren von Tomcat {#install-and-configure-tomcat}

In diesem Teil werden wir TOMCAT installieren und die Datei sampleRest.war in TOMCAT bereitstellen. Der von dieser WAR-Datei angezeigte REST-Endpunkt wird die Grundlage für unsere Datenquelle und unser Formulardatenmodell sein.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Um Tomcat einzurichten, befolgen Sie die folgenden Anweisungen:

1. Laden Sie JDK1.8 herunter und installieren Sie es.
2. Setzen Sie JAVA_HOME auf JDK1.8.
3. Laden Sie [tomcat](https://tomcat.apache.org/) herunter. Diese WAR-Datei wurde mit den Tomcat-Versionen 8.5.x und 9.0.x getestet.
4. Laden Sie die Tomcat-Version Ihrer Voreinstellung herunter. Sie können die 64-Bit-Windows-Zip unter den Kernabschnitt herunterladen.
5. Entpacken Sie den Inhalt in Ihre c:\tomcat.
6. Je nach der Version Ihres Tomcat sollte dieses in Ihrem c-Laufwerk **c:\tomcat\apache-tomcat-8.5.27** angezeigt werden.
7. Erstellen Sie eine Umgebungsvariable mit dem Namen &quot;CATALINA_HOME&quot;und legen Sie ihren Wert auf den Tomcat-Installationsordner fest, Beispiel c:\tomcat\apache- tomcat-8.5.27
8. Kopieren Sie die Datei SampleRest.war in den Ordner webapps Ihrer Tomcat-Installation.
9. Starten Sie ein neues Eingabeaufforderungsfenster.
10. Navigieren Sie zu &lt;Tomcat-Installationsordner>\bin und führen Sie die Datei startup.bat aus.
11. Sobald Ihr Tomcat gestartet wurde, testen Sie den von der WAR-Datei angezeigten Endpunkt, indem Sie [hier klicken](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Sie sollten Beispieldaten als Ergebnis dieses Aufrufs erhalten.

Herzlichen Glückwunsch !!!. Sie haben für Tomcat eingerichtet und die Datei SampleRest.war bereitgestellt.

Im folgenden Video wird die Implementierung der Beispielanwendung in Tomcat erläutert.
>[!VIDEO](https://video.tv.adobe.com/v/37815)