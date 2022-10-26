---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: AEM verwendet Public/Private Key Paare, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. Dieses kurze Tutorial zeigt, wie kompatible Schlüssel und Keystores mithilfe des OpenSSL-Befehlszeilen-Tools generiert werden können, das sowohl mit AEM als auch mit Adobe I/O funktioniert.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Development
role: Developer
level: Experienced
exl-id: 62ed9dcc-6b8a-48ff-8efe-57dabdf4aa66
last-substantial-update: 2022-07-17T00:00:00Z
thumbnail: KT-2450.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---

# Öffentliche und private Schlüssel zur Verwendung mit Adobe I/O einrichten

AEM verwendet Public/Private Key Paare, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. In diesem kurzen Tutorial wird veranschaulicht, wie kompatible Schlüssel und Keystores mithilfe der [!DNL openssl] Befehlszeilen-Tool, das sowohl mit AEM als auch mit Adobe I/O funktioniert.

>[!CAUTION]
>
>In diesem Handbuch werden selbstsignierte Schlüssel erstellt, die für die Entwicklung und Verwendung in niedrigeren Umgebungen nützlich sind. In Produktionsszenarios werden Schlüssel in der Regel vom IT-Sicherheitsteam eines Unternehmens generiert und verwaltet.

## Generieren Sie das Schlüsselpaar aus öffentlichem/privatem Schlüssel . {#generate-the-public-private-key-pair}

Die [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) Befehlszeilen-Tool [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) kann verwendet werden, um ein Schlüsselpaar zu generieren, das mit Adobe I/O und Adobe Experience Manager kompatibel ist.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

So schließen Sie die [!DNL openssl generate] -Befehl, geben Sie bei Bedarf die Zertifikatinformationen an. Adobe I/O und AEM kümmern sich nicht darum, was diese Werte sind, sollten jedoch mit Ihrem Schlüssel übereinstimmen und ihn beschreiben.

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

