---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM verwendet Paare öffentlicher/privater Schlüssel, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. Dieses kurze Lernprogramm zeigt, wie kompatible Schlüssel und Keystores mit dem OpenSSL-Befehlszeilenwerkzeug generiert werden können, das sowohl mit AEM als auch mit Adobe I/O funktioniert. '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: 3f973e36531a2d04cbaf6bb8dd70b39fef7d8b2f
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---


# Öffentliche und private Schlüssel für die Verwendung mit Adobe I/O einrichten

AEM verwendet Paare öffentlicher/privater Schlüssel, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. Dieses kurze Lernprogramm zeigt, wie kompatible Schlüssel und Keystores mit dem Befehlszeilenwerkzeug [!DNL openssl] generiert werden können, das sowohl mit AEM als auch mit Adobe I/O funktioniert.

>[!CAUTION]
>
>Dieses Handbuch erstellt selbstsignierte Schlüssel, die für die Entwicklung und Verwendung in niedrigeren Umgebung nützlich sind. In Produktionsszenarien werden Schlüssel in der Regel vom IT-Sicherheitsteam eines Unternehmens generiert und verwaltet.

## Generieren des öffentlichen/privaten Schlüsselpaars {#generate-the-public-private-key-pair}

Das Befehlszeilenwerkzeug [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) kann verwendet werden, um ein Schlüsselpaar zu generieren, das mit Adobe I/O und Adobe Experience Manager kompatibel ist.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Wenn Sie den Befehl [!DNL openssl generate] ausführen möchten, geben Sie bei Bedarf die Zertifikatsinformationen ein. Adobe I/O und AEM sind sich nicht darum kümmern, was diese Werte sind, sollten sie jedoch ausrichten und beschreiben Sie Ihren Schlüssel.

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

## hinzufügen Schlüsselpaar für einen neuen Keystore {#add-key-pair-to-a-new-keystore}

