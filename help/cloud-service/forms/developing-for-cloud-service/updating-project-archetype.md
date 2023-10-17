---
title: Aktualisieren des Cloud-Service-Projekts mit dem neuesten Archetyp
description: Aktualisieren des AEM Forms-Cloud-Service-Projekts mit dem neuesten Archetyp
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
workflow-type: ht
source-wordcount: '324'
ht-degree: 100%

---

# Migration vom alten AEM-Archetyp

Um Ihr vorhandenes AEM Forms-Projekt mit dem neuesten Maven-Archetyp zu aktualisieren, müssen Sie Ihren Code, Ihre Konfigurationen usw. manuell aus dem alten in das neue Projekt kopieren.

Die folgenden Schritte wurden ausgeführt, um das Projekt, das mit dem Archetyp 30 erstellt wurde, in den Projektarchetyp 33 zu migrieren.

## Erstellen eines Maven-Projekts mit dem neuesten Archetyp

* Öffnen Sie eine Eingabeaufforderung und navigieren Sie zu „C:\cloudmanager“.
* Erstellen Sie ein Maven-Projekt mit dem neuesten Archetyp.
* Kopieren Sie den Inhalt der [Textdatei](assets/creating-maven-project.txt) und fügen Sie ihn in Ihr Eingabeaufforderungsfenster ein. Sie müssen „DarchetypeVersion=33“ möglicherweise abhängig von der [neuesten Version](https://github.com/adobe/aem-project-archetype/releases) ändern. Archetyp 33 enthält neue AEM Forms-Designs.
Da wir das neue Maven-Projekt im Ordner „cloudmanager“ erstellen, der bereits über das Projekt „aem-banking-application“ verfügt, sollten Sie die **DartifactId** von „aem-banking-application“ zu etwas anderem ändern. Für diesen Artikel wurde „aem-banking-application1“ verwendet.

>[!NOTE]
>
>Wenn Sie dieses neue Projekt wie vorliegend bereitstellen, fehlen „HandleFormSubmission“ und „SubmitToAEMServlet“ in der Cloud-Service-Instanz. Dies liegt daran, dass jedes Mal, wenn Sie ein Projekt mit Cloud Manager bereitstellen, alles unter dem Ordner `/apps` gelöscht und überschrieben wird.

## Kopieren des Java-Codes

Nachdem Ihr Projekt erfolgreich erstellt wurde, können Sie damit beginnen, den Code, die Konfigurationen usw. aus dem alten in dieses neue Projekt zu kopieren.

* Kopieren Sie „HandleFormSubmission-Servlet“ aus ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
nach
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Kopieren Sie „CustomSubmit“ von
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` aus „aem-banking-application“ in das Projekt „aem-banking-application1“.

* Importieren Sie das neue Projekt in IntelliJ.

* Aktualisieren Sie die Datei „filter.xml“ im Modul „ui.apps“ des Projekts „aem-banking-application1“, um die folgende Zeile einzuschließen:
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Nachdem Sie den gesamten Code in Ihr neues Projekt kopiert haben, können Sie dieses Projekt an Cloud Manager übertragen.

>[!NOTE]
>
>Um den Inhalt (adaptive Formulare, Formulardatenmodell usw.) mit Ihrem neuen Projekt zu synchronisieren, müssen Sie die entsprechende Ordnerstruktur in Ihrem IntelliJ-Projekt erstellen und dann Ihr IntelliJ-Projekt mit Ihrer AEM-Instanz mithilfe des Get-Befehls des Repo Tools synchronisieren.
