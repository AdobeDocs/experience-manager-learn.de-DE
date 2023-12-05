---
title: Verwenden des SSL-Assistenten in AEM
description: Der SSL-Setup-Assistent von Adobe Experience Manager erleichtert die Einrichtung einer AEM-Instanz, die über HTTPS ausgeführt werden kann.
version: 6.5, Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 615
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 100%

---

# Verwenden des SSL-Assistenten in AEM

Erfahren Sie, wie Sie SSL in Adobe Experience Manager so einrichten, dass es mithilfe des integrierten SSL-Assistenten über HTTPS ausgeführt wird.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>Für verwaltete Umgebungen ist es am besten, wenn die IT-Abteilung CA-vertrauenswürdige Zertifikate und Schlüssel bereitstellt.
>
>Selbstsignierte Zertifikate dürfen nur zu Entwicklungszwecken verwendet werden.

## Verwendung des SSL-Konfigurationsassistenten

Navigieren Sie zu __AEM Author > Tools > Sicherheit > SSL-Konfiguration__ und öffnen Sie den __SSL-Konfigurationsassistenten__.

![SSL-Konfigurationsassistent](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Erstellen von Store-Anmeldeinformationen

Um einen _Key Store_ zu erstellen, der mit der Systembenutzerin bzw. dem Systembenutzer `ssl-service` und einem globalen _Trust Store_ verbunden ist, verwenden Sie den Schritt __Store-Anmeldeinformationen__ des Assistenten.

1. Geben Sie das Kennwort für den __Key Store__ ein, der mit der Systembenutzerin bzw. dem Systembenutzer `ssl-service` verbunden ist, und bestätigen Sie es.
1. Geben Sie das Kennwort für den globalen __Trust Store__ ein und bestätigen Sie es. Es handelt sich um einen systemweiten Trust Store. Wenn er bereits erstellt wurde, wird das eingegebene Kennwort ignoriert.

   ![SSL-Setup – Anmeldeinformationen des Stores](assets/use-the-ssl-wizard/store-credentials.png)

### Hochladen des privaten Schlüssels und Zertifikats

Laden Sie den _privaten Schlüssel_ und das _SSL-Zertifikat_ mit dem Assistenten __Schlüssel und Zertifikat__ hoch.

In der Regel stellt Ihre IT-Abteilung das von einer Zertifizierungsstelle als vertrauenswürdig eingestufte Zertifikat und den Schlüssel bereit. Es kann jedoch auch ein selbstsigniertes Zertifikat für __Entwicklungs-__ und __Testzwecke__ verwendet werden.

Informationen zum Erstellen oder Herunterladen des selbstsignierten Zertifikats finden Sie unter [Selbstsignierter privater Schlüssel und Zertifikat](#self-signed-private-key-and-certificate).

1. Laden Sie den __privaten Schlüssel__ im Format DER (Distinguished Encoding Rules) hoch. Im Gegensatz zu PEM enthalten DER-kodierte Dateien keine reinen Text-Anweisungen wie `-----BEGIN CERTIFICATE-----`
1. Laden Sie das zugehörige __SSL-Zertifikat__ im Format `.crt` hoch.

   ![SSL-Setup – Privater Schlüssel und Zertifikat](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### Aktualisieren der SSL-Connector-Details

Aktualisieren Sie den _Hostnamen_ und den _Port_ mit dem Schritt __SSL-Connector__ des Assistenten.

1. Aktualisieren oder überprüfen Sie den Wert des __HTTPS-Hostnamen__. Dieser sollte mit dem `Common Name (CN)` aus dem Zertifikat übereinstimmen.
1. Aktualisieren oder überprüfen Sie den Wert __HTTPS-Port__.

   ![SSL-Setup – SSL-Connector-Details](assets/use-the-ssl-wizard/ssl-connector-details.png)

### Überprüfen des SSL-Setups

1. Klicken Sie zum Überprüfen des SSL-Setups auf die Schaltfläche __Zur HTTPS-URL gehen__.
1. Bei Verwendung eines selbstsignierten Zertifikats wird der Fehler `Your connection is not private` angezeigt.

   ![SSL-Setup – Überprüfen von AEM über HTTPS](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Selbstsignierter privater Schlüssel und Zertifikat

Die folgende ZIP-Datei enthält [!DNL DER]- und [!DNL CRT]-Dateien, die für die lokale Einrichtung von AEM SSL erforderlich und nur für lokale Entwicklungszwecke vorgesehen sind.

Die [!DNL DER]- und [!DNL CERT]-Dateien werden aus praktischen Gründen bereitgestellt und mithilfe der Schritte generiert, die im Abschnitt „Generieren eines privaten Schlüssels und eines selbstsignierten Zertifikats“ unten beschrieben werden.

Bei Bedarf lautet die Passphrase für die Zertifikatübergabe **admin**.

Dieser localhost – privater Schlüssel und selbstsigniertes certificate.zip (gültig bis Juli 2028)

[Herunterladen der Zertifikatdatei](assets/use-the-ssl-wizard/certificate.zip)

### Generieren von privaten Schlüsseln und selbstsignierten Zertifikaten

Das obige Video zeigt die Einrichtung und Konfiguration von SSL auf einer AEM-Autoreninstanz mithilfe selbstsignierter Zertifikate. Die folgenden Befehle verwenden [[!DNL OpenSSL]](https://www.openssl.org/), um einen privaten Schlüssel und ein Zertifikat zu generieren, die in Schritt 2 des Assistenten verwendet werden sollen.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
