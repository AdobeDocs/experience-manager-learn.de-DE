---
title: Konfigurieren von Reader Extensions in AEM Forms OSGi
description: Berechtigung für Reader Extensions zum Trust Store in AEM Forms OSGi hinzufügen
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 34%

---

# Berechtigung für Reader Extensions hinzufügen{#configuring-reader-extension-osgi}

Der DocAssurance-Dienst kann Verwendungsrechte auf PDF-Dokumente anwenden. Um Verwendungsrechte auf PDF-Dokumente anzuwenden, konfigurieren Sie die Zertifikat.

## Erstellen von Keystore für Benutzer von fd-service

Die Berechtigung für Reader Extensions ist mit dem Benutzer fd-service verknüpft. Gehen Sie wie folgt vor, um die Berechtigung zum fd-service-Benutzer hinzuzufügen. Wenn Sie den Keystore für den fd-service-Benutzer bereits erstellt haben, überspringen Sie diesen Abschnitt

* Melden Sie sich bei Ihrer AEM-Autoreninstanz als Administrator an
* Navigieren Sie zu Tools-Security-Users .
* Scrollen Sie in der Liste der Benutzer nach unten, bis Sie das fd-Dienst-Benutzerkonto finden
* Klicken Sie auf den fd-service-Benutzer
* Klicken Sie auf die Registerkarte Keystore .
* Klicken Sie auf KeyStore erstellen .
* Legen Sie das KeyStore Zugriff-Password fest und speichern Sie Ihre Einstellungen, um das KeyStore-Kennwort zu erstellen

### Berechtigung zum fd-service-Benutzerkeystore hinzufügen

Bitte folgen Sie dem Video, um die Anmeldeinformationen zum fd-service-Benutzer hinzuzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)


Der Befehl zum Auflisten der Details der pfx-Datei lautet. Der folgende Befehl setzt voraus, dass Sie sich im selben Ordner wie die pfx-Datei befinden.

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

Beispiel: keytool -v -list -storetype pkcs12 -keystore 1005566.pfx, wobei 1005566.pfx der Name meiner pfx-Datei ist
