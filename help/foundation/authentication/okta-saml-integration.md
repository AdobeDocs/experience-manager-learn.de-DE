---
title: Konfigurieren von OKTA mit AEM
description: Verstehen Sie verschiedene Konfigurationseinstellungen für die Verwendung von Single Sign-on mit OKTA.
version: 6.5
topic: Integrations, Security, Administration
feature: Integrations
role: Admin
level: Experienced
jira: KT-12305
last-substantial-update: 2023-03-01T00:00:00Z
source-git-commit: de9377236016066cc62819f1c307aac82331a0b6
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 3%

---

# Authentifizierung bei AEM Author mithilfe von OKTA

> Siehe [SAML 2.0-Authentifizierung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/authentication/saml-2-0.html?lang=de) für Anweisungen zum Einrichten von OKTA mit AEM as a Cloud Service.

Der erste Schritt besteht darin, Ihre App im OKTA-Portal zu konfigurieren. Sobald Ihre App von Ihrem OKTA-Administrator genehmigt wurde, haben Sie Zugriff auf das IdP-Zertifikat und die Single Sign-on-URL. Im Folgenden finden Sie die Einstellungen, die normalerweise für die Registrierung neuer Anwendungen verwendet werden.

* **Anwendungsname:** Dies ist Ihr Anwendungsname. Vergewissern Sie sich, dass Sie Ihrer Anwendung einen eindeutigen Namen geben.
* **SAML-Empfänger:** Nach Authentifizierung durch OKTA ist dies die URL, die auf Ihrer AEM-Instanz mit der SAML-Antwort getroffen wird. Der SAML-Authentifizierungs-Handler fängt normalerweise alle URLs mit / saml_login ab. Es empfiehlt sich jedoch, sie nach Ihrem Anwendungsstamm anzuhängen.
* **SAML-Zielgruppe**: Dies ist die Domänen-URL Ihrer Anwendung. Verwenden Sie kein Protokoll (http oder https) in der Domänen-URL.
* **SAML-Name-ID:** Wählen Sie E-Mail aus der Dropdown-Liste aus.
* **Umgebung**: Wählen Sie die gewünschte Umgebung aus.
* **Attribute**: Dies sind die Attribute, die Sie über den Benutzer in der SAML-Antwort erhalten. Geben Sie sie entsprechend Ihren Anforderungen an.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Hinzufügen des OKTA-Zertifikats (IdP) zum AEM Trust Store

