---
title: Verwenden Sie den SSL-Assistenten in AEM
description: Adobe Experience Managers SSL-Setup-Assistent, um die Einrichtung einer AEM Instanz zur Ausführung über HTTPS zu erleichtern.
seo-description: Adobe Experience Managers SSL-Setup-Assistent, um die Einrichtung einer AEM Instanz zur Ausführung über HTTPS zu erleichtern.
version: 6.3, 6,4, 6.5
feature: null
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# Verwenden Sie den SSL-Assistenten in AEM

Adobe Experience Managers SSL-Setup-Assistent, um die Einrichtung einer AEM Instanz zur Ausführung über HTTPS zu erleichtern.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Für verwaltete Umgebung ist es am besten, dass die IT-Abteilung CA-vertrauenswürdige Zertifikate und Schlüssel bereitstellt.
>
>Selbstsignierte Zertifikate dürfen nur zu Entwicklungszwecken verwendet werden.

## Privater Schlüssel und selbst signierter Zertifikatdownload

Die folgende ZIP-Datei enthält [!DNL DER]- und [!DNL CRT]-Dateien, die zum Einrichten AEM SSL auf localhost erforderlich sind und nur für lokale Entwicklungszwecke vorgesehen sind.

Die Dateien [!DNL DER] und [!DNL CERT] werden aus praktischen Gründen bereitgestellt und mithilfe der Schritte generiert, die im Abschnitt &quot;Generate Private Key and Self-Signed Certificate&quot;unten beschrieben werden.

Bei Bedarf lautet der Satz für den Zertifikatpass **admin**.

localhost - privater Schlüssel und selbstsigniertes certificate.zip (läuft Juli 2028 aus)

[Zertifikatdatei herunterladen](assets/use-the-ssl-wizard/certificate.zip)

## Generieren von privaten Schlüsseln und selbstsignierten Zertifikaten

Das obige Video zeigt die Einrichtung und Konfiguration von SSL in einer AEM Autoreninstanz mithilfe von selbstsignierten Zertifikaten. Die folgenden Befehle mit [[!DNL OpenSSL]](https://www.openssl.org/) können einen privaten Schlüssel und ein Zertifikat generieren, die in Schritt 2 des Assistenten verwendet werden sollen.

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
