---
title: Speichern und Abrufen von Formulardaten aus der MySQL-Datenbank
description: Lernprogramm mit mehreren Teilen, um Sie durch die Schritte zum Speichern und Abrufen von Formulardaten zu führen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---


# OSGI-Dienst zum Abrufen von Daten erstellen

Der folgende Code wurde geschrieben, um die gespeicherten Daten des adaptiven Formulars abzurufen. Eine einfache Abfrage wird verwendet, um die mit einer bestimmten GUID verknüpften adaptiven Formulardaten abzurufen. Die abgerufenen Daten werden dann an die aufrufende Anwendung zurückgegeben. Dieselbe Datenquelle, die im ersten Schritt erstellt wurde, wird in diesem Code referenziert.


```java
package com.techmarketing.core.impl;

import com.techmarketing.core.AemFormsAndDB;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

@Component(
        service = AemFormsAndDB.class
)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
   
    @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
    private DataSource dataSource;

    @Override
    public String getData(String guid) {
        log.debug("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '" + guid + "'" + "";
            log.debug(" The query is " + query);
            ResultSet rs = st.executeQuery(query);
            while (rs.next()) {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    public Connection getConnection() {
        log.debug("Getting Connection ");
        Connection con = null;
        try {
            con = dataSource.getConnection();
            log.debug("got connection");
            return con;
        } catch (Exception e) {
            log.debug("not able to get connection ");
            e.printStackTrace();
        }
        return null;
    }
}
```

## Benutzeroberfläche

Im Folgenden finden Sie die verwendete Schnittstellendeklaration

```java
package com.techmarketing.core;

public interface AemFormsAndDB {
   public String getData(String guid);
}
```
