---
title: Vereinfachte Schritte für die Installation von AEM Forms unter Windows
seo-title: Vereinfachte Schritte für die Installation von AEM Forms unter Windows
description: Schnelle und einfache Schritte zur Installation von AEM Forms unter Windows
seo-description: Schnelle und einfache Schritte zur Installation von AEM Forms unter Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: Adaptive Formulare
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 9%

---


# Vereinfachte Schritte für die Installation von AEM Forms unter Windows

>[!NOTE]
>
>Doppelklicken Sie niemals auf die AEM Schnellstartjar, wenn Sie AEM Forms verwenden möchten.
>
>Stellen Sie außerdem sicher, dass im Pfad des Installationsordners von AEM Forms keine Leerzeichen vorhanden sind.
>
>Installieren Sie beispielsweise AEM Forms nicht in c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Wenn Sie AEM Forms 6.5 installieren, stellen Sie sicher, dass Sie die folgenden 32-Bit-Redistributables für Microsoft Visual C++ installiert haben.
>
>* Redistributable Microsoft Visual C++ 2008
>* Redistributable Microsoft Visual C++ 2010
>* Redistributable Microsoft Visual C++ 2012
>* Microsoft Visual C++ 2013 Redistributable (ab 6.5)


Obwohl wir empfehlen, die [offizielle Dokumentation](https://helpx.adobe.com/de/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) zur Installation von AEM Forms zu befolgen. Führen Sie die folgenden Schritte aus, um AEM Forms in der Windows-Umgebung zu installieren und zu konfigurieren:

* Stellen Sie sicher, dass das entsprechende JDK installiert ist.
   * AEM 6.2 benötigen Sie: Oracle SE 8 JDK 1.8.x (64 Bit)
* 
   * AEM 6.3 und AEM 6.4 benötigen Sie: Oracle SE 8 JDK 1.8.x (64 Bit)
* AEM 6.5 benötigen Sie JDK 8 oder JDK 11
* [Die offiziellen JDK-](https://helpx.adobe.com/de/experience-manager/6-3/sites/deploying/using/technical-requirements.html) Anforderungen sind hier aufgeführt
* Stellen Sie sicher, dass JAVA_HOME so eingestellt ist, dass es auf das installierte JDK verweist.
   * Gehen Sie wie folgt vor, um die Variable JAVA_HOME in Windows zu erstellen:
      * Klicken Sie mit der rechten Maustaste auf „Arbeitsplatz“ und wählen Sie „Eigenschaften“.
      * Wählen Sie auf der Registerkarte Erweitert die Option Umgebungsvariablen aus und erstellen Sie eine neue Systemvariable mit dem Namen JAVA_HOME.
      * Legen Sie den Variablenwert so fest, dass er auf das auf Ihrem System installierte JDK verweist. Beispiel: c:\program files\java\jdk1.8.0_25

* Erstellen Sie einen Ordner mit dem Namen AEMForms auf Ihrem C-Laufwerk.
* Suchen Sie die Datei AEMQuickStart.Jar und verschieben Sie sie in den Ordner AEMForms .
* Kopieren Sie die Datei license.properties in diesen Ordner von AEMForms
* Erstellen Sie eine Batch-Datei mit dem Namen &quot;StartAEMForms.bat&quot;mit folgendem Inhalt:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Hier AEM_6.3_Quickstart.jar ist der Name meiner AEM Schnellstart-JAR.
   * Sie können Ihre JAR-Datei in einen beliebigen Namen umbenennen. Stellen Sie jedoch sicher, dass dieser Name in der Batch-Datei angezeigt wird. Speichern Sie die Batch-Datei im Ordner AEMForms .

* Öffnen Sie eine neue Eingabeaufforderung und navigieren Sie zu c:\aemforms.

* Führen Sie die Datei StartAEMForms.bat an der Eingabeaufforderung aus.

* Es sollte ein kleines Dialogfeld angezeigt werden, das den Fortschritt des Starts angibt.

* Sobald der Start abgeschlossen ist, öffnen Sie die Datei sling.properties . Dies befindet sich unter c:\AEMForms\crx-quickstart\conf folder.

* Kopieren Sie die folgenden 2 Zeilen in Richtung des Dateiendes.
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* Diese beiden Eigenschaften sind erforderlich, damit Document Services funktioniert
* Speichern Sie die Datei sling.properties .

* [Bei Package Share anmelden](http://localhost:4502/crx/packageshare/login.html)

   * Sie benötigen AdobeId, um sich bei Package Share anzumelden.
   * Suchen Sie nach AEM Forms Add on Package , das für Ihre Version von AEM Forms und Betriebssystem geeignet ist.
   * Oder [Sie können das entsprechende Forms Add-On-Paket](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html) herunterladen
   * Nachdem Sie das Add-On-Paket installiert haben, müssen die folgenden Schritte ausgeführt werden

      * **Stellen Sie sicher, dass alle Pakete sich im aktiven Status befinden. (Außer AEMFD Signatures bundle).**
      * **Es würde in der Regel 5 oder mehr Minuten dauern, bis alle Pakete aktiv sind.**
   * **Sobald alle Pakete aktiv sind (mit Ausnahme des AEMFD Signatures Bundles), starten Sie das System neu, um die Installation von AEM Forms abzuschließen.**


* Fügen Sie das Paket `sun.util.calendar` zur Zulassungsliste hinzu:

   1. Öffnen Sie die Felix-Webkonsole in Ihrem [Browserfenster](http://localhost:4502/system/console/configMgr)
   2. Suchen und öffnen Sie die Deserialisierungs-Firewallkonfiguration: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Fügen Sie `sun.util.calendar` als neuen Eintrag unter `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name` hinzu.
   4. Speichern Sie die Änderungen.

Herzlichen Glückwunsch!!! Sie haben AEM Forms jetzt auf Ihrem System installiert und konfiguriert.
Abhängig von Ihren Anforderungen können Sie [Reader Extensions](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) oder [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) auf Ihrem Server konfigurieren
