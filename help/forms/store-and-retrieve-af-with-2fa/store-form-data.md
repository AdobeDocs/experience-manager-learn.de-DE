---
title: Speichern von Formulardaten
description: Speichern von Formulardaten zusammen mit der neuen Anhangszuordnung in der Datenbank
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6538
thumbnail: 6538.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 2bd9fe63-8f42-4b89-95a0-13ade49bc31b
duration: 67
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 100%

---

# Speichern von Formulardaten

Der nächste Schritt besteht darin, einen Dienst zum Einfügen einer neuen Zeile in die Datenbank zu erstellen und darüber die Daten des adaptiven Formulars und die zugehörigen Anhangsinformationen (attachmentsinfo) zu speichern.
Der folgende Screenshot zeigt eine Zeile in der Datenbank.


![Beispielzeile](assets/sample-row.JPG)


Mit dem folgenden Code wird eine neue Zeile mit den entsprechenden Daten in die Datenbank eingefügt.

```java
public String storeFormData(String formData, String attachmentsInfo, String telephoneNumber) {
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData + "and the telephone number to insert is  " + telephoneNumber);
    String insertRowSQL = "INSERT INTO aemformstutorial.formdatawithattachments(guid,afdata,attachmentsInfo,telephoneNumber) VALUES(?,?,?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
        pstmt = null;
        pstmt = c.prepareStatement(insertRowSQL);
        pstmt.setString(1, randomUUIDString);
        pstmt.setString(2, formData);
        pstmt.setString(3, attachmentsInfo);
        pstmt.setString(4, telephoneNumber);
        log.debug("Executing the insert statment  " + pstmt.executeUpdate());
        c.commit();
    } catch (SQLException e) {

        log.error("unable to insert data in the table", e.getMessage());
    } finally {
        if (pstmt != null) {
            try {
                pstmt.close();
            } catch (SQLException e) {
                log.debug("error in closing prepared statement " + e.getMessage());
            }
        }
        if (c != null) {
            try {
                c.close();
            } catch (SQLException e) {
                log.debug("error in closing connection " + e.getMessage());
            }
        }
    }
    return randomUUIDString;
}
```

## Nächste Schritte

[Implementieren einer Schaltfläche zum Speichern und Beenden](./create-servlet.md)

