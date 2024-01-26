---
title: Installieren und Konfigurieren von Tomcat
description: Dies ist Teil 1 des mehrstufigen Tutorials zum Erstellen Ihres ersten interaktiven Kommunikationsdokuments. In diesem Teil installieren wir TOMCAT und stellen die Datei sampleRest.war in TOMCAT bereit.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
duration: 54
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 100%

---

# Installieren und Konfigurieren von Tomcat {#install-and-configure-tomcat}

In diesem Teil installieren wir TOMCAT und stellen die Datei sampleRest.war in TOMCAT bereit. Der von dieser WAR-Datei offengelegte REST-Endpunkt ist die Grundlage für unser Datenquellen- und Formulardatenmodell.

Um Tomcat einzurichten, befolgen Sie die folgenden Anweisungen:

1. Laden Sie JDK1.8 herunter und installieren Sie es.
2. Stellen Sie ein, dass JAVA_HOME auf JDK1.8 verweist.
3. Laden Sie [Tomcat](https://tomcat.apache.org/) herunter. Diese WAR-Datei wurde mit den Tomcat-Versionen 8.5.x und 9.0.x getestet.
4. Laden Sie die bevorzugte Tomcat-Version herunter. Sie können die Zip-Datei für 64-Bit-Windows unter den Kernabschnitt herunterladen.
5. Entpacken Sie den Inhalt in Ihr c:\tomcat.
6. Sie sollten in Ihrem C-Laufwerk so etwas wie **C:\tomcat\apache-tomcat-8.5.27** sehen, abhängig von der Version Ihres Tomcat.
7. Erstellen Sie eine Umgebungsvariable mit dem Namen „CATALINA_HOME“ und legen Sie ihren Wert auf den Tomcat-Installationsordner fest, zum Beispiel: c:\tomcat\apache-tomcat-8.5.27
8. Kopieren Sie die Datei SampleRest.war in den Ordner „webapps“ Ihrer Tomcat-Installation.
9. Öffnen Sie ein neues Eingabeaufforderungsfenster.
10. Navigieren Sie zu &lt;tomcat install folder>\bin und starten Sie startup.bat
11. Sobald Ihr Tomcat gestartet wurde, testen Sie den von der WAR-Datei offengelegten Endpunkt, indem Sie [hier klicken](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Sie sollten als Ergebnis dieses Aufrufs Beispieldaten erhalten.

Herzlichen Glückwunsch! Sie haben Tomcat eingerichtet und die Datei SampleRest.war bereitgestellt.

## Nächste Schritte

[Konfigurieren der RESTful-Datenquelle](./parttwo.md)
