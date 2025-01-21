---
title: SAML 2.0 in AEM as a Cloud Service
description: Erfahren Sie, wie Sie die SAML 2.0-Authentifizierung für den Publish-Service von AEM as a Cloud Service konfigurieren.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 6f8d2bdd4ffb1c643cebcdd59fb529d8da1c44cf
workflow-type: tm+mt
source-wordcount: '4262'
ht-degree: 93%

---

# SAML 2.0-Authentifizierung{#saml-2-0-authentication}

Erfahren Sie, wie Sie Endbenutzende (keine AEM-Autorinnen oder -Autoren) für einen mit SAML 2.0 kompatiblen Identitätsanbieter (Identity Provider, IDP) Ihrer Wahl einrichten und authentifizieren.

## SAML für AEM as a Cloud Service

Die SAML 2.0-Integration in AEM Publish (oder in der Vorschau) ermöglicht es Endbenutzenden eines AEM-basierten Web-Erlebnisses, sich bei einem Nicht-Adobe-IDP zu authentifizieren und auf AEM als benannte, autorisierte Benutzende zuzugreifen.

|                       | AEM Author | AEM Publish |
|-----------------------|:----------:|:-----------:|
| Unterstützung für SAML 2.0 | ✘ | ✔ |

+++ Grundlegendes zum SAML 2.0-Fluss mit AEM

Eine SAML-Integration in AEM Publish läuft normalerweise wie folgt ab:

1. Die Benutzerin oder der Benutzer sendet eine Anfrage an AEM Publish, die angibt, dass eine Authentifizierung erforderlich ist.
   + Die Benutzerin oder der Benutzer fordert eine durch CUGs/ACL geschützte Ressource an.
   + Die Benutzerin oder der Benutzer fordert eine Ressource mit Authentifizierungspflicht an.
   + Die Benutzerin oder der Benutzer folgt einem Link zum Anmelde-Endpunkt von AEM (d. h. `/system/sling/login`), der explizit die Anmeldeaktion anfordert.
1. AEM sendet eine Authentifizierungsanfrage (AuthnRequest) an den IDP und fordert den IDP auf, den Authentifizierungsprozess zu starten.
1. Die Benutzerin oder der Benutzer authentifiziert sich beim IDP.
   + Die Benutzerin oder der Benutzer wird vom IDP zur Eingabe der Anmeldeinformationen aufgefordert.
   + Die Benutzerin oder der Benutzer ist bereits beim IDP authentifiziert und muss keine weiteren Anmeldeinformationen angeben.
1. Der IDP generiert eine SAML-Assertion mit den Benutzerdaten und signiert sie mit dem privaten Zertifikat des IDP.
1. Der IDP sendet die SAML-Assertion per HTTP-POST über den Webbrowser der Benutzerin bzw. des Benutzers (RESPECTIVE_PROTECTED_PATH/saml_login) an AEM Publish.
1. AEM Publish erhält die SAML-Assertion und überprüft die Integrität und Authentizität der SAML-Assertion mithilfe des öffentlichen IDP-Zertifikats.
1. AEM Publish verwaltet den AEM-Benutzerdatensatz basierend auf der SAML 2.0-OSGi-Konfiguration und den Inhalten der SAML-Assertion.
   + Erstellt eine Benutzerin oder einen Benutzer
   + Synchronisiert Benutzerattribute
   + Aktualisiert AEM-Benutzergruppen-Mitgliedschaften
1. AEM Publish legt das `login-token`AEM-Cookie in der HTTP-Antwort fest. Dadurch werden nachfolgende Anfragen bei AEM Publish authentifiziert.
1. AEM Publish leitet die Benutzerin bzw. den Benutzer um, wie durch das `saml_request_path`-Cookie festgelegt.

+++

## Konfigurationsanleitung

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

In diesem Video werden die Einrichtung der SAML 2.0-Integration im Publish-Service von AEM as a Cloud Service und die Verwendung von Okta als IDP erläutert.

## Voraussetzungen

Folgendes ist beim Einrichten der SAML 2.0-Authentifizierung erforderlich:

+ Zugriff von Bereitstellungs-Manager auf Cloud Manager
+ AEM-Adminzugriff auf AEM as a Cloud Service-Umgebungen
+ Administratorzugriff auf den IDP
+ (Optional) Zugriff auf ein öffentliches/privates Schlüsselpaar, das zum Verschlüsseln von SAML-Payloads verwendet wird

