---
title: Vereinfachte Schritte für die Installation von AEM Forms unter Windows
seo-title: Vereinfachte Schritte für die Installation von AEM Forms unter Windows
description: Schnelle und einfache Installation von AEM Forms auf Windows
seo-description: Schnelle und einfache Installation von AEM Forms auf Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: adaptive-forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: 82127d5be9a4b969537738f9ba537efe07f38479
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 7%

---

# Vereinfachte Schritte für die Installation von AEM Forms unter Windows

>[!NOTE]
>Klicken Sie niemals auf die AEM Quick Beginn-JAR, wenn Sie AEM Forms verwenden möchten.
>Achten Sie außerdem darauf, dass sich im Pfad des AEM Forms-Installationsordners keine Leerzeichen befinden.
>Installieren Sie beispielsweise AEM Forms nicht unter c:\jack and jill\AEM Forms folder

>[!NOTE]
Wenn Sie AEM Forms 6.5 installieren, vergewissern Sie sich bitte, dass die folgenden 32-Bit-Microsoft Visual C++-Redistributables installiert sind.

* Microsoft Visual C++ 2008 Redistributable
* Microsoft Visual C++ 2010 redistributable
* Microsoft Visual C++ 2012 Redistributable
* Microsoft Visual C++ 2013 redistributable (ab 6.5)

Obwohl wir empfehlen, die [offizielle Dokumentation](https://helpx.adobe.com/de/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) für die Installation von AEM Forms zu befolgen. Die folgenden Schritte können ausgeführt werden, um AEM Forms unter Windows-Umgebung zu installieren und zu konfigurieren:

* Vergewissern Sie sich, dass das entsprechende JDK installiert ist.
   * AEM 6.2 benötigen Sie: Oracle SE 8 JDK 1.8.x (64 Bit)
* 
   * AEM 6.3 und AEM 6.4 benötigen Sie: Oracle SE 8 JDK 1.8.x (64 Bit)
* AEM 6.5 benötigen Sie JDK 8 oder JDK 11
* [Die offiziellen JDK-Anforderungen](https://helpx.adobe.com/de/experience-manager/6-3/sites/deploying/using/technical-requirements.html) finden Sie hier
* Stellen Sie sicher, dass JAVA_HOME so eingestellt ist, dass es auf das installierte JDK verweist.
   * Gehen Sie wie folgt vor, um die Variable JAVA_HOME in Windows zu erstellen:
      * Klicken Sie mit der rechten Maustaste auf „Arbeitsplatz“ und wählen Sie „Eigenschaften“.
      * Wählen Sie auf der Registerkarte &quot;Erweitert&quot;die Option &quot;Umgebung-Variablen&quot;und erstellen Sie eine neue Systemvariable mit dem Namen JAVA_HOME.
      * Legen Sie den Variablenwert so fest, dass er auf das auf Ihrem System installierte JDK verweist. Beispiel: c:\program files\java\jdk1.8.0_25

* Erstellen Sie einen Ordner mit dem Namen AEMForms auf Ihrem Laufwerk.
* Suchen Sie die Datei &quot;AEMQuickStart.Jar&quot;und verschieben Sie sie in den Ordner &quot;AEMForms&quot;.
* Kopieren Sie die Datei &quot;license.properties&quot;in diesen AEMForms-Ordner
* Erstellen Sie eine Stapelverarbeitungsdatei mit dem Namen &quot;StartAemForms.bat&quot;mit folgendem Inhalt:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Hier AEM_6.3_Quickstart.jar ist der Name meiner AEM Schnellstart-JAR.
   * Sie können Ihre JAR-Datei in einen beliebigen Namen umbenennen. Achten Sie jedoch darauf, dass dieser Name in der Stapelverarbeitungsdatei wiedergegeben wird.Speichern Sie die Stapelverarbeitungsdatei im AEMForms-Ordner.

* Öffnen Sie eine neue Eingabeaufforderung und navigieren Sie zu c:\aemforms.

* Führen Sie die Datei StartAemForms.bat an der Eingabeaufforderung aus.

* Es sollte ein kleines Dialogfeld angezeigt werden, das den Fortschritt des Starts angibt.

* Sobald der Start abgeschlossen ist, öffnen Sie die Datei &quot;sling.properties&quot;. Dieser befindet sich in c:\AEMForms\crx-quickstart\conf folder.

* Kopieren Sie die folgenden 2 Zeilen nach unten
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* Diese beiden Eigenschaften sind erforderlich, damit Dokument-Services funktionieren können
* Speichern der Datei &quot;sling.properties&quot;

* [Bei Package Share anmelden](http://localhost:4502/crx/packageshare/login.html)

   * Für die Anmeldung bei Package Share benötigen Sie AdobeId
   * Suchen Sie nach AEM Forms Hinzufügen auf einem Paket, das für Ihre Version von AEM Forms und Betriebssystem geeignet ist.
   * Oder [Sie können das entsprechende Forms Add-On-Paket herunterladen](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html)
   * Nachdem Sie das Add-On-Paket installiert haben, müssen die folgenden Schritte befolgt werden

      * **Vergewissern Sie sich, dass sich alle Pakete im aktiven Status befinden. (Mit Ausnahme des Bundles für AEMFD-Signaturen).**
      * **Es dauert in der Regel 5 oder mehr Minuten, bis alle Pakete aktiv sind.**
   * **Sobald alle Pakete aktiv sind (mit Ausnahme des AEMFD-Signaturen-Bundles), starten Sie das System neu, um die AEM Forms-Installation abzuschließen**


* hinzufügen `sun.util.calendar` Paket zur Zulassungsliste:

   1. Öffnen Sie die Felix-Webkonsole in Ihrem [Browserfenster](http://localhost:4502/system/console/configMgr)
   2. Suchen und öffnen Sie die Deserialisierungs-Firewallkonfiguration: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. hinzufügen `sun.util.calendar` als neuer Eintrag unter `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Speichern Sie die Änderungen.

Herzlichen Glückwunsch!!! Sie haben AEM Forms jetzt auf Ihrem System installiert und konfiguriert.
Je nach Bedarf können Sie [Reader-Erweiterungen](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) oder [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) auf Ihrem Server konfigurieren
