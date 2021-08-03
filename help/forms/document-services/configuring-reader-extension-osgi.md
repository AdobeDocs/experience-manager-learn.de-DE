---
title: Konfigurieren von Reader Extensions in AEM Forms OSGi
description: Berechtigung für Reader Extensions zum Trust Store in AEM Forms OSGi hinzufügen
feature: Reader Extensions
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 14%

---


# Berechtigung für Reader Extensions hinzufügen{#configuring-reader-extension-osgi}

Der DocAssurance-Dienst kann Verwendungsrechte auf PDF-Dokumente anwenden. Um Verwendungsrechte auf PDF-Dokumente anzuwenden, konfigurieren Sie die Zertifikat.

## Erstellen von Keystore für Benutzer von fd-service

Die Berechtigung für Reader Extensions ist mit dem Benutzer fd-service verknüpft. Gehen Sie wie folgt vor, um die Berechtigung zum fd-service-Benutzer hinzuzufügen. Wenn Sie den Keystore für den fd-service-Benutzer bereits erstellt haben, überspringen Sie diesen Abschnitt

* Melden Sie sich bei Ihrer AEM-Autoreninstanz als Administrator an
* Navigieren Sie zu Tools-Security-Users .
* Scrollen Sie in der Liste der Benutzer nach unten, bis Sie das fd-service-Benutzerkonto finden.
* Klicken Sie auf den fd-service-Benutzer
* Klicken Sie auf die Registerkarte Keystore .
* Klicken Sie auf KeyStore erstellen .
* Legen Sie das KeyStore Access Password fest und speichern Sie Ihre Einstellungen, um das KeyStore-Kennwort zu erstellen.

### Berechtigung zum fd-service-Benutzerkeystore hinzufügen

Bitte folgen Sie dem Video, um die Anmeldeinformationen zum fd-service-Benutzer hinzuzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)











