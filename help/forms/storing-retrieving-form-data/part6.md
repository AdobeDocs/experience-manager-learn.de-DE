---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Lernprogramm mit mehreren Teilen, um Sie durch die Schritte zum Speichern und Abrufen von Formulardaten zu führen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 11%

---


# Bereitstellen auf dem Server

>[!NOTE]
>
>Die folgenden Schritte sind erforderlich, um diese Funktion auf Ihrem System ausführen zu können
>
>* AEM Forms(Version 6.3 oder höher)
>* MySql-Datenbank


Gehen Sie wie folgt vor, um diese Funktion auf Ihrer AEM Forms-Instanz zu testen

* Laden Sie die JAR-Dateien für den [MySQL-Treiber unter ](assets/mysqldriver.jar) mit der [felix-Webkonsole](http://localhost:4502/system/console/bundles) herunter und stellen Sie sie bereit
* Laden Sie das [OSGi-Bundle](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) mit der [felix-Webkonsole](http://localhost:4502/system/console/bundles) herunter und stellen Sie es bereit
* Laden Sie das [Paket mit Client-lib, Vorlage für adaptive Formulare und die benutzerdefinierte Seitenkomponente](assets/store-and-fetch-af-with-data.zip) mit dem [Paketmanager](http://localhost:4502/crx/packmgr/index.jsp) herunter und installieren Sie es und installieren Sie es mithilfe des Paketmanagers
* Importieren Sie das adaptive Beispielformular [mithilfe der [FormsAndDocuments-Schnittstelle](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)](assets/sample-adaptive-form.zip)

* Importieren Sie [form-data-db.sql](assets/form-data-db.sql) mit MySql Workbench. Dadurch werden die erforderlichen Schemas und Tabellen in Ihrer Datenbank erstellt, damit dieses Lernprogramm funktioniert.
* Melden Sie sich bei [configMgr an.](http://localhost:4502/system/console/configMgr) Suchen Sie nach &quot;Apache Sling Connection Pooled DataSource. Erstellen Sie einen neuen Apache Sling Connection Pooled DataSource-Eintrag namens **SaveAndContinue** mit den folgenden Eigenschaften:

| Eigenschaftsname | Wert |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungsuri | jdbc:mysql://localhost:3306/aemformstutorial |


* Öffnen Sie das [Adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Füllen Sie einige Details aus und klicken Sie auf die Schaltfläche &quot;Später speichern und weiter&quot;.
* Sie sollten eine URL mit einer GUID zurückerhalten.
* Kopieren Sie die URL und fügen Sie sie in eine neue Registerkarte des Browsers ein. **Stellen Sie sicher, dass am Ende der URL kein Leerzeichen steht.**
* Adaptives Formular sollte mit den Daten aus dem vorherigen Schritt gefüllt werden.
