---
title: Benutzerdefinierter SAML 2.0-Anmelde-Hook
description: Erfahren Sie, wie Sie einen benutzerdefinierten SAML 2.0-Anmelde-Hook für AEM entwickeln.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# SAML 2.0-Anmelde-Hook

Erfahren Sie, wie Sie einen benutzerdefinierten SAML 2.0-Anmelde-Hook für AEM entwickeln. Dieses Tutorial enthält Schritt-für-Schritt-Anweisungen zum Erstellen eines benutzerdefinierten Anmelde-Hooks, der mit einem SAML 2.0-Identitätsanbieter integriert ist, sodass sich Benutzer mithilfe ihrer SAML-Anmeldeinformationen authentifizieren können.

Wenn der IDP die Benutzerprofildaten und die Benutzergruppenmitgliedschaft nicht in der SAML-Assertion senden kann oder die Daten vor der Synchronisierung mit AEM transformiert werden müssen, können benutzerdefinierte SAML-Hooks implementiert werden, um den SAML-Authentifizierungsprozess zu erweitern. SAML-Hooks ermöglichen die Anpassung der Gruppenzugehörigkeitszuweisung, die Änderung von Benutzerprofilattributen und das Hinzufügen benutzerdefinierter Geschäftslogik während des Authentifizierungsflusses.

>[!NOTE]
>Benutzerdefinierte SAML-Hooks werden für **AEM as a Cloud Service** und **AEM LTS** unterstützt. Diese Funktion ist in älteren AEM-Versionen nicht verfügbar.

## Häufige Anwendungsfälle

Benutzerdefinierte SAML-Hooks sind nützlich, wenn Folgendes erforderlich ist:

+ Dynamische Zuweisung der Gruppenmitgliedschaft basierend auf benutzerdefinierter Geschäftslogik über die in SAML-Assertionen bereitgestellten hinaus
+ Benutzerprofildaten vor der Synchronisierung mit AEM transformieren oder anreichern
+ Zuordnen komplexer SAML-Attributstrukturen zu AEM-Benutzereigenschaften
+ Implementieren benutzerdefinierter Autorisierungsregeln oder bedingter Gruppenzuweisungen
+ Hinzufügen benutzerdefinierter Protokollierung oder Prüfung während der SAML-Authentifizierung
+ Integration mit externen Systemen während des Authentifizierungsprozesses

## `SamlHook` OSGi-Service-Schnittstelle

Die `com.adobe.granite.auth.saml.spi.SamlHook`-Schnittstelle bietet zwei Hook-Methoden, die in verschiedenen Phasen des SAML-Authentifizierungsprozesses aufgerufen werden:

### `postSamlValidationProcess()`

Diese Methode wird als **after** bezeichnet, nachdem die SAML-Antwort validiert wurde, jedoch **bevor** der Prozess der Benutzersynchronisierung gestartet wird. Dies ist der ideale Ort, um die SAML-Assertionsdaten zu ändern, z. B. um Attribute hinzuzufügen oder zu transformieren.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### Anwendungsfälle

+ Hinzufügen zusätzlicher Gruppenmitgliedschaften zur Bestätigung
+ Transformiert Attributwerte vor der Synchronisierung
+ Assertion mit Daten aus externen Quellen anreichern
+ Validieren benutzerdefinierter Geschäftsregeln

### `postSyncUserProcess()`

Diese Methode wird **nach** dem Abschluss des Benutzersynchronisierungsprozesses aufgerufen. Dieser Hook kann verwendet werden, um zusätzliche Vorgänge auszuführen, nachdem der AEM-Benutzer erstellt oder aktualisiert wurde.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### Anwendungsfälle

+ Aktualisieren zusätzlicher Benutzerprofileigenschaften, die nicht von der Standardsynchronisierung abgedeckt werden
+ Erstellen oder Aktualisieren benutzerdefinierter benutzerbezogener Ressourcen in AEM
+ Trigger-Workflows oder -Benachrichtigungen nach der Benutzerauthentifizierung
+ Benutzerdefinierte Authentifizierungsereignisse protokollieren

**Wichtig:** Um Benutzereigenschaften im Repository zu ändern, ist für die Hook-Implementierung Folgendes erforderlich:

+ Eine `SlingRepository` Referenz, die über `@Reference` eingefügt wird
+ Ein konfigurierter [Dienstbenutzer](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) mit entsprechenden Berechtigungen (konfiguriert in „Apache Sling Service User Mapper Service Amendment„)
+ Richtige Sitzungsverwaltung mit try-catch-finally-Blöcken

## Implementieren eines benutzerdefinierten SAML-Hooks

Die folgenden Schritte beschreiben, wie Sie einen benutzerdefinierten SAML-Hook erstellen und bereitstellen.

### Erstellen der SAML-Hook-Implementierung

Erstellen Sie eine neue Java-Klasse im AEM-Projekt, die die `com.adobe.granite.auth.saml.spi.SamlHook` implementiert:

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### SAML-Hook konfigurieren

Der SAML-Hook verwendet die OSGi-Konfiguration, um anzugeben, für welchen IDP er gelten soll. Erstellen Sie eine OSGi-Konfigurationsdatei im Projekt unter:

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