SAML 2.0 wird nur zur Authentifizierung von Benutzenden für AEM Publish oder die Vorschau unterstützt. Um die Authentifizierung von AEM Author mit einem IDP zu verwalten, [integrieren Sie den IDP in Adobe IMS](https://helpx.adobe.com/de/enterprise/using/set-up-identity.html).


## Installieren des öffentlichen IDP-Zertifikats in AEM

Das öffentliche Zertifikat des IDP wird zum AEM Global Trust Store hinzugefügt und dient zur Überprüfung der vom IDP gesendeten SAML-Assertion.

+ + + SAML-Assertion – Ablauf des Signiervorgangs

![SAML 2.0 – IDP-Signatur der SAML-Assertion](./assets/saml-2-0/idp-signing-diagram.png)

1. Die Benutzerin oder der Benutzer authentifiziert sich beim IDP.
1. Der IDP generiert eine SAML-Assertion mit den Benutzerdaten.
1. Der IDP signiert die SAML-Assertion mit seinem privaten Zertifikat.
1. Der IDP initiiert eine Client-seitige HTTP-POST-Anfrage an den SAML-Endpunkt von AEM Publish (`.../saml_login`), die die signierte SAML-Assertion enthält.
1. AEM Publish empfängt die HTTP-POST-Anfrage mit der signierten SAML-Assertion und kann die Signatur mithilfe des öffentlichen IDP-Zertifikats überprüfen.

+++

![Hinzufügen des öffentlichen IDP-Zertifikats zum Global Trust Store](./assets/saml-2-0/global-trust-store.png)

1. Beziehen Sie die __öffentliche Zertifikatdatei__ beim IDP. Mit diesem Zertifikat kann AEM die vom IDP bereitgestellte SAML-Assertion überprüfen.

   Das im PEM-Format vorliegende Zertifikat sollte in etwa wie folgt aussehen:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Melden Sie sich bei AEM Author als AEM-Admin an.
1. Navigieren Sie zu __Tools > Sicherheit > Trust Store__.
1. Erstellen oder öffnen Sie den Global Trust Store. Wenn Sie einen Global Trust Store erstellen, bewahren Sie das Passwort an einem sicheren Ort auf.
1. Erweitern Sie __Zertifikat aus CER-Datei hinzufügen__.
1. Wählen Sie __Zertifikatdatei auswählen__ aus und laden Sie die vom IDP bereitgestellte Zertifikatdatei hoch.
1. Lassen Sie __Benutzer Zertifikat zuordnen__ leer.
1. Klicken Sie auf __Übermitteln__.
1. Das neu hinzugefügte Zertifikat wird über dem Abschnitt __Zertifikat aus CRT-Datei hinzufügen__ angezeigt.
1. Notieren Sie sich den __Alias__, da dieser Wert in der [OSGi-Konfiguration des SAML 2.0-Authentifizierungs-Handlers](#saml-2-0-authentication-handler-osgi-configuration) verwendet wird.
1. Klicken Sie auf __Speichern und schließen__.

Der Global Trust Store ist mit dem öffentlichen Zertifikat des IDP in AEM Author konfiguriert. Da SAML jedoch nur in AEM Publish verwendet wird, muss der Global Trust Store in AEM Publish repliziert werden, damit dort auf das öffentliche IDP-Zertifikat zugegriffen werden kann.

![Replizieren des Global Trust Store für AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. Navigieren Sie zu __Tools > Bereitstellung > Pakete__.
1. Erstellen Sie ein Paket:
   + Paketname: `Global Trust Store`
   + Version: `1.0.0`
   + Gruppe: `com.your.company`
1. Bearbeiten Sie das neue __Global Trust Store__-Paket.
1. Wählen Sie die Registerkarte __Filter__ aus und fügen Sie einen Filter für den Stammpfad `/etc/truststore` hinzu.
1. Wählen Sie __Fertig__ und dann __Speichern__ aus.
1. Wählen Sie die Schaltfläche __Erstellen__ für das __Global Trust Store__-Paket aus.
1. Wählen Sie nach der Erstellung __Mehr__ > __Replizieren__ aus, um den Global Trust Store-Knoten (`/etc/truststore`) für AEM Publish zu aktivieren.

## Erstellen eines Keystores für „authentication-service“{#authentication-service-keystore}

_Für „authentication-service“ muss ein Keystore erstellt werden, wenn die [OSGi-Konfigurationseigenschaft `handleLogout` des SAML 2.0-Authentifizierungs-Handlers auf `true`](#saml-20-authenticationsaml-2-0-authentication) gesetzt ist oder wenn eine [AuthnRequest-Signierung/Verschlüsselung der SAML-Assertion](#install-aem-public-private-key-pair) erforderlich ist._

1. Melden Sie sich bei AEM Author als AEM-Admin an, um den privaten Schlüssel hochzuladen.
1. Navigieren Sie zu __Tools > Sicherheit > Benutzer__, wählen Sie __authentication-service__ als Benutzerin bzw. Benutzer aus und klicken Sie dann in der oberen Aktionsleiste auf __Eigenschaften__.
1. Wählen Sie die Registerkarte __Keystore__ aus.
1. Erstellen oder öffnen Sie den Keystore. Bewahren Sie beim Erstellen eines Keystores das Passwort sicher auf.
   + Ein [öffentlicher/privater Keystore wird in diesem Keystore nur installiert](#install-aem-public-private-key-pair), wenn eine AuthnRequest-Signierung/Verschüsselung der SAML-Assertion erforderlich ist.
   + Wenn diese SAML-Integration zwar Abmeldungen unterstützt, aber keine AuthnRequest-Signierung/SAML-Assertion, dann ist ein leerer Keystore ausreichend.
1. Klicken Sie auf __Speichern und schließen__.
1. Erstellen Sie ein Paket mit der aktualisierten Benutzerin bzw. dem aktualisierten Benutzer __authentication-service__.

   _Nutzen Sie die folgende temporäre Problemumgehung unter Verwendung von Paketen:_

   1. Navigieren Sie zu __Tools > Bereitstellung > Pakete__.
   1. Erstellen Sie ein Paket:
      + Paketname: `Authentication Service`
      + Version: `1.0.0`
      + Gruppe: `com.your.company`
   1. Bearbeiten Sie das neue __Authentication Service Key Store__-Paket.
   1. Wählen Sie die Registerkarte __Filter__ aus und fügen Sie einen Filter für den Stammpfad `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore` hinzu.
      + `<AUTHENTICATION SERVICE UUID>` finden Sie, indem Sie zu __Tools > Sicherheit > Benutzer__ navigieren und __authentication-service__ als Benutzerin bzw. Benutzer auswählen. Die UUID ist der letzte Teil der URL.
   1. Wählen Sie __Hinzufügen__ und dann __Speichern__ aus.
   1. Wählen Sie die Schaltfläche __Erstellen__ für das __Authentication Service Key Store__-Paket aus.
   1. Wählen Sie nach der Erstellung __Mehr__ > __Replizieren__ aus, um den Keystore „authentication-service“ für AEM Publish zu aktivieren.

## Installieren des öffentlichen/privaten AEM-Schlüsselpaars{#install-aem-public-private-key-pair}

_Die Installation des öffentlichen/privaten AEM-Schlüsselpaars ist optional._

AEM Publish kann so konfiguriert werden, dass AuthnRequests (an IDP) signiert und SAML-Assertionen (an AEM) verschlüsselt werden. Dies wird erreicht, indem ein privater Schlüssel für AEM Publish und der zugehörige öffentliche Schlüssel für den IDP bereitgestellt wird.

+++ Grundlegendes zum Ablauf der AuthnRequest-Signierung (optional)

Die AuthnRequest (die Anfrage an den IDP von AEM Publish, die den Anmeldeprozess initiiert) kann von AEM Publish signiert werden. Dazu signiert AEM Publish die AuthnRequest mithilfe des privaten Schlüssels, und der IDP überprüft dann die Signatur mithilfe des öffentlichen Schlüssels. Der IDP hat so die Garantie, dass die AuthnRequest von AEM Publish initiiert und angefordert wurde – und nicht von einem böswilligen Dritten.

![SAML 2.0 – AuthnRequest-Signierung durch den Dienstanbieter](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. Die Benutzerin bzw. der Benutzer sendet eine HTTP-Anfrage an AEM Publish, die zu einer SAML-Authentifizierungsanfrage an den IDP führt.
1. AEM Publish generiert die an den IDP zu sendende SAML-Anfrage.
1. AEM Publish signiert die SAML-Anfrage mithilfe des privaten AEM-Schlüssels.
1. AEM Publish initiiert die AuthnRequest, eine Client-seitige HTTP-Umleitung zum IDP, die die signierte SAML-Anfrage enthält.
1. Der IDP erhält die AuthnRequest und überprüft die Signatur mithilfe des öffentlichen AEM-Schlüssels. Dies garantiert, dass AEM Publish die AuthnRequest initiiert hat.
1. AEM Publish überprüft dann die Integrität und Authentizität der entschlüsselten SAML-Assertion mithilfe des öffentlichen IDP-Zertifikats.

+++

+++ Grundlegendes zum Fluss beim Verschlüsseln der SAML-Assertion (optional)

Die gesamte HTTP-Kommunikation zwischen IDP und AEM Publish sollte über HTTPS erfolgen und daher standardmäßig sicher sein. Bei Bedarf können SAML-Assertionen jedoch verschlüsselt werden, wenn zusätzlich zu der von HTTPS bereitgestellten eine noch größere Vertraulichkeit erforderlich ist. Dazu verschlüsselt der IDP die SAML-Assertionsdaten mit dem privaten Schlüssel, und AEM Publish entschlüsselt die SAML-Assertion mit dem privaten Schlüssel.

![SAML 2.0 – SAML-Assertionsverschlüsselung durch den Dienstanbieter](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. Die Benutzerin oder der Benutzer authentifiziert sich beim IDP.
1. Der IDP generiert eine SAML-Assertion mit den Benutzerdaten und signiert sie mit dem privaten Zertifikat des IDP.
1. Der IDP verschlüsselt dann die SAML-Assertion mit dem öffentlichen AEM-Schlüssel, wofür der private AEM-Schlüssel entschlüsselt werden muss.
1. Die verschlüsselte SAML-Assertion wird über den Webbrowser der Benutzerin bzw. des Benutzers an AEM Publish gesendet.
1. AEM Publish empfängt die SAML-Assertion und entschlüsselt sie mithilfe des privaten AEM-Schlüssels.
1. Der IDP fordert die Benutzerin bzw. den Benutzer zur Authentifizierung auf.

+++

Die Signierung der AuthnRequest und Verschlüsselung der SAML-Assertion sind optional. Beides wird jedoch mit der [OSGi-Konfigurationseigenschaft `useEncryption`](#saml-20-authenticationsaml-2-0-authentication) des SAML 2.0-Authentifizierungs-Handlers aktiviert. Das bedeutet, dass entweder beide oder keines der Verfahren verwendet werden kann.

![AEM-Keystore „authentication-service“](./assets/saml-2-0/authentication-service-key-store.png)

1. Beziehen Sie den öffentlichen Schlüssel, den privaten Schlüssel (PKCS#8 im DER-Format) und die Zertifikatkettendatei (kann der öffentliche Schlüssel sein), die zum Signieren der AuthnRequest verwendet werden, und verschlüsseln Sie die SAML-Assertion. Die Schlüssel werden in der Regel vom Sicherheits-Team der IT-Organisation bereitgestellt.

   + Ein selbstsigniertes Schlüsselpaar kann mit __openssl__ generiert werden:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Laden Sie den öffentlichen Schlüssel für den IDP hoch.
   + Bei Verwendung der oben genannten `openssl`-Methode ist der öffentliche Schlüssel die Datei `aem-public.crt`.
1. Melden Sie sich bei AEM Author als AEM-Admin an, um den privaten Schlüssel hochzuladen.
1. Navigieren Sie zu __Tools > Sicherheit > Trust Store__, wählen Sie __authentication-service__ als Benutzerin bzw. Benutzer aus und klicken Sie dann in der oberen Aktionsleiste auf __Eigenschaften__.
1. Navigieren Sie zu __Tools > Sicherheit > Benutzer__, wählen Sie __authentication-service__ als Benutzerin bzw. Benutzer aus und klicken Sie dann in der oberen Aktionsleiste auf __Eigenschaften__.
1. Wählen Sie die Registerkarte __Keystore__ aus.
1. Erstellen oder öffnen Sie den Keystore. Bewahren Sie beim Erstellen eines Keystores das Passwort sicher auf.
1. Wählen Sie __Privaten Schlüssel aus DER-Datei hinzufügen__ aus und fügen Sie den privaten Schlüssel und die Kettendatei zu AEM hinzu:
   + __Alias__: Geben Sie einen aussagekräftigen Namen ein. Häufig wird der Name des IDP verwendet.
   + __Datei mit privatem Schlüssel__: Laden Sie die Datei mit dem privaten Schlüssel hoch (PKCS#8 im DER-Format).
      + Bei Verwendung der oben genannten `openssl`-Methode ist dies die Datei `aem-private-pkcs8.der`.
   + __Zertifikatkettendateien auswählen__: Laden Sie die zugehörige Kettendatei hoch (dies kann der öffentliche Schlüssel sein).
      + Bei Verwendung der oben genannten `openssl`-Methode ist dies die Datei `aem-public.crt`.
   + Klicken Sie auf __Übermitteln__
1. Das neu hinzugefügte Zertifikat wird über dem Abschnitt __Zertifikat aus CRT-Datei hinzufügen__ angezeigt.
   + Notieren Sie sich den __Alias__, da dieser Wert in der [OSGi-Konfiguration des SAML 2.0-Authentifizierungs-Handlers](#saml-20-authentication-handler-osgi-configuration) verwendet wird.
1. Klicken Sie auf __Speichern und schließen__.
1. Erstellen Sie ein Paket mit der aktualisierten Benutzerin bzw. dem aktualisierten Benutzer __authentication-service__.

   _Nutzen Sie die folgende temporäre Problemumgehung unter Verwendung von Paketen:_

   1. Navigieren Sie zu __Tools > Bereitstellung > Pakete__.
   1. Erstellen Sie ein Paket:
      + Paketname: `Authentication Service`
      + Version: `1.0.0`
      + Gruppe: `com.your.company`
   1. Bearbeiten Sie das neue __Authentication Service Key Store__-Paket.
   1. Wählen Sie die Registerkarte __Filter__ aus und fügen Sie einen Filter für den Stammpfad `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore` hinzu.
      + `<AUTHENTICATION SERVICE UUID>` finden Sie, indem Sie zu __Tools > Sicherheit > Benutzer__ navigieren und __authentication-service__ als Benutzerin bzw. Benutzer auswählen. Die UUID ist der letzte Teil der URL.
   1. Wählen Sie __Hinzufügen__ und dann __Speichern__ aus.
   1. Wählen Sie die Schaltfläche __Erstellen__ für das __Authentication Service Key Store__-Paket aus.
   1. Wählen Sie nach der Erstellung __Mehr__ > __Replizieren__ aus, um den Keystore „authentication-service“ für AEM Publish zu aktivieren.

## Konfigurieren des SAML 2.0-Authentifizierungs-Handlers{#configure-saml-2-0-authentication-handler}

Die SAML-Konfiguration für AEM erfolgt über die OSGi-Konfiguration __Adobe Granite SAML 2.0 Authentication Handler__.
Die Konfiguration ist eine OSGi-Werkskonfiguration, d. h., ein Publish-Service von AEM as a Cloud Service kann über mehrere SAML-Konfigurationen verfügen, die diskrete Ressourcenbäume des Repositorys abdecken. Dies ist für AEM-Multi-Site-Bereitstellungen nützlich.

+++ SAML 2.0-Authentifizierungs-Handler – OSGi-Konfigurationsglossar

### OSGi-Konfiguration „Adobe Granite SAML 2.0 Authentication Handler“{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi-Eigenschaft | Erforderlich | Wertformat | Standardwert | Beschreibung |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Pfade | `path` | ✔ | Zeichenfolgen-Array | `/` | AEM-Pfade, für die dieser Authentifizierungs-Handler verwendet wird. |
| IDP-URL | `idpUrl` | ✔ | Zeichenfolge |                           | IDP-URL, an die die SAML-Authentifizierungsanfrage gesendet wird. |
| IDP-Zertifikatalias | `idpCertAlias` | ✔ | Zeichenfolge |                           | Alias des IDP-Zertifikats im AEM Global Trust Store. |
| IDP-HTTP-Umleitung | `idpHttpRedirect` | ✘ | Boolesch | `false` | Gibt an, ob eine HTTP-Umleitung zur IDP-URL erfolgt, anstatt eine AuthnRequest zu senden. Legen Sie `true` für eine IDP-initiierte Authentifizierung fest. |
| IDP-Kennung | `idpIdentifier` | ✘ | Zeichenfolge |                           | Eindeutige IDP-ID, um die Eindeutigkeit von AEM-Benutzenden und -Gruppen sicherzustellen. Ohne Angabe wird stattdessen die `serviceProviderEntityId` verwendet. |
| Assertionsverbraucherdienst-URL | `assertionConsumerServiceURL` | ✘ | Zeichenfolge |                           | Das `AssertionConsumerServiceURL`-URL-Attribut in der AuthnRequest, die angibt, wo die `<Response>`-Nachricht an AEM gesendet werden muss. |
| Entitäts-ID des Dienstanbieters | `serviceProviderEntityId` | ✔ | Zeichenfolge |                           | Eindeutige AEM-Identifizierung gegenüber dem IDP, normalerweise über den AEM-Host-Namen. |
| Verschlüsselung durch den Dienstanbieter | `useEncryption` | ✘ | Boolesch | `true` | Gibt an, ob der IDP SAML-Assertionen verschlüsselt. Erfordert, dass `spPrivateKeyAlias` und `keyStorePassword` festgelegt werden. |
| Alias für privaten Schlüssel des Dienstanbieters | `spPrivateKeyAlias` | ✘ | Zeichenfolge |                           | Der Alias des privaten Schlüssels im Keystore `authentication-service` der Benutzerin oder des Benutzers. Erforderlich, wenn für `useEncryption` der Wert `true` festgelegt ist. |
| Keystore-Passwort des Dienstanbieters | `keyStorePassword` | ✘ | Zeichenfolge |                           | Das Passwort des Keystores „authentication-service“ der Benutzerin oder des Benutzers. Erforderlich, wenn für `useEncryption` der Wert `true` festgelegt ist. |
| Standardumleitung | `defaultRedirectUrl` | ✘ | Zeichenfolge | `/` | Die standardmäßige Umleitungs-URL nach erfolgreicher Authentifizierung. Kann relativ zum AEM-Host sein (z. B. `/content/wknd/us/en/html`). |
| Benutzer-ID-Attribut | `userIDAttribute` | ✘ | Zeichenfolge | `uid` | Der Name des SAML-Assertionsattributs, das die Benutzer-ID der AEM-Benutzerin oder des AEM-Benutzers enthält. Kann frei gelassen werden, um die `Subject:NameId` zu verwenden. |
| Automatisches Erstellen von AEM-Benutzenden | `createUser` | ✘ | Boolesch | `true` | Gibt an, ob AEM-Benutzende bei erfolgreicher Authentifizierung erstellt werden. |
| Zwischenpfad für AEM-Benutzende | `userIntermediatePath` | ✘ | Zeichenfolge |                           | Bei der Erstellung von AEM-Benutzenden wird dieser Wert als Zwischenpfad verwendet (z. B. `/home/users/<userIntermediatePath>/jane@wknd.com`). Erfordert, dass für `createUser` der Wert `true` festgelegt wird. |
| AEM-Benutzerattribute | `synchronizeAttributes` | ✘ | Zeichenfolgen-Array |                           | Liste der SAML-Attributzuordnungen, die für AEM-Benutzende gespeichert werden sollen. Dies geschieht im Format `[ "saml-attribute-name=path/relative/to/user/node" ]` (z. B. `[ "firstName=profile/givenName" ]`). Siehe die [vollständige Liste nativer AEM-Attribute](#aem-user-attributes). |
| Hinzufügen einer Benutzerin oder eines Benutzers zu AEM-Gruppen | `addGroupMemberships` | ✘ | Boolesch | `true` | Gibt an, ob eine AEM-Benutzerin oder ein AEM-Benutzer nach erfolgreicher Authentifizierung automatisch zu AEM-Benutzergruppen hinzugefügt wird. |
| Attribut der AEM-Gruppenmitgliedschaft | `groupMembershipAttribute` | ✘ | Zeichenfolge | `groupMembership` | Der Name des SAML-Assertionsattributs mit einer Liste der AEM-Benutzergruppen, zu denen die Benutzerin oder der Benutzer hinzugefügt werden soll. Erfordert, dass für `addGroupMemberships` der Wert `true` festgelegt wird. |
| AEM-Standardgruppen | `defaultGroups` | ✘ | Zeichenfolgen-Array |                           | Eine Liste der AEM-Benutzergruppen, zu denen authentifizierte Benutzende immer hinzugefügt werden (z. B. `[ "wknd-user" ]`). Erfordert, dass für `addGroupMemberships` der Wert `true` festgelegt wird. |
| NameIDPolicy-Format | `nameIdFormat` | ✘ | Zeichenfolge | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | Der Wert des NameIDPolicy-Formatparameters, der in der AuthnRequest-Nachricht gesendet werden soll. |
| Speichern der SAML-Antwort | `storeSAMLResponse` | ✘ | Boolesch | `false` | Gibt an, ob der Wert von `samlResponse` im `cq:User`AEM-Knoten gespeichert wird. |
| Abmeldung verarbeiten | `handleLogout` | ✘ | Boolesch | `false` | Gibt an, ob die Abmeldeanfrage von diesem SAML-Authentifizierungs-Handler verarbeitet wird. Erfordert, dass `logoutUrl` festgelegt wird. |
| Abmelde-URL | `logoutUrl` | ✘ | Zeichenfolge |                           | IDP-URL, an die die SAML-Abmeldeanfrage gesendet wird. Erforderlich, wenn für `handleLogout` der Wert `true` festgelegt ist. |
| Uhrentoleranz | `clockTolerance` | ✘ | Ganzzahl | `60` | IDP- und AEM(Dienstanbieter)-Uhrentoleranz bei der Validierung von SAML-Assertionen. |
| Digest-Methode | `digestMethod` | ✘ | Zeichenfolge | `http://www.w3.org/2001/04/xmlenc#sha256` | Der Digest-Algorithmus, den der IDP beim Signieren einer SAML-Nachricht verwendet. |
| Signaturmethode | `signatureMethod` | ✘ | Zeichenfolge | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Der Signaturalgorithmus, den der IDP beim Signieren einer SAML-Nachricht verwendet. |
| Identitäts-Synchronisierungstyp | `identitySyncType` | ✘ | `default` oder `idp` | `default` | Darf nicht vom `from` Standard für AEM as a Cloud Service geändert werden. |
| Dienstpriorität | `service.ranking` | ✘ | Ganzzahl | `5002` | Konfigurationen mit höherem Rang werden für denselben `path` bevorzugt. |

### AEM-Benutzerattribute{#aem-user-attributes}

AEM verwendet die folgenden Benutzerattribute, die über die Eigenschaft `synchronizeAttributes` in der OSGi-Konfiguration „Adobe Granite SAML 2.0 Authentication Handler“ aufgefüllt werden können. Alle IDP-Attribute können mit einer beliebigen AEM-Benutzereigenschaft synchronisiert werden. Die Zuordnung zu AEM-Benutzerattribut-Eigenschaften (siehe unten) ermöglicht es AEM jedoch, diese natürlich zu verwenden.

| Benutzerattribut | Relativer Eigenschaftspfad vom `rep:User`-Knoten |
|--------------------------------|--------------------------|
| Anrede (beispielsweise `Mrs`) | `profile/title` |
| Vorname | `profile/givenName` |
| Nachname | `profile/familyName` |
| Stellenbezeichnung | `profile/jobTitle` |
| E-Mail-Adresse | `profile/email` |
| Straße und Hausnummer | `profile/street` |
| Stadt | `profile/city` |
| Postleitzahl | `profile/postalCode` |
| Land | `profile/country` |
| Telefonnummer | `profile/phoneNumber` |
| Info zu eigener Person | `profile/aboutMe` |

+++

1. Erstellen Sie eine OSGi-Konfigurationsdatei in Ihrem Projekt unter `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` und öffnen Sie diese in Ihrer IDE.
   + Ändern Sie `/wknd-examples/` zu Ihrem `/<project name>/`.
   + Die Kennung nach dem Zeichen `~` im Dateinamen sollte diese Konfiguration eindeutig identifizieren, etwa über den Namen des IDP, z. B. `...~okta.cfg.json`. Es sollte ein alphanumerischer Wert mit Bindestrichen sein.
1. Fügen Sie folgenden JSON-Inhalt in die Datei `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` ein und aktualisieren Sie die `wknd`-Verweise nach Bedarf.

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. Aktualisieren Sie die Werte entsprechend den Anforderungen Ihres Projekts. Beschreibungen der Konfigurationseigenschaften finden Sie oben unter __SAML 2.0-Authentifizierungs-Handler – OSGi-Konfigurationsglossar__.
1. Es wird empfohlen, ist jedoch nicht erforderlich, OSGi-Umgebungsvariablen und -Geheimnisse zu verwenden, wenn Werte nicht mehr mit dem Versionszyklus synchron sind oder sich die Werte zwischen ähnlichen Umgebungstypen/Service-Ebenen unterscheiden. Standardwerte können mithilfe der Syntax `$[env:..;default=the-default-value]"` festgelegt werden, wie oben dargestellt.

OSGi-Konfigurationen pro Umgebung (`config.publish.dev`, `config.publish.stage` und `config.publish.prod`) können mit bestimmten Attributen definiert werden, wenn die SAML-Konfiguration zwischen Umgebungen variiert.

### Verwenden von Verschlüsselung

Beim [Verschlüsseln der AuthnRequest und SAML-Assertion](#encrypting-the-authnrequest-and-saml-assertion) sind die folgenden Eigenschaften erforderlich: `useEncryption`, `spPrivateKeyAlias` und `keyStorePassword`. `keyStorePassword` enthält ein Passwort. Daher darf der Wert nicht in der OSGi-Konfigurationsdatei gespeichert werden, sondern muss stattdessen mithilfe [geheimer Konfigurationswerte](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=de#secret-configuration-values) eingefügt werden.

+++ (Optional) Aktualisieren Sie die OSGi-Konfiguration, damit verschlüsselt wird.

1. Öffnen Sie `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` in Ihrer IDE.
1. Fügen Sie die drei Eigenschaften `useEncryption`, `spPrivateKeyAlias` und `keyStorePassword` hinzu, wie unten dargestellt.

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. Die drei für die Verschlüsselung erforderlichen OSGi-Konfigurationseigenschaften sind:

+ `useEncryption` ist auf `true` festgelegt
+ `spPrivateKeyAlias` enthält den Keystore-Eintragsalias für den privaten Schlüssel, der von der SAML-Integration verwendet wird.
+ `keyStorePassword` enthält eine [geheime OSGi-Konfigurationsvariable](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=de#secret-configuration-values) mit dem Passwort für den Benutzer-Keystore `authentication-service`.

+++

## Konfigurieren des Referrer-Filters

Während des SAML-Authentifizierungsprozesses initiiert der IDP eine Client-seitige HTTP-POST-Anfrage an den AEM Publish-Endpunkt `.../saml_login`. Haben der IDP und AEM Publish einen anderen Ursprung, wird der __Referrer-Filter__ von AEM Publish über die OSGi-Konfiguration konfiguriert, um HTTP-POST-Anfragen vom IDP-Ursprung zuzulassen.

1. Erstellen (oder bearbeiten) Sie eine OSGi-Konfigurationsdatei in Ihrem Projekt unter `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Ändern Sie `/wknd-examples/` zu Ihrem `/<project name>/`.
1. Stellen Sie sicher, dass der Wert `allow.empty` auf `true` festgelegt ist, `allow.hosts` (oder `allow.hosts.regexp`, falls von Ihnen bevorzugt) den IDP-Ursprung enthält und `filter.methods` `POST` umfasst. Die OSGi-Konfiguration sollte in etwa wie folgt aussehen:

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish unterstützt eine Konfiguration mit einem einzelnen Referrer-Filter. Führen Sie daher die SAML-Konfigurationsanforderungen mit vorhandenen Konfigurationen zusammen.

OSGi-Konfigurationen pro Umgebung (`config.publish.dev`, `config.publish.stage` und `config.publish.prod`) können mit bestimmten Attributen definiert werden, wenn `allow.hosts` (oder `allow.hosts.regex`) zwischen Umgebungen variiert.

## Konfigurieren von Cross-Origin Resource Sharing (CORS)

Während des SAML-Authentifizierungsprozesses initiiert der IDP eine Client-seitige HTTP-POST-Anfrage an den AEM Publish-Endpunkt `.../saml_login`. Wenn der IDP und AEM Publish auf verschiedenen Hosts/Domains vorhanden sind, muss der Mechanismus __Cross-Origin Resource Sharing (CORS)__ so konfiguriert werden, dass HTTP-POST-Anfragen vom Host bzw. von der Domain des IDP zugelassen werden.

Der `Origin`-Header dieser HTTP-POST-Anfrage weist in der Regel einen anderen Wert auf als der AEM Publish-Host und erfordert daher eine CORS-Konfiguration.

Beim Testen der SAML-Authentifizierung für das lokale AEM SDK (`localhost:4503`) kann der IDP den `Origin`-Header auf `null` festlegen. Ist dies der Fall, fügen Sie `"null"` der `alloworigin`-Liste hinzu.

1. Erstellen Sie eine OSGi-Konfigurationsdatei in Ihrem Projekt unter `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`.
   + Ändern Sie `/wknd-examples/` zu Ihrem Projektnamen.
   + Die Kennung nach dem Zeichen `~` im Dateinamen sollte diese Konfiguration eindeutig identifizieren, etwa über den Namen des IDP, z. B. `...CORSPolicyImpl~okta.cfg.json`. Es sollte ein alphanumerischer Wert mit Bindestrichen sein.
1. Fügen Sie folgenden JSON-Inhalt in die Datei `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` ein.

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

OSGi-Konfigurationen pro Umgebung (`config.publish.dev`, `config.publish.stage`, und `config.publish.prod`) können mit bestimmten Attributen definiert werden, wenn `alloworigin` und `allowedpaths` zwischen Umgebungen variieren.

## Konfigurieren von AEM Dispatcher zum Zulassen von SAML-HTTP-POSTs

Nach erfolgreicher Authentifizierung beim IDP orchestriert der IDP eine HTTP-POST-Anfrage zurück zum registrierten `/saml_login`-AEM-Endpunkt (beim IDP konfiguriert). Diese HTTP-POST-Anfrage an `/saml_login` wird standardmäßig auf Dispatcher-Ebene blockiert. Daher muss diese explizit mithilfe der folgenden Dispatcher-Regel zugelassen werden:

1. Öffnen Sie `dispatcher/src/conf.dispatcher.d/filters/filters.any` in Ihrer IDE.
1. Fügen Sie am Ende der Datei eine Zulassungsregel für HTTP-POST-Anfragen an URLs hinzu, die mit `/saml_login` enden.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>Stellen Sie beim Bereitstellen mehrerer SAML-Konfigurationen in AEM für verschiedene geschützte Pfade und unterschiedliche IDP-Endpunkte sicher, dass der IDP an den Endpunkt RESPECTIVE_PROTECTED_PATH/saml_login postet, um die entsprechende SAML-Konfiguration auf der AEM-Seite auszuwählen. Wenn für denselben geschützten Pfad doppelte SAML-Konfigurationen vorhanden sind, erfolgt die Auswahl der SAML-Konfiguration nach dem Zufallsprinzip.

Wenn das Umschreiben von URLs auf dem Apache-Webserver konfiguriert ist (`dispatcher/src/conf.d/rewrites/rewrite.rules`) stellen Sie sicher, dass Anfragen an `.../saml_login`-Endpunkte nicht versehentlich beschädigt werden.

## Dynamische Gruppenmitgliedschaft

Dynamische Gruppenmitgliedschaft ist eine Funktion in [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) mit der die Leistung der Gruppenauswertung und -bereitstellung erhöht wird. In diesem Abschnitt wird beschrieben, wie Benutzer und Gruppen gespeichert werden, wenn diese Funktion aktiviert ist, und wie die Konfiguration des SAML-Authentifizierungs-Handlers geändert wird, um ihn für neue oder vorhandene Umgebungen zu aktivieren.

### Aktivieren der dynamischen Gruppenmitgliedschaft für SAML-Benutzende in neuen Umgebungen

Um die Leistung bei der Gruppenbewertung in neuen AEM as a Cloud Service-Umgebungen deutlich zu verbessern, wird die Aktivierung der Funktion „Dynamische Gruppenmitgliedschaft“ in neuen Umgebungen empfohlen.
Dies ist auch ein notwendiger Schritt bei der Aktivierung der Datensynchronisation. Weitere Informationen dazu finden Sie [hier](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier).
Fügen Sie dazu der OSGi-Konfigurationsdatei die folgende Eigenschaft hinzu:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Mit dieser Konfiguration werden Benutzende und Gruppen als [externe Oak-Benutzende](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html) erstellt. In AEM haben externe Benutzende und Gruppen standardmäßig einen `rep:principalName`, bestehend aus `[user name];[idp]` oder `[group name];[idp]`.
Beachten Sie, dass Zugriffssteuerungslisten (ACL) mit dem PrincipalName von Benutzenden oder Gruppen verknüpft sind.
Wenn diese Konfiguration in einer bestehenden Bereitstellung eingesetzt wird, in der zuvor `identitySyncType` nicht angegeben oder auf `default` festgelegt wurde, werden neue Benutzende und Gruppen erstellt und die ACL muss auf diese neuen Benutzenden und Gruppen angewendet werden. Beachten Sie, dass externe Gruppen keine lokalen Benutzenden enthalten können. [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html) kann verwendet werden, um ACL für externe SAML-Gruppen zu erstellen, wobei diese erst erstellt werden, wenn die Benutzenden eine Anmeldung durchführen.
Um diese Umgestaltung der ACL zu vermeiden, wurde eine [Standard-Migrationsfunktion](#automatic-migration-to-dynamic-group-membership-for-existing-environments) implementiert.

### Speicherung von Mitgliedschaften in lokalen und externen Gruppen mit dynamischer Gruppenmitgliedschaft

Bei lokalen Gruppen werden die Gruppenmitglieder im folgenden Oak-Attribut gespeichert: `rep:members`. Das Attribut enthält die Liste der UID jedes Gruppenmitglieds. Weitere Informationen finden Sie [hier](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
Zum Beispiel:

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

Externe Gruppen mit dynamischer Gruppenmitgliedschaft speichern keine Mitglieder im Gruppeneintrag.
Die Gruppenmitgliedschaft wird stattdessen in den Benutzereinträgen gespeichert. Weitere Einzelheiten finden Sie [hier](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). Dies ist beispielsweise der OAK-Knoten für die Gruppe:

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

Dies ist der Knoten für ein Benutzermitglied dieser Gruppe:

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### Aktivieren der dynamischen Gruppenmitgliedschaft für SAML-Benutzer in vorhandenen Umgebungen

Wie im vorherigen Abschnitt erläutert, unterscheidet sich das Format externer Benutzer und Gruppen geringfügig von dem für lokale Benutzer und Gruppen verwendeten. Es ist möglich, eine neue ACL für externe Gruppen zu definieren und neue externe Benutzer bereitzustellen, oder das Migrations-Tool wie unten beschrieben zu verwenden.

#### Aktivieren der dynamischen Gruppenmitgliedschaft für bestehende Umgebungen mit externen Benutzern

Der SAML-Authentifizierungs-Handler erstellt externe Benutzer, wenn die folgende Eigenschaft angegeben wird: `"identitySyncType": "idp"`. In diesem Fall kann die dynamische Gruppenmitgliedschaft aktiviert werden, indem diese Eigenschaft wie folgt geändert wird: `"identitySyncType": "idp_dynamic"`. Es ist keine Migration erforderlich.

#### Automatische Migration zu einer dynamischen Gruppenmitgliedschaft für bestehende Umgebungen mit lokalen Benutzern

Der SAML-Authentifizierungs-Handler erstellt lokale Benutzer, wenn die folgende Eigenschaft angegeben ist: `"identitySyncType": "default"`. Dies ist auch der Standardwert, wenn die Eigenschaft nicht angegeben ist. In diesem Abschnitt werden die Schritte beschrieben, die mit dem automatischen Migrationsprozess durchgeführt werden.

Wenn diese Migration aktiviert ist, wird sie während der Benutzerauthentifizierung durchgeführt und umfasst die folgenden Schritte:
1. Die lokalen Benutzenden werden zu externen Benutzenden migriert, wobei der ursprüngliche Benutzername beibehalten wird. Dies bedeutet, dass migrierte lokale Benutzende, die nun als externe Benutzende agieren, ihren ursprünglichen Benutzernamen behalten, anstatt der im vorherigen Abschnitt erwähnten Namenssyntax zu folgen. Eine weitere Eigenschaft wird hinzugefügt, nämlich `rep:externalId` mit dem Wert `[user name];[idp]`. Der `PrincipalName` dieser Person wird nicht geändert.
2. Für jede externe Gruppe, die in der SAML-Zusicherung empfangen wird, wird eine externe Gruppe erstellt. Wenn eine entsprechende lokale Gruppe vorhanden ist, wird die externe Gruppe der lokalen Gruppe als Mitglied hinzugefügt.
3. Die Benutzerin bzw. der Benutzer wird als Mitglied der externen Gruppe hinzugefügt.
4. Die lokale Benutzerin bzw. der lokale Benutzer wird dann aus allen lokalen SAML-Gruppen entfernt, in denen sie bzw. er Mitglied war. Lokale SAML-Gruppen werden durch die folgende OAK-Eigenschaft identifiziert: `rep:managedByIdp`. Diese Eigenschaft wird vom SAML-Authentifizierungs-Handler festgelegt, wenn das Attribut `syncType` nicht angegeben oder auf `default` festgelegt ist.

Wenn beispielsweise vor der Migration `user1` eine lokale Benutzerin bzw. ein lokaler Benutzer und Mitglied der lokalen Gruppe `group1` ist, treten nach der Migration die folgenden Änderungen auf:
`user1` wird zu einer bzw. einem externen Benutzenden. Das Attribut `rep:externalId` wird dem Profil dieser Person hinzugefügt.
`user1` wird Mitglied der externen Gruppe: `group1;idp`
`user1` ist nicht mehr direktes Mitglied der lokalen Gruppe: `group1`
`group1;idp` ist Mitglied der lokalen Gruppe: `group1`.
`user1` ist durch Vererbung dann ein Mitglied der lokalen Gruppe: `group1`

Die Gruppenmitgliedschaft für externe Gruppen wird im Benutzerprofil in der Eigenschaft `rep:externalPrincipalNames` gespeichert

### Konfigurieren der automatischen Migration zu einer dynamischen Gruppenmitgliedschaft

1. Aktivieren Sie die Eigenschaft `"identitySyncType": "idp_dynamic_simplified_id"` in der SAML-OSGi-Konfigurationsdatei: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` :
2. Konfigurieren Sie den neuen OSGi-Dienst mit werkseitiger PID, beginnend mit: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`. Eine PID kann beispielsweise sein: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`. Legen Sie die folgende Eigenschaft fest:

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

Um mehrere SAML-Konfigurationen zu migrieren, müssen mehrere OSGi-Werkskonfigurationen für `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration` erstellt werden, wobei jede eine zu migrierende `idpIdentifier` angibt.

## Bereitstellen der SAML-Konfiguration

Die OSGi-Konfigurationen müssen an Git übertragen und mithilfe von Cloud Manager für AEM as a Cloud Service bereitgestellt werden.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Stellen Sie die Git-Verzweigung von Cloud Manager (in diesem Beispiel `develop`) mithilfe einer Full-Stack-Implementierungs-Pipeline bereit.

## Aufrufen der SAML-Authentifizierung

Der SAML-Authentifizierungsfluss kann von einer AEM Site-Web-Seite aus aufgerufen werden, indem ein speziell gestalteter Link oder eine Schaltfläche erstellt wird. Die unten beschriebenen Parameter können bei Bedarf programmgesteuert festgelegt werden. Beispielsweise kann eine Anmeldeschaltfläche den `saml_request_path`, der die Person nach erfolgreicher SAML-Authentifizierung auf verschiedene AEM-Seiten führt, je nach Kontext der Schaltfläche auf verschiedene Seiten weiterleiten.

## Sicheres Caching bei SAMLVerwendung

In der AEM-Veröffentlichungsinstanz werden die meisten Seiten normalerweise im Cache gespeichert. Bei SAML-geschützten Pfaden sollte jedoch das Caching entweder deaktiviert oder sicheres Caching über die auth_checker-Konfiguration aktiviert werden. Weitere Informationen finden Sie [hier](https://experienceleague.adobe.com/de/docs/experience-manager-dispatcher/using/configuring/permissions-cache).

Beachten Sie, dass es beim Caching geschützter Pfade ohne auth_checker-Aktivierung zu unvorhersehbarem Verhalten kommen kann.

### GET-Anfrage

Die SAML-Authentifizierung kann durch Erstellen einer HTTP-GET-Anfrage im folgenden Format aufgerufen werden:

`HTTP GET /system/sling/login`

und durch Bereitstellung von Abfrageparametern:

| Name des Abfrageparameters | Wert des Abfrageparameters |
|----------------------|-----------------------|
| `resource` | Jeder JCR-Pfad oder Unterpfad, den der SAML-Authentifizierungs-Handler überwacht, wie in der `path`-Eigenschaft der [OSGi-Konfiguration des Adobe Granite SAML 2.0 Authentifizierungs-Handlers](#configure-saml-2-0-authentication-handler) definiert. |
| `saml_request_path` | Der URL-Pfad, zu dem die Person nach erfolgreicher SAML-Authentifizierung weitergeleitet werden soll. |

Dieser HTML-Link löst beispielsweise den SAML-Anmeldevorgang aus und leitet die Person bei Erfolg zu `/content/wknd/us/en/protected/page.html`. Diese Abfrageparameter können bei Bedarf programmgesteuert festgelegt werden.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## POST-Anfrage

Die SAML-Authentifizierung kann durch Erstellen einer HTTP-POST-Anfrage im folgenden Format aufgerufen werden:

`HTTP POST /system/sling/login`

und durch Angeben der Formulardaten:

| Formulardatenname | Formulardatenwert |
|----------------------|-----------------------|
| `resource` | Jeder JCR-Pfad oder Unterpfad, den der SAML-Authentifizierungs-Handler überwacht, wie in der `path`-Eigenschaft der [OSGi-Konfiguration des Adobe Granite SAML 2.0 Authentifizierungs-Handlers](#configure-saml-2-0-authentication-handler) definiert. |
| `saml_request_path` | Der URL-Pfad, zu dem die Person nach erfolgreicher SAML-Authentifizierung weitergeleitet werden soll. |


Diese HTML-Schaltfläche verwendet beispielsweise einen HTTP-POST, um den SAML-Anmeldevorgang auszulösen und die Person bei Erfolg zu `/content/wknd/us/en/protected/page.html` weiterzuleiten. Diese Formulardatenparameter können je nach Bedarf programmgesteuert festgelegt werden.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Dispatcher-Konfiguration

Sowohl die HTTP-GET- als auch die POST-Methode erfordern einen Client-Zugriff auf die `/system/sling/login`-Endpunkte von AEM und müssen daher über den AEM Dispatcher zugelassen werden.

Zulassen der erforderlichen URL-Muster, je nachdem, ob GET oder POST verwendet wird

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
