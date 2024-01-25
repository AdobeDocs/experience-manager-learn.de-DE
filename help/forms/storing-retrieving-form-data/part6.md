---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank – Bereitstellung
description: Mehrteiliges Tutorial, das Sie durch die Schritte zum Speichern und Abrufen von Formulardaten führt
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 59
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 100%

---

# Stellen Sie dies auf Ihrem Server bereit

>[!NOTE]
>
>Folgendes ist erforderlich, damit dies auf Ihrem System ausgeführt werden kann:
>
>* AEM Forms (Version 6.3 oder höher)
>* MySql Database

Um diese Funktion auf Ihrer AEM Forms-Instanz zu testen, führen Sie die folgenden Schritte aus

* Laden Sie die Dateien [MySql Driver Jar](assets/mysqldriver.jar) herunter und verteilen Sie sie über die [Felix-Web-Konsole](http://localhost:4502/system/console/bundles).
* Laden Sie das [OSGi-Bundle](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) herunter und verteilen Sie es über die [Felix-Web-Konsole](http://localhost:4502/system/console/bundles).
* Laden Sie das Paket [mit der Client-Lib, der adaptiven Formularvorlage und der benutzerdefinierten Seitenkomponente](assets/store-and-fetch-af-with-data.zip) herunter und installieren Sie es mit dem [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Importieren Sie das Beispiel des [adaptiven Formulars](assets/sample-adaptive-form.zip) über die Schnittstelle [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importieren Sie die Datei [form-data-db.sql](assets/form-data-db.sql) mit MySql Workbench.  Dadurch werden das erforderliche Schema und die Tabellen in Ihrer Datenbank erstellt, damit dieses Tutorial funktioniert.
* Melden Sie sich bei [configMgr an.](http://localhost:4502/system/console/configMgr) Suchen Sie nach „Apache Sling Connection Pooled DataSource“. Erstellen Sie einen Eintrag für eine neue Apache Sling Connection Pooled Datasource mit dem Namen **SaveAndContinue** unter Verwendung der folgenden Eigenschaften:

| Eigenschaftsname | Wert |
| ------------------------|---------------------------------------|
| Datenquellenname | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |

* Öffnen Sie das [adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Geben Sie einige Daten ein und klicken Sie auf die Schaltfläche „Speichern und später fortfahren“.
* Sie sollten eine URL mit einer GUID zurückerhalten.
* Kopieren Sie die URL und fügen Sie sie in eine neue Registerkarte des Browsers ein. **Stellen Sie sicher, dass am Ende der URL keine Leerzeichen vorhanden sind.**
* Das adaptive Formular sollte mit den Daten aus dem vorherigen Schritt gefüllt werden.