Der `idpIdentifier` muss mit dem `idpIdentifier` übereinstimmen, der in der entsprechenden OSGi-Werkskonfiguration für den SAML-Authentifizierungs-Handler konfiguriert ist (PID: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`). Diese Zuordnung ist wichtig: Der SAML-Hook wird nur für die SAML-Authentifizierungs-Handler-Instanz aufgerufen, die denselben `idpIdentifier` hat. Der SAML-Authentifizierungs-Handler ist eine Werkskonfiguration, d. h. Sie können über mehrere Instanzen verfügen (z. B. `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`), und jeder Hook ist über die `idpIdentifier` an einen bestimmten Handler gebunden. Die `service.ranking`-Eigenschaft steuert die Ausführungsreihenfolge, wenn mehrere Hooks konfiguriert sind (höhere Werte werden zuerst ausgeführt).

### Hinzufügen von Maven-Abhängigkeiten

Fügen Sie die erforderliche SAML-SPI-Abhängigkeit zur `pom.xml` des AEM Maven-Kernprojekts hinzu.

**Verwenden Sie für AEM as a Cloud Service** Projekte die AEM SDK-API-Abhängigkeit, die die SAML-Schnittstellen enthält:

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

Das `aem-sdk-api`-Artefakt enthält alle erforderlichen Adobe Granite SAML-Schnittstellen einschließlich `com.adobe.granite.auth.saml.spi.SamlHook`.

### Service-Benutzer konfigurieren (optional)

Wenn der SAML-Hook Inhalte im JCR-Repository von AEM ändern muss, z. B. Benutzereigenschaften (wie im `postSyncUserProcess` Beispiel gezeigt), muss ein [Dienstbenutzer](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) konfiguriert werden:

1. Erstellen Sie eine Service-Benutzerzuordnung im Projekt unter `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`:

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. Erstellen Sie ein repoinit-Skript, um den Dienstbenutzer und die Berechtigungen unter `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json` zu definieren:

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

Dadurch erhält der Service-Benutzer Berechtigungen zum Lesen und Ändern von Benutzereigenschaften im Repository.

### Bereitstellen in AEM

Bereitstellen des benutzerdefinierten SAML-Hooks für AEM as a Cloud Service:

1. Erstellen des AEM-Projekts
1. Übertragen Sie den Code in das Cloud Manager-Git-Repository
1. Bereitstellen mithilfe einer Full-Stack-Bereitstellungs-Pipeline
1. Der SAML-Hook wird automatisch aktiviert, wenn sich ein Benutzer über SAML authentifiziert


### Wichtige Überlegungen

+ **IDP-Kennungsabgleich**: Die im SAML-Hook konfigurierten `idpIdentifier` müssen genau mit der `idpIdentifier` in der Werkskonfiguration für den SAML-Authentifizierungs-Handler (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`) übereinstimmen
+ **Attributnamen**: Stellen Sie sicher, dass die Attributnamen, auf die im Hook verwiesen wird (z. B. `groupMembership`), mit den Attributen übereinstimmen, die im SAML-Authentifizierungs-Handler konfiguriert sind
+ **Leistung**: Halten Sie Hook-Implementierungen so einfach wie möglich, da sie bei jeder SAML-Authentifizierung ausgeführt werden
+ **Fehlerbehandlung**: SAML-Hook-Implementierungen sollten `com.adobe.granite.auth.saml.spi.SamlHookException` auslösen, wenn kritische Fehler auftreten, die bei der Authentifizierung fehlschlagen sollten. Der SAML-Authentifizierungs-Handler erfasst diese Ausnahmen und gibt `AuthenticationInfo.FAIL_AUTH` zurück. Fangen Sie bei Repository-Vorgängen immer `RepositoryException` und protokollieren Sie Fehler entsprechend. Verwenden Sie try-catch-finally-Blöcke, um eine ordnungsgemäße Bereinigung der Ressourcen sicherzustellen
+ **Testen**: Testen Sie benutzerdefinierte Hooks gründlich in niedrigeren Umgebungen, bevor Sie sie in der Produktion bereitstellen
+ **Mehrere Hooks**: Es können mehrere SAML-Hook-Implementierungen konfiguriert werden; alle übereinstimmenden Hooks werden ausgeführt. Verwenden Sie die `service.ranking`-Eigenschaft in der OSGi-Komponente, um die Ausführungsreihenfolge zu steuern (höhere Rangwerte werden zuerst ausgeführt). Um einen SAML-Hook über mehrere SAML Authentication Handler Factory-Konfigurationen (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`) hinweg wiederzuverwenden, erstellen Sie mehrere Hook-Konfigurationen (OSGi-Factory-Konfigurationen) mit jeweils unterschiedlichen `idpIdentifier`, die dem entsprechenden SAML Authentication Handler entsprechen
+ **Sicherheit**: Überprüfen und bereinigen Sie alle Daten aus SAML-Assertionen, bevor Sie sie in der Geschäftslogik verwenden
+ **Repository-Zugriff**: Verwenden Sie beim Ändern von Benutzereigenschaften in `postSyncUserProcess` immer einen [Service-Benutzer](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) mit entsprechenden Berechtigungen anstelle von Verwaltungssitzungen
+ **Service-Benutzerberechtigungen**: Gewähren minimaler erforderlicher Berechtigungen für den [Service-Benutzer](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) (z. B. nur `jcr:read` und `rep:write` auf `/home/users`, keine vollständigen Administratorrechte)
+ **Sitzungsverwaltung**: Verwenden Sie immer try-catch-finally-Blöcke, um sicherzustellen, dass Repository-Sitzungen ordnungsgemäß geschlossen werden, auch wenn Ausnahmen auftreten
+ **Zeitpunkt der Benutzersynchronisierung**: Der `postSyncUserProcess`-Hook wird ausgeführt, nachdem der Benutzer mit OAK synchronisiert wurde. Das Benutzerobjekt ist also zu diesem Zeitpunkt garantiert im Repository vorhanden
