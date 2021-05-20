---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM verwendet Public/Private Key Paare, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. Dieses kurze Tutorial zeigt, wie kompatible Schlüssel und Keystores mithilfe des OpenSSL-Befehlszeilen-Tools generiert werden können, das sowohl mit AEM als auch mit Adobe I/O funktioniert. '
version: 6.4, 6.5
feature: 'Benutzer und Gruppen '
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# Öffentliche und private Schlüssel zur Verwendung mit Adobe I/O einrichten

AEM verwendet Public/Private Key Paare, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. Dieses kurze Tutorial zeigt, wie kompatible Schlüssel und Keystores mithilfe des Befehlszeilen-Tools [!DNL openssl] erstellt werden können, das sowohl mit AEM als auch mit Adobe I/O funktioniert.

>[!CAUTION]
>
>In diesem Handbuch werden selbstsignierte Schlüssel erstellt, die für die Entwicklung und Verwendung in niedrigeren Umgebungen nützlich sind. In Produktionsszenarios werden Schlüssel in der Regel vom IT-Sicherheitsteam eines Unternehmens generiert und verwaltet.

## Generieren Sie das Paar aus öffentlichem/privatem Schlüssel {#generate-the-public-private-key-pair}

Der Befehl [[!DNL req] ](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) des Befehlszeilen-Tools [[!DNL openssl]command](https://www.openssl.org/docs/man1.0.2/man1/req.html) kann verwendet werden, um ein Schlüsselpaar zu generieren, das mit Adobe I/O und Adobe Experience Manager kompatibel ist.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Um den Befehl [!DNL openssl generate] auszuführen, geben Sie bei Bedarf die Zertifikatinformationen an. Adobe I/O und AEM kümmern sich nicht darum, was diese Werte sind, sollten jedoch mit Ihrem Schlüssel übereinstimmen und ihn beschreiben.

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## Schlüsselpaar zu einem neuen Keystore hinzufügen {#add-key-pair-to-a-new-keystore}

Schlüsselpaare können zu einem neuen [!DNL PKCS12]-Keystore hinzugefügt werden. Im Rahmen des Befehls [[!DNL openssl]'s [!DNL pcks12] wird](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) der Name des Keystore (über `-  caname`), der Name des Schlüssels (über `-name`) und das Kennwort des Keystore (über `-  passout`) definiert.

Diese Werte sind erforderlich, um den Keystore und die Schlüssel in AEM zu laden.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

Die Ausgabe dieses Befehls ist eine `keystore.p12`-Datei.

>[!NOTE]
>
>Die Parameterwerte von **[!DNL my-keystore]**, **[!DNL my-key]** und **[!DNL my-password]** müssen durch Ihre eigenen Werte ersetzt werden.

## Überprüfen des Keystore-Inhalts {#verify-the-keystore-contents}

Das Java [[!DNL keytool] Befehlszeilen-Tool](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) bietet Einblicke in einen Keystore, um sicherzustellen, dass die Schlüssel erfolgreich in die Keystore-Datei ([!DNL keystore.p12]) geladen werden.

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Schlüsselspeicher in Adobe I/O überprüfen](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Hinzufügen des Keystore zu AEM {#adding-the-keystore-to-aem}

AEM verwendet den generierten **privaten Schlüssel**, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. Damit der private Schlüssel für AEM zugänglich ist, muss er im Keystore eines AEM Benutzers installiert sein.

Navigieren Sie zu **AEM > [!UICONTROL Tools] > [!UICONTROL Sicherheit] > [!UICONTROL Benutzer]** und bearbeiten Sie den Benutzer **, dem der private Schlüssel zugeordnet werden soll.**

### Erstellen eines AEM-Keystore {#create-an-aem-keystore}

![KeyStore in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM erstellen >  [!UICONTROL Tools]  >  [!UICONTROL Sicherheit]  >  [!UICONTROL Benutzer]  > Benutzer bearbeiten*

Wenn Sie dazu aufgefordert werden, einen Keystore zu erstellen, tun Sie dies. Dieser Keystore existiert nur in AEM und ist NICHT der Keystore, der über openssl erstellt wurde. Das Kennwort kann beliebig sein und muss nicht mit dem Kennwort übereinstimmen, das im Befehl [!DNL openssl] verwendet wird.

### Installieren Sie den privaten Schlüssel über den Keystore {#install-the-private-key-via-the-keystore}.

![Privaten Schlüssel in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser hinzufügen]  >  [!UICONTROL Keystore]  > privaten Schlüssel aus Keystore  [!UICONTROL hinzufügen]*

Klicken Sie in der Keystore-Konsole des Benutzers auf **[!UICONTROL Add Private Key Form KeyStore file]** und fügen Sie die folgenden Informationen hinzu:

* **[!UICONTROL Neuer Alias]**: den Alias des Schlüssels in AEM. Dies kann alles sein und muss nicht mit dem Namen des Keystore übereinstimmen, der mit dem openssl-Befehl erstellt wurde.
* **[!UICONTROL KeyStore-Datei]**: die Ausgabe des Befehls openssl pkcs12 (keystore.p12)
* **[!UICONTROL KeyStore-Dateikennwort]**: Das Kennwort, das im Befehl openssl pkcs12 über  `-passout` Argument festgelegt wurde.
* **[!UICONTROL Alias für privaten Schlüssel]**: Der dem  `-name` Argument im oben stehenden Befehl openssl pkcs12 angegebene Wert (d. h.  `my-key`).
* **[!UICONTROL Passwort für privaten Schlüssel]**: Das Kennwort, das im Befehl openssl pkcs12 über  `-passout` Argument festgelegt wurde.

>[!CAUTION]
>
>Das Kennwort für die KeyStore-Datei und das Kennwort für den privaten Schlüssel sind für beide Eingaben identisch. Wenn Sie ein nicht übereinstimmendes Kennwort eingeben, wird der Schlüssel nicht importiert.

### Überprüfen Sie, ob der private Schlüssel in den AEM-Keystore geladen wird {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Überprüfen des privaten Schlüssels in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL AEMser]  >  [!UICONTROL Keystore]*

Wenn der private Schlüssel erfolgreich aus dem bereitgestellten Keystore in den AEM-Keystore geladen wurde, werden die Metadaten des privaten Schlüssels in der Keystore-Konsole des Benutzers angezeigt.

## Hinzufügen des öffentlichen Schlüssels zur Adobe I/O {#adding-the-public-key-to-adobe-i-o}

Der entsprechende öffentliche Schlüssel muss in Adobe I/O hochgeladen werden, damit der AEM-Dienstbenutzer, der über den privaten Schlüssel des öffentlichen Schlüssels verfügt, sicher kommunizieren kann.

### Erstellen einer neuen Adobe I/O-Integration {#create-a-adobe-i-o-new-integration}

![Erstellen einer neuen Adobe I/O-Integration](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Adobe I/O-Integration erstellen]](https://console.adobe.io/)  >  [!UICONTROL Neue Integration]*

Für das Erstellen einer neuen Integration in Adobe I/O ist das Hochladen eines öffentlichen Zertifikats erforderlich. Laden Sie die **certificate.crt** hoch, die vom `openssl req`-Befehl generiert wurde.

### Überprüfen Sie, ob die öffentlichen Schlüssel in Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o} geladen werden.

![Überprüfen der öffentlichen Schlüssel in Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Die installierten öffentlichen Schlüssel und ihre Ablaufdaten werden in der Konsole [!UICONTROL Integrationen] auf der Adobe I/O aufgelistet. Über die Schaltfläche **[!UICONTROL Öffentlichen Schlüssel hinzufügen]** können mehrere öffentliche Schlüssel hinzugefügt werden.

Nun AEM den privaten Schlüssel speichern, und die Adobe I/O-Integration enthält den entsprechenden öffentlichen Schlüssel, sodass AEM sicher mit der Adobe I/O kommunizieren können.
