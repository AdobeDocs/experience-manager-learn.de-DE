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
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 6%

---


# Bereitstellen auf dem Server

>[!NOTE]
>
>Die folgenden Schritte sind erforderlich, um diese Funktion auf Ihrem System ausführen zu können
>
>* AEM Forms(Version 6.3 oder höher)
>* MYSQL-Datenbank


Gehen Sie wie folgt vor, um diese Funktion auf Ihrer AEM Forms-Instanz zu testen

* Laden Sie die [Übungselemente](assets/store-retrieve-form-data.zip) herunter und entpacken Sie sie auf Ihr lokales System
* Bereitstellen und Beginn der Pakete techmarketingdemos.jar und mysqldriver.jar mithilfe der [Felix-Webkonsole](http://localhost:4502/system/console/configMgr)
* Importieren Sie aemformstutorial.sql mit MYSQL Workbench. Dadurch werden die erforderlichen Schemas und Tabellen in Ihrer Datenbank erstellt, damit dieses Lernprogramm funktioniert.
* Importieren Sie &quot;StoreAndRetrieve.zip&quot;mit [AEM Package Manager.](http://localhost:4502/crx/packmgr/index.jsp) Dieses Paket enthält die Vorlage für das adaptive Formular, die Client-Bibliothek für die Seitenkomponente sowie die Beispiel-Konfiguration für das adaptive Formular und die Datenquelle.
* Melden Sie sich bei [configMgr an.](http://localhost:4502/system/console/configMgr) Suchen Sie nach &quot;Apache Sling Connection Pooled DataSource. Öffnen Sie den mit aemformstutorial verknüpften Datenquelleneintrag und geben Sie den Benutzernamen und das Kennwort für Ihre Datenbankinstanz ein.
* Öffnen des [adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Füllen Sie einige Details aus und klicken Sie auf die Schaltfläche &quot;Speichern und weiter&quot;
* Sie sollten eine URL mit einer GUID zurückerhalten.
* Kopieren Sie die URL und fügen Sie sie in eine neue Registerkarte des Browsers ein. **Stellen Sie sicher, dass am Ende der URL kein Leerzeichen steht.**
* Adaptives Formular sollte mit den Daten aus dem vorherigen Schritt gefüllt werden
