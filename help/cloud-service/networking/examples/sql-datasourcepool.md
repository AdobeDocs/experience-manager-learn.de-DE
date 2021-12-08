---
title: SQL-Verbindungen mit JDBC DataSourcePool
description: Erfahren Sie, wie Sie über AEM JDBC DataSourcePool und Ausgangs-Ports von AEM as a Cloud Service eine Verbindung zu SQL-Datenbanken herstellen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9355
thumbnail: KT-9355.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---


# SQL-Verbindungen mit JDBC DataSourcePool

Verbindungen zu SQL-Datenbanken (und anderen Nicht-HTTP/HTTPS-Diensten) müssen AEM bereitgestellt werden, einschließlich derjenigen, die mit dem AEM DataSourcePool-OSGi-Dienst zur Verwaltung der Verbindungen hergestellt werden.

## Erweiterte Netzwerkunterstützung

Das folgende Codebeispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

| Kein erweitertes Netzwerk | [Flexibles Port-Egress](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ms | ms | ms |

## OSGi-Konfiguration

Die Verbindungszeichenfolge der OSGi-Konfiguration verwendet:

+ `AEM_PROXY_HOST` Wert über die [OSGi-Konfigurationsumgebungsvariable](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST]` als Host der Verbindung
+ `30001` , die `portOrig` Wert für die Cloud Manager-Anschlussvorschau-Zuordnung `30001` → `mysql.example.com:3306`

Da Geheimnisse nicht im Code gespeichert werden dürfen, sollten Benutzername und Kennwort der SQL-Verbindung am besten über OSGi-Konfigurationsvariablen bereitgestellt werden, die mithilfe von AIO CLI- oder Cloud Manager-APIs festgelegt werden.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

Folgendes `aio CLI` kann verwendet werden, um die OSGi-Geheimnisse für jede Umgebung festzulegen:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Codebeispiel

Dieses Java™-Codebeispiel ist ein OSGi-Dienst, der über AEM DataSourcePool-OSGi-Dienst eine Verbindung zu einer externen MySQL-Datenbank herstellt.
Die DataSourcePool-OSGi-Werkskonfiguration gibt wiederum einen Port (`30001`), die über die `portForwards` -Regel in [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) Betrieb zum externen Host und Port, `mysql.example.com:3306`.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
    }
}
```

## Abhängigkeiten von MySQL-Treibern

AEM as a Cloud Service erfordert oft, dass Sie Java™-Datenbanktreiber bereitstellen, um die Verbindungen zu unterstützen. Die Bereitstellung der Treiber wird in der Regel am besten dadurch erreicht, dass die OSGi-Bundle-Artefakte, die diese Treiber enthalten, über das AEM in das Projekt eingebettet werden. `all` Paket.

### Reactor pom.xml

Abhängigkeiten von Datenbanktreibern in den Reaktor einschließen `pom.xml` und referenzieren Sie sie dann im `all` Teilprojekte.

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

## Alle pom.xml

Betten Sie die Abhängigkeitsartefakte des Datenbanktreibers in `all` -Paket bereitgestellt werden und auf AEM as a Cloud Service verfügbar sind. Diese Artefakte __must__ sind OSGi-Bundles, die die Java™-Klasse des Datenbanktreibers exportieren.

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
