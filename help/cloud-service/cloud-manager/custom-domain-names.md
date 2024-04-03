---
title: Hinzufügen eines benutzerdefinierten Domain-Namens
description: Erfahren Sie, wie Sie einen benutzerdefinierten Domain-Namen zu einer AEM as a Cloud Service-gehosteten Site hinzufügen.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: null
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
exl-id: 8936c3ae-2daf-4d0f-b260-28376ae28087
source-git-commit: c2b969829dc44e8235abafe0b53040b9c50fb91b
workflow-type: ht
source-wordcount: '701'
ht-degree: 100%

---

# Hinzufügen eines benutzerdefinierten Domain-Namens

Erfahren Sie, wie Sie einer AEM as a Cloud Service-Website einen benutzerdefinierten Domain-Namen hinzufügen.

In diesem Tutorial wird das Branding der Beispiel-Website [AEM WKND](https://github.com/adobe/aem-guides-wknd) erweitert, indem ein HTTPS-adressierbarer benutzerdefinierter Domain-Name `wknd.enablementadobe.com` mit Transport Layer Security (TLS) hinzugefügt wird.

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

Die allgemeinen Schritte lauten wie folgt:

![Allgemeine Schritte benutzerdefinierter Domain-Namen](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## Voraussetzungen

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) und [dig](https://www.isc.org/blogs/dns-checker/) sind auf Ihrem lokalen Computer installiert.
- Es besteht Zugang zu Drittanbieterdiensten:
   - Zertifizierungsstelle (ZS) – zum Anfordern des signierten Zertifikats für Ihre Sitedomain, z. B. [DigitCert](https://www.digicert.com/)
   - Domain Name System(DNS)-Hosting-Dienst – zum Hinzufügen von DNS-Einträgen für Ihre benutzerdefinierte Domain, z. B. Azure DNS oder AWS Route 53
- Es besteht Zugang zu [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) mit der Rolle „Geschäftsinhaber“ oder „Bereitstellungs-Manager“.
- Die Beispiel-Website [AEM WKND](https://github.com/adobe/aem-guides-wknd) wurde in einer AEMCS-Umgebung vom Typ [Produktionsprogramm](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) bereitgestellt.

Wenn Sie keinen Zugang zu Drittanbieterdiensten haben, _führen Sie diese Schritte zusammen mit Ihrem Sicherheits- oder Hostingteam aus_.

## Generieren eines SSL-Zertifikats

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Es gibt zwei Optionen:

- Mit dem Befehlszeilen-Tool `openssl` können Sie einen privaten Schlüssel und einen Certificate Signing Request (CSR) für Ihre Sitedomain generieren. Um ein signiertes Zertifikat anzufordern, reichen Sie den CSR bei einer Zertifizierungsstelle (ZS) ein.

- Ihr Hostingteam stellt den erforderlichen privaten Schlüssel und das signierte Zertifikat für Ihre Site bereit.

Sehen wir uns die Schritte für die erste Option an.

Um einen privaten Schlüssel und einen CSR zu generieren, führen Sie die folgenden Befehle aus und geben Sie die erforderlichen Informationen ein, wenn Sie dazu aufgefordert werden:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Um ein signiertes Zertifikat anzufordern, stellen Sie der ZS den generierten CSR bereit, indem Sie die zugehörige Dokumentation befolgen. Nachdem die ZS den CSR signiert hat, erhalten Sie die signierte Zertifikatsdatei.

### Überprüfen des signierten Zertifikats

Sie sollten das signierte Zertifikat überprüfen, bevor Sie es zu Cloud Manager hinzufügen. Um sich die Details zum Zertifikat anzusehen, können Sie den folgenden Befehl verwenden:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

Das signierte Zertifikat kann die Zertifikatskette enthalten, die die Stamm- und Zwischenzertifikate zusammen mit dem Anwenderzertifikat (auch als End-Entity-Zertifikat bezeichnet) umfasst.

Adobe Cloud Manager akzeptiert das Anwenderzertifikat und die Zertifikatskette _in separaten Formularfeldern_, sodass Sie das Anwenderzertifikat und die Zertifikatskette aus dem signierten Zertifikat extrahieren müssen.

In diesem Tutorial wird das [DigitCert](https://www.digicert.com/)-signierte Zertifikat, das für die Domain `*.enablementadobe.com` ausgestellt wurde, als Beispiel verwendet. Das Anwenderzertifikat und die Zertifikatskette werden extrahiert, indem das signierte Zertifikat in einem Texteditor geöffnet und der Inhalt zwischen den Markern `-----BEGIN CERTIFICATE-----` und `-----END CERTIFICATE-----` kopiert wird.

## Hinzufügen eines SSL-Zertifikats in Cloud Manager

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Um das SSL-Zertifikat in Cloud Manager hinzuzufügen, befolgen Sie die Dokumention zum [Hinzufügen eines SSL-Zertifikats](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate).

## Verifizieren des Domain-Namens

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

Gehen Sie wie folgt vor, um den Domänennamen zu verifizieren:

- Fügen Sie den Domain-Namen in Cloud Manager hinzu, indem Sie die Dokumentation zum [Hinzufügen eines benutzerdefinierten Domain-Namens](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name) befolgen.
- Fügen Sie einen AEM-spezifischen [TXT-Eintrag](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) in Ihrem DNS-Hosting-Dienst hinzu.
- Verifizieren Sie die oben genannten Schritte, indem Sie die DNS-Server mithilfe des `dig`-Befehls abrufen.

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

Die erfolgreiche Beispielantwort sieht wie folgt aus:

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

In diesem Tutorial wird Azure DNS als Beispiel verwendet. Um den TXT-Eintrag hinzuzufügen, müssen Sie die Dokumentation zu Ihrem DNS-Hosting-Dienst befolgen.

Lesen Sie unter [Überprüfen des Domain-Namenstatus](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) nach, sollte ein Problem vorliegen.

## Konfigurieren des DNS-Eintrags

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

Gehen Sie wie folgt vor, um den DNS-Eintrag für Ihre benutzerdefinierte Domain zu konfigurieren:

- Bestimmen Sie den DNS-Eintragstyp (CNAME oder APEX) basierend auf dem Domain-Typ, z. B. Stamm-Domain (APEX) oder Subdomain (CNAME), und befolgen Sie die Dokumentation zum [Konfigurieren von DNS-Einstellungen](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings).
- Fügen Sie den DNS-Eintrag in Ihrem DNS-Hosting-Dienst hinzu.
- Lösen Sie die Validierung der DNS-Einträge aus, indem Sie die Dokumentation zum [Überprüfen des Status von DNS-Einträgen](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status) befolgen.

In diesem Tutorial wird `wknd.enablementadobe.com` als **Subdomain** verwendet und der Eintragstyp „CNAME“ hinzugefügt, der auf `cdn.adobeaemcloud.com` verweist.

Wenn Sie allerdings die **Stamm-Domain** verwenden, müssen Sie den Eintragstyp „APEX“ (auch als A, ALIAS oder ANAME bezeichnet) hinzufügen, der auf die von Adobe angegebenen IP-Adressen verweist.

## Verifizieren der Site

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

Um zu verifizieren, ob auf die Site über den benutzerdefinierten Domain-Nnamen zugegriffen werden kann, öffnen Sie einen Webbrowser und navigieren Sie zur URL der benutzerdefinierten Domain. Stellen Sie sicher, dass die Site zugänglich ist und der Browser eine sichere Verbindung durch ein Schlosssymbol anzeigt.

## Vollständiges Video

Sie können sich auch das vollständige Video ansehen. Dieses liefert Ihnen einen allgemeinen Überblick und beschreibt die Voraussetzungen sowie die obigen Schritte zum Hinzufügen eines benutzerdefinierten Domain-Namens zu einer AEM as a Cloud Service-gehosteten Site.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)
