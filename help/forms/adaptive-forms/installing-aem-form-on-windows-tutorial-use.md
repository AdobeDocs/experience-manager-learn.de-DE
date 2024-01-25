---
title: Vereinfachte Schritte zum Installieren von AEM Forms unter Windows
description: Schnelle und einfache Schritte zur Installation von AEM Forms unter Windows
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 132
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 100%

---

# Vereinfachte Schritte zum Installieren von AEM Forms unter Windows

>[!NOTE]
>
>Doppelklicken Sie niemals auf die AEM-Schnellstart-JAR-Datei, wenn Sie AEM Forms verwenden möchten.
>
>Stellen Sie außerdem sicher, dass im Pfad des AEM Forms-Installationsordners keine Leerzeichen vorhanden sind.
>
>Installieren Sie beispielsweise AEM Forms nicht im Ordner „C:\Max und Moritz\AEM Forms“.

>[!NOTE]
>
>Wenn Sie AEM Forms 6.5 installieren, stellen Sie bitte sicher, dass Sie die folgenden verteilbaren 32-Bit-Dateien von Microsoft Visual C++ installiert haben.
>
>* Microsoft Visual C++ 2008 Redistributable
>* Microsoft Visual C++ 2010 Redistributable
>* Microsoft Visual C++ 2012 Redistributable
>* Microsoft Visual C++ 2013 Redistributable (für 6.5)

Es wird empfohlen, die [offizielle Dokumentation](https://helpx.adobe.com/de/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) zur Installation von AEM Forms zu befolgen. Führen Sie die folgenden Schritte aus, um AEM Forms in Windows-Umgebungen zu installieren und zu konfigurieren:

* Stellen Sie sicher, dass das entsprechende JDK installiert ist.
   * Für AEM 6.2 ist Oracle SE 8 JDK 1.8.x (64 Bit) erforderlich.
   * Für AEM 6.3 und AEM 6.4 ist Oracle SE 8 JDK 1.8.x (64 Bit) erforderlich.
   * Für AEM 6.5 ist JDK 8 oder JDK 11 erforderlich.
   * Die offiziellen JDK-Anforderungen sind [hier](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=de) aufgeführt.
* Stellen Sie sicher, dass JAVA_HOME auf das installierte JDK verweist.
   * Gehen Sie wie folgt vor, um die Variable JAVA_HOME unter Windows zu erstellen:
      * Klicken Sie mit der rechten Maustaste auf „Arbeitsplatz“ und wählen Sie „Eigenschaften“ aus.
      * Wählen Sie auf der Registerkarte „Erweitert“ die Option „Umgebungsvariablen“ aus und erstellen Sie eine neue Systemvariable mit dem Namen JAVA_HOME.
      * Legen Sie den Variablenwert so fest, dass er auf das auf Ihrem System installierte JDK verweist, z. B.: C:\Program Files\java\jdk1.8.0_25.

* Erstellen Sie einen Ordner namens „AEMForms“ auf dem Laufwerk „C:“.
* Suchen Sie die Datei „AEMQuickStart.jar“ und verschieben Sie sie in den Ordner „AEMForms“.
* Kopieren Sie die Datei „license.properties“ in diesen AEMForms-Ordner.
* Erstellen Sie eine Batch-Datei mit dem Namen „StartAEMForms.bat“ mit folgendem Inhalt:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * „AEM_6.5_Quickstart.jar“ entspricht dabei dem Namen der jeweiligen AEM-Schnellstart-JAR-Datei.
   * Sie können Ihre JAR-Datei beliebig umbenennen. Stellen Sie jedoch sicher, dass dieser Name in der Batch-Datei widergespiegelt wird. Speichern Sie die Batch-Datei im Ordner „AEMForms“.

* Öffnen Sie eine neue Eingabeaufforderung und navigieren Sie zu _C:\aemforms_.

* Führen Sie die Datei „StartAEMForms.bat“ über die Eingabeaufforderung aus.

* Es sollte ein kleines Dialogfeld angezeigt werden, das den Fortschritt des Startvorgangs anzeigt.

* Sobald der Startvorgang abgeschlossen ist, öffnen Sie die Datei „sling.properties“. Diese befindet sich im Ordner „C:\AEMForms\crx-quickstart\conf“.

* Kopieren Sie die folgenden beiden Zeilen im unteren Bereich der Datei.
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Diese beiden Eigenschaften sind erforderlich, damit Dokumentendienste verwendet werden können.
* Speichern Sie die Datei „sling.properties“.
* [Laden Sie das entsprechende Formular-Add-on-Paket herunter.](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=de)
* Installieren Sie das Formular-Add-on-Paket mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).
* Nachdem Sie das Paket installiert haben, müssen Sie die folgenden Schritte ausführen:

   * **Stellen Sie sicher, dass alle Bundles sich in einem aktiven Status befinden (mit Ausnahme des AEMFD Signatures-Bundles).**
   * **Es dauert in der Regel mindestens 5 Minuten, bis alle Bundles aktiv sind.**

   * **Sobald alle Bundles aktiv sind (mit Ausnahme des AEMFD Signatures-Bundles), starten Sie das System neu, um die AEM Forms-Installation abzuschließen.**

## Hinzufügen des sun.util.calendar-Pakets zur Zulassungsliste

1. Öffnen Sie die Felix-Web-Wonsole in Ihrem [Browser-Fenster](http://localhost:4502/system/console/configMgr).
1. Suchen und öffnen Sie die Deserialisierungs-Firewall-Konfiguration: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. Fügen Sie `sun.util.calendar` als neuen Eintrag unter `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name` hinzu.
1. Speichern Sie die Änderungen.

Herzlichen Glückwunsch! AEM Forms ist jetzt auf Ihrem System installiert und konfiguriert.
Je nach Bedarf können Sie [Reader Extensions](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=de) oder [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=de) auf Ihrem Server konfigurieren.
