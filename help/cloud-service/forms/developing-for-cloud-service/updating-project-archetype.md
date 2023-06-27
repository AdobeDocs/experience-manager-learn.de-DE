---
title: Aktualisieren des Cloud Service-Projekts mit dem neuesten Archetyp
description: Aktualisieren des AEM Forms Cloud Service-Projekts mit dem neuesten Archetyp
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: AEM Project Archetype
kt: 9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---

# Migration vom alten AEM-Archetyp

Um Ihr vorhandenes AEM Forms-Projekt mit dem neuesten Maven-Archetyp zu aktualisieren, müssen Sie Ihren Code/Ihre Konfigurationen usw. manuell vom alten Projekt in das neue Projekt kopieren.

Die folgenden Schritte wurden ausgeführt, um das Projekt, das mit dem Archetyp 30 erstellt wurde, in das Projektarchetyp 33 zu migrieren

## Erstellen eines Maven-Projekts mit dem neuesten Archetyp

* Öffnen Sie die Eingabeaufforderung und navigieren Sie zu c:\cloudmanager
* Erstellen Sie ein Maven-Projekt mit dem neuesten Archetyp.
* Kopieren Sie den Inhalt der [Textdatei](assets/creating-maven-project.txt) in Ihrem Eingabeaufforderungsfenster angezeigt. Sie müssen die DarchetypeVersion=33 möglicherweise je nach [neueste Version](https://github.com/adobe/aem-project-archetype/releases). Archetyp 33 enthält neue AEM Forms-Designs.
Da wir das neue Maven-Projekt im Ordner cloudmanager erstellen, der bereits über ein AEM-Banking-Anwendungsprojekt verfügt, sollten Sie die **DartifactId** von aem-banking-application zu etwas anderem. Ich habe aem-banking-application1 für diesen Artikel verwendet.

>[!NOTE]
>
>Wenn Sie dieses neue Projekt so bereitstellen, wie es für die Cloud Service-Instanz ist, verfügen Sie nicht über HandleFormSubmission und das SubmitToAEMServlet. Dies liegt daran, dass jedes Mal, wenn Sie ein Projekt mit Cloud Manager bereitstellen, etwas unter der `/apps` -Ordner gelöscht und überschrieben wird.

## Java-Code kopieren

Nachdem Ihr Projekt erfolgreich erstellt wurde, können Sie mit dem Kopieren von Code/Konfigurationen usw. vom alten Projekt in dieses neue Projekt beginnen

* Kopieren Sie das HandleFormSubmission-Servlet aus ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
nach
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Kopieren Sie CustomSubmit aus
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` von aem-banking-application zum aem-banking-application1-Projekt

* Importieren des neuen Projekts in IntelliJ

* Aktualisieren Sie die Datei &quot;filter.xml&quot;im Modul ui.apps des Projekts aem-banking-application1, um die folgende Zeile einzuschließen
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Nachdem Sie den gesamten Code in Ihr neues Projekt kopiert haben, können Sie dieses Projekt an Cloud Manager senden.

>[!NOTE]
>
>Um den Inhalt (Adaptive Forms, Formulardatenmodell usw.) mit Ihrem neuen Projekt zu synchronisieren, müssen Sie die entsprechende Ordnerstruktur in Ihrem IntelliJ-Projekt erstellen und dann Ihr IntelliJ-Projekt mit Ihrer AEM-Instanz mithilfe des Befehls Abrufen des Repo Tools synchronisieren.
