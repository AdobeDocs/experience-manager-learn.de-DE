---
title: Verwenden Sie den SSL-Assistenten in AEM
description: Adobe Experience Managers SSL-Setup-Assistent erleichtert die Einrichtung einer AEM-Instanz, die über HTTPS ausgeführt werden kann.
seo-description: Adobe Experience Managers SSL-Setup-Assistent erleichtert die Einrichtung einer AEM-Instanz, die über HTTPS ausgeführt werden kann.
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Sicherheit
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 1%

---


# Verwenden Sie den SSL-Assistenten in AEM

Adobe Experience Managers SSL-Setup-Assistent erleichtert die Einrichtung einer AEM-Instanz, die über HTTPS ausgeführt werden kann.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Für verwaltete Umgebungen ist es am besten, wenn die IT-Abteilung CA-vertrauenswürdige Zertifikate und Schlüssel bereitstellt.
>
>Selbstsignierte Zertifikate dürfen nur zu Entwicklungszwecken verwendet werden.

## Privater Schlüssel und selbstsignierter Zertifikatdownload

Die folgende ZIP-Datei enthält [!DNL DER]- und [!DNL CRT]-Dateien, die für die Einrichtung AEM SSL auf localhost erforderlich und nur für lokale Entwicklungszwecke vorgesehen sind.

Die Dateien [!DNL DER] und [!DNL CERT] werden aus praktischen Gründen bereitgestellt und mithilfe der Schritte generiert, die im Abschnitt Privaten Schlüssel und selbst signiertes Zertifikat generieren unten beschrieben werden.

Bei Bedarf lautet der Satz für die Zertifikatübergabe **admin**.

localhost - privater Schlüssel und selbstsigniertes certificate.zip (gültig bis Juli 2028)

[Zertifikatdatei herunterladen](assets/use-the-ssl-wizard/certificate.zip)

## Generieren von privaten Schlüsseln und selbstsignierten Zertifikaten

Das obige Video zeigt die Einrichtung und Konfiguration von SSL auf einer AEM Autoreninstanz mithilfe selbstsignierter Zertifikate. Die folgenden Befehle mit [[!DNL OpenSSL]](https://www.openssl.org/) können einen privaten Schlüssel und ein Zertifikat generieren, die in Schritt 2 des Assistenten verwendet werden sollen.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