Da SAML-Assertionen verschlüsselt sind, müssen wir das IdP-Zertifikat (OKTA) zum AEM Trust Store hinzufügen, um eine sichere Kommunikation zwischen OKTA und AEM zu ermöglichen.
[Trust Store initialisieren](http://localhost:4502/libs/granite/security/content/truststore.html), falls nicht bereits initialisiert.
Merken Sie sich das Kennwort für den Trust Store. Wir müssen dieses Kennwort später in diesem Prozess verwenden.

* Navigieren Sie zu [Globaler Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html).
* Klicken Sie auf &quot;Zertifikat aus CER-Datei hinzufügen&quot;. Fügen Sie das von OKTA bereitgestellte IdP-Zertifikat hinzu und klicken Sie auf &quot;Senden&quot;.

   >[!NOTE]
   >
   >Weisen Sie das Zertifikat keinem Benutzer zu.

Beim Hinzufügen des Zertifikats zum Trust Store sollten Sie den Zertifikatalias wie im Screenshot unten dargestellt erhalten. Der Aliasname kann in Ihrem Fall anders sein.

![Certificate-alias](assets/cert-alias.PNG)

**Notieren Sie sich den Zertifikatalias. Sie benötigen dies in den späteren Schritten.**

### SAML-Authentifizierungs-Handler konfigurieren

Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
Suchen und öffnen Sie &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Geben Sie die folgenden Eigenschaften wie unten angegeben an. Die folgenden Schlüsseleigenschaften müssen angegeben werden:

* **path** - Dies ist der Pfad, in dem der Authentifizierungs-Handler ausgelöst wird.
* **IdP-URL**:Dies ist Ihre IdP-URL, die von OKTA bereitgestellt wird.
* **IDP-Zertifikatalias**:Dieser Alias, den Sie erhalten haben, als Sie das IdP-Zertifikat AEM Trust Store hinzugefügt haben
* **Entitäts-ID des Dienstanbieters**:Dies ist der Name Ihres AEM-Servers.
* **Kennwort für Key Store**:Dies ist das von Ihnen verwendete Trust Store-Kennwort.
* **Standardumleitung**:Dies ist die URL, zu der bei erfolgreicher Authentifizierung umgeleitet wird.
* **UserID-Attribut**:uid
* **Verschlüsselung verwenden**:false
* **CRX-Benutzer automatisch erstellen**:true
* **Zu Gruppen hinzufügen**:true
* **Standardgruppen**:okersInteraction(Dies ist die Gruppe, der die Benutzer hinzugefügt werden. Sie können jede bestehende Gruppe in AEM bereitstellen.)
* **NamedIDPolicy**: Gibt Einschränkungen für die Namenskennung an, die zur Darstellung des angeforderten Betreffs verwendet werden soll. Kopieren Sie die folgende hervorgehobene Zeichenfolge und fügen Sie sie ein **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Synchronisierte Attribute** - Dies sind die Attribute, die aus der SAML-Assertion in AEM Profil gespeichert werden.

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Konfigurieren des Apache Sling Referrer-Filters

Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr).
Suchen und öffnen Sie &quot;Apache Sling Referrer Filter&quot;. Legen Sie die folgenden Eigenschaften wie unten angegeben fest:

* **Leere erlauben**: false
* **Hosts zulassen**: Hostname des IdP (in Ihrem Fall anders)
* **Regexp-Host zulassen**: Hostname des IdP (in Ihrem Fall anders) Der Screenshot der Eigenschaften des Sling Referrer Filters Referrer

![referrer-filter](assets/okta-referrer.png)

#### Konfigurieren der DEBUG-Protokollierung für die OKTA-Integration

Beim Einrichten der OKTA-Integration in AEM kann es hilfreich sein, die DEBUG-Protokolle für AEM SAML-Authentifizierungs-Handler zu überprüfen. Um die Protokollebene auf DEBUG festzulegen, erstellen Sie eine neue Sling Logger-Konfiguration über die AEM OSGi-Web-Konsole.

Denken Sie daran, diese Protokollfunktion in der Staging- und Produktionsumgebung zu entfernen oder zu deaktivieren, um das Protokollrauschen zu reduzieren.

Beim Einrichten der OKTA-Integration in AEM kann es hilfreich sein, DEBUG-Protokolle für AEM SAML-Authentifizierungs-Handler zu überprüfen. Um die Protokollebene auf DEBUG festzulegen, erstellen Sie eine neue Sling Logger-Konfiguration über die AEM OSGi-Web-Konsole.
**Denken Sie daran, diese Protokollfunktion in der Staging- und Produktionsumgebung zu entfernen oder zu deaktivieren, um das Protokollrauschen zu reduzieren.**
* Navigieren Sie zu [configMgr](http://localhost:4502/system/console/configMgr)

* Suchen und öffnen Sie &quot;Apache Sling Logging Logger Configuration&quot;
* Erstellen Sie einen Logger mit folgender Konfiguration:
   * **Protokollebene**: Debuggen
   * **Protokolldatei**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Klicken Sie auf Speichern , um Ihre Einstellungen zu speichern

#### Testen der OKTA-Konfiguration

Melden Sie sich von Ihrer AEM-Instanz ab. Versuchen Sie, auf den Link zuzugreifen. Sie sollten die OKTA SSO in Aktion sehen.
