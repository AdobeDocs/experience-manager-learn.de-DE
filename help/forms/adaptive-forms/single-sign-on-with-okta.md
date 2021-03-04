---
title: OKTA mit AEM konfigurieren
description: Verstehen Sie verschiedene Konfigurationseinstellungen für die Verwendung von Single Sign-On mit okta.
feature: Adaptive Formulare
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
topic: Administration
role: 'Administrator  '
level: Erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 3%

---


# Authentifizierung bei AEM Author mit OKTA

Der erste Schritt besteht darin, Ihre App im OKTA-Portal zu konfigurieren. Sobald Ihre App von Ihrem OKTA-Administrator genehmigt wurde, haben Sie Zugriff auf das IdP-Zertifikat und die Single Sign-On-URL. Die folgenden Einstellungen werden normalerweise bei der Registrierung neuer Anwendungen verwendet.

* **Anwendungsname:** Dies ist Ihr Anwendungsname. Vergewissern Sie sich, dass Sie der Anwendung einen eindeutigen Namen geben.
* **SAML-Empfänger:** Nach der Authentifizierung von OKTA ist dies die URL, die bei Ihrer AEM mit der SAML-Antwort getroffen würde. Der SAML-Authentifizierungs-Handler fängt normalerweise alle URLs mit / saml_login ab, es ist jedoch empfehlenswert, diese nach dem Anwendungsstamm anzuhängen.
* **SAML-Audience**: Dies ist die Domänen-URL Ihrer Anwendung. Verwenden Sie in der URL der Domäne kein Protokoll (http oder https).
* **SAML-Name-ID:** Wählen Sie E-Mail aus der Dropdown-Liste.
* **Umgebung**: Wählen Sie die gewünschte Umgebung aus.
* **Attribute**: Dies sind die Attribute, die Sie über den Benutzer in der SAML-Antwort erhalten. Geben Sie sie entsprechend Ihren Anforderungen an.


![okta-Anwendung](assets/okta-app-settings-blurred.PNG)


## hinzufügen das OKTA(IdP)-Zertifikat an den AEM Trust Store

Da SAML-Zusicherungen verschlüsselt sind, müssen wir das IdP-Zertifikat (OKTA) zum AEM Trust Store hinzufügen, um eine sichere Kommunikation zwischen OKTA und AEM zu ermöglichen.
[Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html) initialisieren, falls er nicht bereits initialisiert wurde.
Speichern Sie das Trust Store-Kennwort. Wir müssen dieses Kennwort später in diesem Prozess verwenden.

* Navigieren Sie zu [Globaler Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html).
* Klicken Sie auf &quot;Hinzufügen Zertifikat aus CER-Datei&quot;. hinzufügen Sie das von OKTA bereitgestellte IdP-Zertifikat und klicken Sie auf Senden.

   >[!NOTE]
   >
   >Bitte ordnen Sie das Zertifikat keinem Benutzer zu

Wenn Sie das Zertifikat zu Trust Store hinzufügen, erhalten Sie den Zertifikatalias, wie im Screenshot unten dargestellt. Der Aliasname kann in Ihrem Fall anders sein.

![Certificate-alias](assets/cert-alias.PNG)

**Notieren Sie sich den Zertifikatalias. Sie benötigen dies in den späteren Schritten.**

### SAML-Authentifizierungshandler konfigurieren

Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
Suchen und öffnen Sie &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Stellen Sie die folgenden Eigenschaften wie unten angegeben bereit
Die folgenden Schlüsseleigenschaften müssen angegeben werden:

* **path**  - Dies ist der Pfad, in dem der Authentifizierungs-Handler ausgelöst wird
* **IdP-URL**:Dies ist Ihre IdP-URL, die von OKTA bereitgestellt wird
* **IDP-Zertifikatalias**: Der Alias, den Sie erhalten haben, wenn Sie AEM Trust Store das IdP-Zertifikat hinzugefügt haben
* **Dienstleister-Entitäts-ID**: Dies ist der Name Ihres AEM
* **Kennwort des Schlüsselspeichers**: Dies ist das Kennwort für den Trust Store, das Sie verwendet haben
* **Standardumleitung**: Dies ist die URL, zu der bei erfolgreicher Authentifizierung umgeleitet werden soll
* **UserID-Attribut**:uid
* **Verschlüsselung** verwenden: false
* **CRX-Benutzer** automatisch erstellen: true
* **hinzufügen zu Gruppen**:true
* **Standardgruppen**:Interaktionsgruppen(Dies ist die Gruppe, der die Benutzer hinzugefügt werden. Sie können jede bestehende Gruppe innerhalb von AEM bereitstellen.)
* **NamedIDPopolicy**: Gibt Einschränkungen für die Benennungskennung an, die für die Darstellung des angeforderten Objekts verwendet werden soll. Kopieren Sie die folgende hervorgehobene Zeichenfolge **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress** und fügen Sie sie ein
* **Synchronisierte Attribute** : Dies sind die Attribute, die in der SAML-Zusicherung in AEM Profil gespeichert werden

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Apache Sling Werber-Filter konfigurieren

Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
Suchen und öffnen Sie &quot;Apache Sling Werber Filter&quot;. Legen Sie die folgenden Eigenschaften wie unten angegeben fest:

* **Leer** zulassen: true
* **Hosts** zulassen: Hostname von IdP (in Ihrem Fall anders)
* **Regexp-Host** zulassen: Hostname von IdP (in Ihrem Fall anders) Der Screenshot zu den Eigenschaften des Sling-Werber-Werbers

![Werber-Filter](assets/sling-referrer-filter.PNG)

#### DEBUG-Protokollierung für die OKTA-Integration konfigurieren

Beim Einrichten der OKTA-Integration auf AEM kann es hilfreich sein, die DEBUG-Protokolle für AEM SAML-Authentifizierungs-Handler zu überprüfen. Um die Protokollierungsstufe auf DEBUG festzulegen, erstellen Sie eine neue Sling Logger-Konfiguration über die AEM OSGi Web Console.

Denken Sie daran, diese Protokollfunktion auf der Bühne und in der Produktion zu entfernen oder zu deaktivieren, um das Protokollieren zu reduzieren.

Beim Einrichten der OKTA-Integration auf AEM kann es hilfreich sein, die DEBUG-Protokolle für AEM SAML-Authentifizierungs-Handler zu überprüfen. Um die Protokollierungsstufe auf DEBUG festzulegen, erstellen Sie eine neue Sling Logger-Konfiguration über die AEM OSGi Web Console.
**Denken Sie daran, diese Protokollfunktion auf der Bühne und in der Produktion zu entfernen oder zu deaktivieren, um das Protokollieren zu reduzieren.**
* Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr)

* Suchen und öffnen Sie &quot;Apache Sling Logging Logger Configuration&quot;.
* Erstellen Sie einen Logger mit folgender Konfiguration:
   * **Protokollierungsstufe**: Debuggen
   * **Protokolldatei**: logs/saml.log
   * **Protokollfunktion**: com.adobe.granite.auth.saml
* Klicken Sie auf Speichern, um Ihre Einstellungen zu speichern



#### OKTA-Konfiguration testen

Melden Sie sich bei Ihrer AEM ab. Versuchen Sie, auf den Link zuzugreifen. Sie sollten die OKTA SSO in Aktion sehen.
