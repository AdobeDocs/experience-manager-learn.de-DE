---
title: SQL-Verbindungen mit Java™-APIs
description: Erfahren Sie, wie Sie von AEM as a Cloud Service mithilfe von Java™-SQL-APIs und Ausgangs-Ports eine Verbindung zu SQL-Datenbanken herstellen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9356
thumbnail: KT-9356.jpeg
exl-id: ec9d37cb-70b6-4414-a92b-3b84b3f458ab
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: ht
source-wordcount: '305'
ht-degree: 100%

---

# SQL-Verbindungen mit Java™-APIs

Verbindungen zu SQL-Datenbanken (und anderen Nicht-HTTP/HTTPS-Diensten) müssen aus AEM herausgeleitet werden.

Die Ausnahme von dieser Regel ist, wenn eine [dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) verwendet wird und sich der Dienst auf Adobe oder Azure befindet.

## Erweiterte Netzwerkunterstützung

Das folgende Code-Beispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

Stellen Sie sicher, dass vor diesem Tutorial die [geeignete](../advanced-networking.md#advanced-networking) erweiterte Netzwerkkonfiguration eingerichtet wurde.

| Keine erweiterten Netzwerkfunktionen | [Flexibler Port-Ausgang](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi-Konfiguration

Da Geheimnisse nicht im Code gespeichert werden dürfen, sollten Benutzername und Kennwort der SQL-Verbindung am besten über [geheime OSGi-Konfigurationsvariablen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#secret-configuration-values) bereitgestellt werden. Festgelegt werden diese mit der AIO-CLI oder Cloud Manager-APIs.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

Der folgende `aio CLI`-Befehl kann verwendet werden, um die OSGi-Geheimnisse für die einzelnen Umgebungen festzulegen:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Code-Beispiel

Dieses Java™-Code-Beispiel bezieht sich auf einen OSGi-Dienst, der über die folgende Cloud Manager-`portForwards`-Regel des Vorgangs [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) eine Verbindung zu einem externen SQL Server-Webserver herstellt.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/MySqlExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.sql.*;

@Component
@Designate(ocd = MySqlExternalServiceImpl.Config.class)
public class MySqlExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(MySqlExternalServiceImpl.class);

    // Get the proxy host using the AEM_PROXY_HOST Java environment variable provided by AEM as a Cloud Service
    private static final String PROXY_HOST = System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel");

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = 30001;

    private Config config;

    @Override
    public boolean isAccessible() {
        log.debug("MySQL connection URL: [ {} ]", getConnectionUrl(config));

        // Establish a connection with the external MySQL service
        // This MySQL connection URL is created in getConnection(..) which will use the AEM_PROXY_HOST is it exists, and the proxied port.
        try (Connection connection = DriverManager.getConnection(getConnectionUrl(config), config.username(), config.password())) {
            // Validate the connection
            connection.isValid(1000);

            // Close the connection, since this is just a simple connectivity check
            connection.close();

            // Return true if AEM could reach the external SQL service
            return true;
        } catch (SQLException e) {
            log.error("Unable to establish an connection with MySQL service using connection URL  [ {} ]", getConnectionUrl(config), e);
        }

        return false;
    }

    /**
     * Create a connection string to the MySQL using the AEM-provided AEM_PROXY_HOST and portForwards.portOrg port
     * defined in the Cloud Manager API mapping.
     *
     * @param config OSGi configuration object
     * @return the MySQL connection URI
     */
    private String getConnectionUrl(Config config) {
        return String.format("jdbc:mysql://%s:%d/wknd-examples", PROXY_HOST, PORT_FORWARDS_PORT_ORIG);
    }

    @Activate
    protected void activate(Config config) throws ClassNotFoundException, SQLException {
        this.config = config;

        // Load the required MySQL Driver class required for Java to make the connection
        // The OSGi bundle that contains this driver is deployed via the project's all project
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
    }

    @ObjectClassDefinition
    @interface Config {
        @AttributeDefinition(type = AttributeType.STRING)
        String username();

        @AttributeDefinition(type = AttributeType.STRING)
        String password();
    }
}
```

## Abhängigkeiten von MySQL-Treibern

Bei AEM as a Cloud Service müssen Sie oft Java™-Datenbanktreiber zur Unterstützung der Verbindungen bereitstellen. Die Bereitstellung der Treiber wird in der Regel am besten dadurch erreicht, dass die OSGi-Bundle-Artefakte, die diese Treiber enthalten, über das `all`-Paket in das AEM-Projekt eingebettet werden.

### Reactor pom.xml

Schließen Sie die Datenbanktreiber-Abhängigkeiten in den Reaktor `pom.xml` ein und referenzieren Sie sie dann in den `all`-Unterprojekten.

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## All pom.xml

Betten Sie die Abhängigkeitsartefakte des Datenbanktreibers in das `all`-Paket ein, damit sie bereitgestellt werden und in AEM as a Cloud Service verfügbar sind. Diese Artefakte __müssen__ OSGi-Bundles sein, mit denen die Datenbanktreiber-Java™-Klasse exportiert wird.

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
