---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Mehrteiliges Tutorial, das Sie durch die Schritte führt, die zum Speichern und Abrufen von Formulardaten erforderlich sind
feature: Adaptive Formulare
topic: Entwicklung
role: Developer
level: Experienced
version: 6.3,6.4,6.5
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 12%

---


# Bereitstellen auf Ihrem Server

>[!NOTE]
>
>Folgendes ist erforderlich, damit dies auf Ihrem System ausgeführt werden kann
>
>* AEM Forms (Version 6.3 oder höher)
>* MySql Database


Um diese Funktion auf Ihrer AEM Forms-Instanz zu testen, führen Sie die folgenden Schritte aus

* Laden Sie die [MySql Driver Jar](assets/mysqldriver.jar)-Dateien mit der [felix-Web-Konsole](http://localhost:4502/system/console/bundles) herunter und stellen Sie sie bereit.
* Laden Sie das [OSGi-Bundle](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) herunter und stellen Sie es mithilfe der [felix-Webkonsole](http://localhost:4502/system/console/bundles) bereit.
* Laden Sie das [Paket mit Client-Bibliothek, adaptiver Formularvorlage und der benutzerdefinierten Seitenkomponente](assets/store-and-fetch-af-with-data.zip) mit dem [Package Manager](http://localhost:4502/crx/packmgr/index.jsp) herunter und installieren Sie es.
* Importieren Sie das [Beispiel-Adaptive Formular](assets/sample-adaptive-form.zip) mithilfe der [FormsAndDocuments-Schnittstelle](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importieren Sie [form-data-db.sql](assets/form-data-db.sql) mithilfe von MySql Workbench. Dadurch werden das erforderliche Schema und die Tabellen in Ihrer Datenbank erstellt, damit dieses Tutorial funktioniert.
* Melden Sie sich bei [configMgr an.](http://localhost:4502/system/console/configMgr) Suchen Sie nach &quot;Apache Sling Connection Pooled DataSource. Erstellen Sie mit den folgenden Eigenschaften einen neuen Eintrag für die Datenquelle der Apache Sling Connection Pooled mit dem Namen **SaveAndContinue**:

| Eigenschaftsname | Wert |
| ------------------------|---------------------------------------|
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |

* Öffnen Sie das [Adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled).
* Füllen Sie einige Details aus und klicken Sie auf die Schaltfläche &quot;Speichern und weiter später&quot;.
* Sie sollten eine URL mit einer GUID zurückerhalten.
* Kopieren Sie die URL und fügen Sie sie in eine neue Registerkarte des Browsers ein. **Stellen Sie sicher, dass am Ende der URL kein leerer Leerraum vorhanden ist.**
* Das adaptive Formular sollte mit den Daten aus dem vorherigen Schritt gefüllt werden.
