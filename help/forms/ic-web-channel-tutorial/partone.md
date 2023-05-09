---
title: Installieren und Konfigurieren von Tomcat
seo-title: Install and Configure Tomcat
description: Dies ist Teil 1 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil installieren wir TOMCAT und stellen die Datei sampleRest.war in TOMCAT bereit.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 1%

---

# Installieren und Konfigurieren von Tomcat {#install-and-configure-tomcat}

In diesem Teil installieren wir TOMCAT und stellen die Datei sampleRest.war in TOMCAT bereit. Der von dieser WAR-Datei angezeigte REST-Endpunkt ist die Grundlage für unser Datenquellen- und Formulardatenmodell.

Um Tomcat einzurichten, befolgen Sie die folgenden Anweisungen:

1. Laden Sie JDK1.8 herunter und installieren Sie es.
2. Setzen Sie JAVA_HOME auf JDK1.8.
3. Download [Tomcat](https://tomcat.apache.org/). Diese WAR-Datei wurde mit den Tomcat-Versionen 8.5.x und 9.0.x getestet.
4. Laden Sie die Tomcat-Version Ihrer Voreinstellung herunter. Sie können die 64-Bit-Windows-Zip unter den Kernabschnitt herunterladen.
5. Entpacken Sie den Inhalt in Ihre c:\tomcat.
6. Sie sollten so etwas in Ihrem c-Laufwerk sehen **c:\tomcat\apache-tomcat-8.5.27** abhängig von der Version Ihres Tomcat
7. Erstellen Sie eine Umgebungsvariable mit dem Namen &quot;CATALINA_HOME&quot;und legen Sie ihren Wert auf den Tomcat-Installationsordner fest, Beispiel c:\tomcat\apache- tomcat-8.5.27
8. Kopieren Sie die Datei SampleRest.war in den Ordner webapps Ihrer Tomcat-Installation.
9. Starten Sie ein neues Eingabeaufforderungsfenster.
10. Navigieren Sie zu &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin und starten Sie startup.bat
11. Sobald Ihr Tomcat gestartet wurde, testen Sie den Endpunkt, der von der WAR-Datei offen gelegt wird von [Hier klicken](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Sie sollten Beispieldaten als Ergebnis dieses Aufrufs erhalten.

Herzlichen Glückwunsch !!!. Sie haben für Tomcat eingerichtet und die Datei SampleRest.war bereitgestellt.

## Nächste Schritte

[RESTful-Datenquelle konfigurieren](./parttwo.md)