Schlüsselpaare können einem neuen Keystore [!DNL PKCS12] hinzugefügt werden. Als Teil von [[!DNL openssl]'s [!DNL pcks12] Befehl,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) der Name des Keystore (über `-  caname`), werden der Name des Schlüssels (über `-name`) und das Kennwort des Keystore (über `-  passout`) definiert.

Diese Werte sind erforderlich, um den Keystore und die Schlüssel in AEM zu laden.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

Die Ausgabe dieses Befehls ist eine `keystore.p12`-Datei.

>[!NOTE]
>
>Die Parameterwerte von **[!DNL my-keystore]**, **[!DNL my-key]** und **[!DNL my-password]** werden durch Ihre eigenen Werte ersetzt.

## Keystore-Inhalt {#verify-the-keystore-contents} überprüfen

Das Java [[!DNL keytool] Befehlszeilenwerkzeug](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) bietet Sichtbarkeit in einen Keystore, um sicherzustellen, dass die Schlüssel erfolgreich in die Keystore-Datei ([!DNL keystore.p12]) geladen werden.

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

AEM verwendet den generierten **privaten Schlüssel**, um sicher mit Adobe I/O und anderen Webdiensten zu kommunizieren. Damit der private Schlüssel AEM zugänglich ist, muss er in den Keystore eines AEM Benutzers installiert werden.

Navigieren Sie zu **AEM > [!UICONTROL Tools] > [!UICONTROL Sicherheit] > [!UICONTROL Benutzer]** und **bearbeiten Sie den Benutzer**, dem der private Schlüssel zugeordnet werden soll.

### Erstellen eines AEM Keystore {#create-an-aem-keystore}

![Create KeyStore in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEMAEM >  [!UICONTROL Tools] >  [!UICONTROL Security] >  [!UICONTROL Users] > Edit user*

Wenn Sie dazu aufgefordert werden, einen Keystore zu erstellen, führen Sie dies aus. Dieser Keystore existiert nur in AEM und ist NICHT der Keystore, der über openssl erstellt wird. Das Kennwort kann alles sein und muss nicht mit dem Kennwort übereinstimmen, das im Befehl [!DNL openssl] verwendet wird.

### Installieren Sie den privaten Schlüssel über den Keystore {#install-the-private-key-via-the-keystore}

![hinzufügen privater Schlüssel in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser] >  [!UICONTROL Keystore] >  [!UICONTROL Hinzufügen privaten Schlüssel aus Keystore]*

Klicken Sie in der Keystore-Konsole des Benutzers auf **[!UICONTROL Hinzufügen KeyStore-Datei für den privaten Schlüssel]** und fügen Sie die folgenden Informationen hinzu:

* **[!UICONTROL Neuer Alias]**: den Alias des Schlüssels in AEM. Dies kann alles sein und muss nicht mit dem Namen des Keystore übereinstimmen, der mit dem Befehl openssl erstellt wurde.
* **[!UICONTROL KeyStore-Datei]**: Ausgabe des Befehls openssl pkcs12 (keystore.p12)
* **[!UICONTROL KeyStore-Dateikennwort]**: Das im Befehl openssl pkcs12 festgelegte Kennwort  `-passout` über ein Argument.
* **[!UICONTROL Alias]** für privaten Schlüssel: Der Wert, der dem  `-name` Argument im Befehl openssl pkcs12 oben (d.h.  `my-key`).
* **[!UICONTROL Kennwort]** für privaten Schlüssel: Das im Befehl openssl pkcs12 festgelegte Kennwort  `-passout` über ein Argument.

>[!CAUTION]
>
>Das KeyStore-Dateikennwort und das Kennwort für den privaten Schlüssel sind für beide Eingaben gleich. Wenn Sie ein nicht übereinstimmendes Kennwort eingeben, wird der Schlüssel nicht importiert.

### Vergewissern Sie sich, dass der private Schlüssel in den AEM-Keystore {#verify-the-private-key-is-loaded-into-the-aem-keystore} geladen wird.

![Private Schlüssel in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL AEMUser]  überprüfen >  [!UICONTROL Keystore]*

Wenn der private Schlüssel erfolgreich aus dem bereitgestellten Keystore in den AEM Keystore geladen wurde, werden die Metadaten des privaten Schlüssels in der Keystore-Konsole des Benutzers angezeigt.

## Hinzufügen des öffentlichen Schlüssels zu Adobe I/O {#adding-the-public-key-to-adobe-i-o}

Der entsprechende öffentliche Schlüssel muss nach Adobe I/O hochgeladen werden, damit der AEM-Dienstbenutzer, der über den entsprechenden privaten Schlüssel verfügt, sicher kommunizieren kann.

### Neue Adobe I/O-Integration {#create-a-adobe-i-o-new-integration} erstellen

![Neue Adobe I/O-Integration erstellen](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Adobe I/O-Integration]](https://console.adobe.io/)  erstellen >  [!UICONTROL Neue Integration]*

Zum Erstellen einer neuen Integration in Adobe I/O muss ein öffentliches Zertifikat hochgeladen werden. Laden Sie die Datei **certificate.crt** hoch, die mit dem Befehl `openssl req` generiert wurde.

### Vergewissern Sie sich, dass die öffentlichen Schlüssel in Adobe I/O geladen wurden.{#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Öffentliche Schlüssel in Adobe I/O überprüfen](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Die installierten öffentlichen Schlüssel und ihr Ablaufdatum werden in der Konsole [!UICONTROL Integrationen] unter Adobe I/O aufgeführt. Über die Schaltfläche **[!UICONTROL Hinzufügen einem öffentlichen Schlüssel]** können mehrere öffentliche Schlüssel hinzugefügt werden.

Jetzt halten AEM den privaten Schlüssel und die Adobe I/O-Integration enthält den entsprechenden öffentlichen Schlüssel, sodass AEM sicher mit Adobe I/O kommunizieren können.
