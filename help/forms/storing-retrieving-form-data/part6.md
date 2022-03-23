---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank - Bereitstellen
description: Mehrteiliges Tutorial, das Sie durch die Schritte führt, die zum Speichern und Abrufen von Formulardaten erforderlich sind
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.3,6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 11%

---

# Bereitstellen auf Ihrem Server

>[!NOTE]
>
>Folgendes ist erforderlich, damit dies auf Ihrem System ausgeführt werden kann
>
>* AEM Forms (Version 6.3 oder höher)
>* MySql Database


Um diese Funktion auf Ihrer AEM Forms-Instanz zu testen, führen Sie die folgenden Schritte aus

* Herunterladen und Bereitstellen der [MySql Driver Jar](assets/mysqldriver.jar) -Dateien, die [Felix-Webkonsole](http://localhost:4502/system/console/bundles)
* Herunterladen und Bereitstellen der [OSGi-Bundle](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) mithilfe der [Felix-Webkonsole](http://localhost:4502/system/console/bundles)
* Laden Sie die [Paket mit Client-Bibliothek, adaptiver Formularvorlage und der benutzerdefinierten Seitenkomponente](assets/store-and-fetch-af-with-data.zip) mithilfe der [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Importieren Sie die [Beispiel für ein adaptives Formular](assets/sample-adaptive-form.zip) mithilfe der [FormsAndDocuments-Schnittstelle](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importieren Sie die [form-data-db.sql](assets/form-data-db.sql) Verwendung von MySql Workbench. Dadurch werden das erforderliche Schema und die Tabellen in Ihrer Datenbank erstellt, damit dieses Tutorial funktioniert.
* Anmelden bei [configMgr.](http://localhost:4502/system/console/configMgr) Suchen Sie nach &quot;Apache Sling Connection Pooled DataSource. Erstellen Sie einen neuen Eintrag für eine Apache Sling Connection Pooled Datasource mit dem Namen **SaveAndContinue** mit den folgenden Eigenschaften:

| Eigenschaftsname | Wert |
| ------------------------|---------------------------------------|
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URI | jdbc:mysql://localhost:3306/aemformstutorial |

* Öffnen Sie die [Adaptives Formular](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Füllen Sie einige Details aus und klicken Sie auf die Schaltfläche &quot;Speichern und weiter später&quot;.
* Sie sollten eine URL mit einer GUID zurückerhalten.
* Kopieren Sie die URL und fügen Sie sie in eine neue Registerkarte des Browsers ein. **Stellen Sie sicher, dass am Ende der URL kein leerer Leerraum vorhanden ist.**
* Das adaptive Formular sollte mit den Daten aus dem vorherigen Schritt gefüllt werden.