Schlüsselpaare können einem neuen [!DNL PKCS12] Keystore. Als Teil von [[!DNL openssl]'s [!DNL pcks12] Befehl,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) den Namen des Keystore (über `-  caname`), der Name des Schlüssels (über `-name`) und das Kennwort des Keystore (über `-  passout`) definiert werden.

Diese Werte sind erforderlich, um den Keystore und die Schlüssel in AEM zu laden.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

Die Ausgabe dieses Befehls ist ein `keystore.p12` -Datei.

>[!NOTE]
>
>Die Parameterwerte von **[!DNL my-keystore]**, **[!DNL my-key]** und **[!DNL my-password]** durch Ihre eigenen Werte ersetzt werden.

## Überprüfen des Keystore-Inhalts {#verify-the-keystore-contents}

Das Java™ [[!DNL keytool] Befehlszeilen-Tool](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) bietet Einblick in einen Keystore, um sicherzustellen, dass die Schlüssel erfolgreich in die Keystore-Datei geladen werden ([!DNL keystore.p12]).

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

AEM verwendet die generierte **privater Schlüssel** zur sicheren Kommunikation mit Adobe I/O und anderen Webdiensten. Damit der private Schlüssel für AEM zugänglich ist, muss er im Keystore eines AEM Benutzers installiert sein.

Navigieren Sie zu **AEM > [!UICONTROL Instrumente] > [!UICONTROL Sicherheit] > [!UICONTROL Benutzer]** und **Bearbeiten des Benutzers** der private Schlüssel zugeordnet werden soll.

### Erstellen eines AEM-Keystore {#create-an-aem-keystore}

![KeyStore in AEM erstellen](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM > [!UICONTROL Instrumente] > [!UICONTROL Sicherheit] > [!UICONTROL Benutzer] > Benutzer bearbeiten*

Wenn Sie dazu aufgefordert werden, einen Keystore zu erstellen, tun Sie dies. Dieser Keystore existiert nur in AEM und ist NICHT der Keystore, der über openssl erstellt wurde. Das Kennwort kann beliebig sein und muss nicht mit dem im [!DNL openssl] Befehl.

### Installieren Sie den privaten Schlüssel über den Keystore. {#install-the-private-key-via-the-keystore}

![Hinzufügen eines privaten Schlüssels in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL Benutzer] > [!UICONTROL Keystore] > [!UICONTROL Privaten Schlüssel aus Keystore hinzufügen]*

Klicken Sie in der Keystore-Konsole des Benutzers auf **[!UICONTROL Fügen Sie die KeyStore-Datei mit privatem Schlüssel hinzu]** und fügen Sie die folgenden Informationen hinzu:

* **[!UICONTROL Neuer Alias]**: den Alias des Schlüssels in AEM. Dies kann alles sein und muss nicht mit dem Namen des Keystore übereinstimmen, der mit dem openssl-Befehl erstellt wurde.
* **[!UICONTROL KeyStore-Datei]**: die Ausgabe des Befehls openssl pkcs12 (keystore.p12)
* **[!UICONTROL KeyStore-Dateikennwort]**: Das Kennwort, das im Befehl openssl pkcs12 über festgelegt wurde. `-passout` -Argument.
* **[!UICONTROL Alias für privaten Schlüssel]**: Der Wert, der dem `-name` -Argument im oben genannten Befehl openssl pkcs12 (d. h. `my-key`).
* **[!UICONTROL Passwort für privaten Schlüssel]**: Das Kennwort, das im Befehl openssl pkcs12 über festgelegt wurde. `-passout` -Argument.

>[!CAUTION]
>
>Das Kennwort für die KeyStore-Datei und das Kennwort für den privaten Schlüssel sind für beide Eingaben identisch. Wenn Sie ein nicht übereinstimmendes Kennwort eingeben, wird der Schlüssel nicht importiert.

### Überprüfen Sie, ob der private Schlüssel in den AEM-Keystore geladen wird. {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Überprüfen des privaten Schlüssels in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL Benutzer] > [!UICONTROL Keystore]*

Wenn der private Schlüssel erfolgreich aus dem bereitgestellten Keystore in den AEM-Keystore geladen wurde, werden die Metadaten des privaten Schlüssels in der Keystore-Konsole des Benutzers angezeigt.

## Hinzufügen des öffentlichen Schlüssels zur Adobe I/O {#adding-the-public-key-to-adobe-i-o}

Der entsprechende öffentliche Schlüssel muss in Adobe I/O hochgeladen werden, damit der AEM-Dienstbenutzer, der über den privaten Schlüssel des öffentlichen Schlüssels verfügt, sicher kommunizieren kann.

### Erstellen einer neuen Adobe I/O-Integration {#create-a-adobe-i-o-new-integration}

![Erstellen einer neuen Adobe I/O-Integration](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Adobe I/O-Integration erstellen]](https://developer.adobe.com/console/) > [!UICONTROL Neue Integration]*

Für das Erstellen einer Integration in Adobe I/O ist das Hochladen eines öffentlichen Zertifikats erforderlich. Hochladen der **certificate.crt** von `openssl req` Befehl.

### Stellen Sie sicher, dass die öffentlichen Schlüssel in Adobe I/O geladen werden. {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Überprüfen der öffentlichen Schlüssel in Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Die installierten öffentlichen Schlüssel und ihre Ablaufdaten werden im Abschnitt [!UICONTROL Integrationen] -Konsole auf Adobe I/O. Mehrere öffentliche Schlüssel können über die **[!UICONTROL Hinzufügen eines öffentlichen Schlüssels]** Schaltfläche.

Jetzt enthält AEM den privaten Schlüssel, und die Adobe I/O-Integration enthält den entsprechenden öffentlichen Schlüssel, sodass AEM sicher mit der Adobe I/O kommunizieren können.
