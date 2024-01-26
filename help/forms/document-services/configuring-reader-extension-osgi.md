---
title: Konfigurieren von Reader Extensions in AEM Forms OSGi
description: Hinzufügen der Anmeldedaten für Reader Extensions zum Trust Store in AEM Forms OSGi
feature: Reader Extensions
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 317
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 100%

---

# Hinzufügen der Anmeldedaten für Reader Extensions{#configuring-reader-extension-osgi}

Der DocAssurance-Dienst kann Verwendungsrechte auf PDF-Dokumente anwenden. Um Verwendungsrechte auf PDF-Dokumente anzuwenden, konfigurieren Sie die Zertifikat.

## Erstellen eines Keystore für Benutzerinnen und Benutzer von fd-service

Die Anmeldedaten für Reader Extensions sind mit der Benutzerin bzw. dem Benutzer von fd-service verknüpft. Gehen Sie wie folgt vor, um die Anmeldedaten zur Benutzerin bzw. zum Benutzer von fd-service hinzuzufügen. Wenn Sie den Keystore für die fd-service-Benutzenden bereits erstellt haben, überspringen Sie diesen Abschnitt.

* Melden Sie sich bei Ihrer AEM-Autoreninstanz als Admin an
* Navigieren Sie zu „Tools“ > „Sicherheit“ > „Benutzende“
* Scrollen Sie in der Liste der Benutzenden nach unten, bis Sie das fd-service-Benutzerkonto finden
* Klicken Sie auf die Benutzerin bzw. den Benutzer von fd-service
* Klicken Sie auf die Registerkarte „Keystore“
* Klicken Sie auf „KeyStore erstellen“
* Legen Sie das Kennwort für den KeyStore-Zugriff fest und speichern Sie Ihre Einstellungen, um das KeyStore-Kennwort zu erstellen

### Hinzufügen der Anmeldedaten zum Keystore der Benutzerin bzw. des Benutzers von fd-service

Bitte folgen Sie dem Video, um die Anmeldedaten der Benutzerin bzw. des Benutzers von fd-service hinzuzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


Der Befehl zum Auflisten der Details der pfx-Datei lautet. Der folgende Befehl setzt voraus, dass Sie sich in demselben Verzeichnis wie die pfx-Datei befinden.

**keytool -v -list -storetype pkcs12 -keystore &lt;name of your .pfx file>**

Beispiel: keytool -v -list -storetype pkcs12 -keystore 1005566.pfx, wobei 1005566.pfx der Name meiner pfx-Datei ist
