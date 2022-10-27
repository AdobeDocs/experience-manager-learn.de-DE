---
title: Vereinfachte Schritte für die Installation von AEM Forms unter Windows
description: Schnelle und einfache Schritte zur Installation von AEM Forms unter Windows
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '578'
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
>* Microsoft Visual C++ 2008 Redistributable
>* Microsoft Visual C++ 2010-Redistributable
>* Microsoft Visual C++ 2012-Redistributable
>* Microsoft Visual C++ 2013 Redistributable (ab 6.5)


Es wird empfohlen, [amtliche Dokumentation](https://helpx.adobe.com/de/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) für die Installation von AEM Forms. Führen Sie die folgenden Schritte aus, um AEM Forms in der Windows-Umgebung zu installieren und zu konfigurieren:

* Stellen Sie sicher, dass das entsprechende JDK installiert ist.
   * AEM 6.2 benötigen Sie: Oracle SE 8 JDK 1.8.x (64 Bit)
* 
   * AEM 6.3 und AEM 6.4 benötigen Sie: Oracle SE 8 JDK 1.8.x (64 Bit)
* AEM 6.5 benötigen Sie JDK 8 oder JDK 11
* [Offizielle JDK-Anforderungen](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=en) sind hier aufgeführt
* Stellen Sie sicher, dass JAVA_HOME so eingestellt ist, dass es auf das installierte JDK verweist.
   * Gehen Sie wie folgt vor, um die Variable JAVA_HOME in Windows zu erstellen:
      * Klicken Sie mit der rechten Maustaste auf „Arbeitsplatz“ und wählen Sie „Eigenschaften“.
      * Wählen Sie auf der Registerkarte Erweitert die Option Umgebungsvariablen aus und erstellen Sie eine neue Systemvariable mit dem Namen JAVA_HOME.
      * Legen Sie den Variablenwert so fest, dass er auf das auf Ihrem System installierte JDK verweist. Beispiel: c:\program files\java\jdk1.8.0_25

* Erstellen Sie einen Ordner mit dem Namen AEMForms auf Ihrem C-Laufwerk.
* Suchen Sie die Datei AEMQuickStart.Jar und verschieben Sie sie in den Ordner AEMForms .
* Kopieren Sie die Datei license.properties in diesen Ordner von AEMForms
* Erstellen Sie eine Batch-Datei mit dem Namen &quot;StartAEMForms.bat&quot;mit folgendem Inhalt:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * Hier AEM_6.5_Quickstart.jar ist der Name meiner AEM Schnellstart-JAR.
   * Sie können Ihre JAR-Datei in einen beliebigen Namen umbenennen. Stellen Sie jedoch sicher, dass dieser Name in der Batch-Datei angezeigt wird. Speichern Sie die Batch-Datei im Ordner &quot;AEMForms&quot;.

* Öffnen Sie eine neue Eingabeaufforderung und navigieren Sie zu _c:\aemforms_.

* Führen Sie die Datei StartAEMForms.bat an der Eingabeaufforderung aus.

* Es sollte ein kleines Dialogfeld angezeigt werden, das den Fortschritt des Starts angibt.

* Sobald der Start abgeschlossen ist, öffnen Sie die Datei sling.properties . Dies befindet sich unter c:\AEMForms\crx-quickstart\conf folder.

* Kopieren Sie die folgenden 2 Zeilen in Richtung des Dateiendes.
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Diese beiden Eigenschaften sind erforderlich, damit Document Services funktioniert
* Speichern Sie die Datei sling.properties .
* [Herunterladen des entsprechenden Forms Add-On-Pakets](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=de)
* Installieren Sie die Formulare, die dem Paket hinzugefügt werden, mit [Package Manager.](http://localhost:4502/crx/packmgr/index.jsp)
* Nachdem Sie das Paket installiert haben, müssen Sie die folgenden Schritte ausführen

       * **Stellen Sie sicher, dass alle Pakete sich im aktiven Status befinden. (Außer AEMFD Signatures Bundle).**
       * **Es würde in der Regel 5 oder mehr Minuten dauern, bis alle Bundles in den aktiven Status gelangen.**
   
   * **Sobald alle Pakete aktiv sind (mit Ausnahme des AEMFD Signatures Bundles), starten Sie das System neu, um die Installation von AEM Forms abzuschließen.**

## sun.util.calendar -Paket zur Zulassungsliste

1. Öffnen Sie die Felix-Webkonsole in Ihrem [Browserfenster](http://localhost:4502/system/console/configMgr)
2. Suchen und öffnen Sie die Deserialisierungs-Firewallkonfiguration: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. Hinzufügen `sun.util.calendar` als neuer Eintrag unter `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. Speichern Sie die Änderungen.

Herzlichen Glückwunsch!!! Sie haben AEM Forms jetzt auf Ihrem System installiert und konfiguriert.
Je nach Bedarf können Sie  [Reader-Erweiterungen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) oder [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=de) auf dem Server
