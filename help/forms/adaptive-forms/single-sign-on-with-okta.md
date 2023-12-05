---
title: Konfigurieren von OKTA mit AEM
description: Grundlegendes zu den verschiedenen Konfigurationseinstellungen zur Verwendung von Single Sign-on mit OKTA.
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
duration: 203
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '733'
ht-degree: 100%

---

# Authentifizieren bei AEM Author mithilfe von OKTA

Der erste Schritt besteht darin, Ihre App im OKTA-Portal zu konfigurieren. Sobald Ihre App von Ihrem OKTA-Admin-Team genehmigt wurde, haben Sie Zugriff auf das IdP-Zertifikat und die Single-Sign-on-URL. Im Folgenden finden Sie die Einstellungen, die normalerweise für die Registrierung neuer Anwendungen verwendet werden.

* **Anwendungsname:** Dies ist der Name Ihrer Anwendung. Stellen Sie sicher, dass Sie Ihrer Anwendung einen eindeutigen Namen geben.
* **SAML-Empfänger:** Nach Authentifizierung durch OKTA ist dies die URL, die in Ihrer AEM-Instanz mit der SAML-Antwort zu finden ist. Der SAML-Authentifizierungs-Handler fängt normalerweise alle URLs mit „/ saml_login“ ab. Es empfiehlt sich jedoch, sie nach Ihrem Anwendungsstamm anzuhängen.
* **SAML-Zielgruppe**: Dies ist die Domain-URL Ihrer Anwendung. Verwenden Sie kein Protokoll (http oder https) in der Domain-URL.
* **SAML-Namens-ID:** Wählen Sie in der Dropdown-Liste die E-Mail-Option aus.
* **Umgebung**: Wählen Sie die gewünschte Umgebung aus.
* **Attribute**: Dies sind die Attribute, die Sie über die Benutzerin oder den Benutzer in der SAML-Antwort erhalten. Legen Sie diese entsprechend Ihren Anforderungen fest.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Hinzufügen des OKTA-Zertifikats (IdP) zum AEM Trust Store

Da SAML-Assertionen verschlüsselt sind, müssen wir das IdP-Zertifikat (OKTA) zum AEM Trust Store hinzufügen, um eine sichere Kommunikation zwischen OKTA und AEM zu ermöglichen.
[Initialisieren Sie den Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html), falls noch nicht geschehen.
Merken Sie sich das Kennwort für den Trust Store. Wir benötigen dieses Kennwort noch an späterer Stelle.

* Navigieren Sie zu [Global Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html).
* Klicken Sie auf „Zertifikat aus CER-Datei hinzufügen“. Fügen Sie das von OKTA bereitgestellte IdP-Zertifikat hinzu und klicken Sie auf „Senden“.

  >[!NOTE]
  >
  >Weisen Sie das Zertifikat keiner Benutzerin bzw. keinem Benutzer zu.

Beim Hinzufügen des Zertifikats zum Trust Store sollten Sie den Zertifikatalias erhalten, wie im Screenshot unten dargestellt. Der Aliasname kann in Ihrem Fall anders sein.

![Certificate-alias](assets/cert-alias.PNG)

**Notieren Sie sich den Zertifikatalias. Sie benötigen diese Information später.**

### Konfigurieren des SAML-Authentifizierungs-Handlers

Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
Suchen und öffnen Sie „Adobe Granite SAML 2.0 Authentication Handler“.
Legen Sie die folgenden Eigenschaften fest, wie unten angegeben.
Die folgenden Schlüsseleigenschaften müssen angegeben werden:

* **Pfad**: Dies ist der Pfad, in dem der Authentifizierungs-Handler ausgelöst wird.
* **IDP-URL**: Dies ist Ihre von OKTA bereitgestellte IDP-URL.
* **IDP-Zertifikatalias**: Diesen Alias haben Sie erhalten, als Sie das IDP-Zertifikat zum AEM Trust Store hinzugefügt haben.
* **Entitäts-ID des Dienstanbieters**: Dies ist der Name Ihres AEM-Servers.
* **Keystore-Kennwort**: Dies ist das von Ihnen verwendete Trust Store-Kennwort.
* **Standardumleitung**: Dies ist die URL, zu der bei erfolgreicher Authentifizierung umgeleitet wird.
* **UserID-Attribut**: Dies ist die UID.
* **Verschlüsselung verwenden**: false.
* **CRX-Benutzende automatisch erstellen**: true
* **Zu Gruppen hinzufügen**: true
* **Standardgruppen**: oktausers (dies ist die Gruppe, zu der die Benutzenden hinzugefügt werden. Sie können jede bestehende Gruppe in AEM bereitstellen.)
* **NamedIDPolicy**: Hier werden Einschränkungen für die Namenskennung angegeben, die zur Darstellung des angeforderten Betreffs verwendet werden soll. Kopieren Sie die folgende hervorgehobene Zeichenfolge und fügen Sie sie ein: **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Synchronisierte Attribute**: Dies sind die Attribute, die aus der SAML-Assertion im AEM-Profil gespeichert werden

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Konfigurieren von Apache Sling Referrer Filter

Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
Suchen und öffnen Sie „Apache Sling Referrer Filter“. Legen Sie die folgenden Eigenschaften fest, wie unten angegeben:

* **Leere zulassen**: false
* **Hosts zulassen**: Host-Name des IDP (in Ihrem Fall anders)
* **Regexp-Host zulassen**: Dies ist der IDP-Host-Name (in Ihrem Fall anders)
Screenshot mit den Sling Referrer Filter-Eigenschaften

![referrer-filter](assets/okta-referrer.png)

#### Konfigurieren der DEBUG-Protokollierung für die OKTA-Integration

Beim Einrichten der OKTA-Integration in AEM kann es hilfreich sein, die DEBUG-Protokolle für den AEM-SAML-Authentifizierungs-Handler zu überprüfen. Um die Protokollebene auf DEBUG festzulegen, erstellen Sie eine neue Sling Logger-Konfiguration über die AEM-OSGi-Web-Konsole.

Denken Sie daran, diesen Logger in der Staging- und Produktionsumgebung zu entfernen oder zu deaktivieren, um das „Protokollrauschen“ (Log Noise) zu reduzieren.

Beim Einrichten der OKTA-Integration in AEM kann es hilfreich sein, DEBUG-Protokolle für den AEM-SAML-Authentifizierungs-Handler zu überprüfen. Um die Protokollebene auf DEBUG festzulegen, erstellen Sie eine neue Sling Logger-Konfiguration über die AEM-OSGi-Web-Konsole.
**Denken Sie daran, diesen Logger in der Staging- und Produktionsumgebung zu entfernen oder zu deaktivieren, um das „Protokollrauschen“ (Log Noise) zu reduzieren.**
* Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr)

* Suchen und öffnen Sie „Apache Sling Logging Logger Configuration“.
* Erstellen Sie einen Logger mit folgender Konfiguration:
   * **Protokollebene**: Debugging
   * **Protokolldatei**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Klicken Sie auf „Speichern“, um Ihre Einstellungen zu speichern.

#### Testen Ihrer OKTA-Konfiguration

Melden Sie sich bei Ihrer AEM-Instanz ab. Versuchen Sie, auf den Link zuzugreifen. Sie sollten OKTA SSO nun in Aktion sehen.
