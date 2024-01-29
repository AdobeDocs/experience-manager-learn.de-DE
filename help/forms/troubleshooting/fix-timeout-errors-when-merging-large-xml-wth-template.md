---
title: Beheben von Timeout-Fehlern beim Zusammenführen großer XML-Datendateien mit einer XDP-Vorlage
description: Zusammenführen großer XML-Dateien mit Vorlagen in AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 45
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '178'
ht-degree: 100%

---

# Aktivieren der Erstellung von PDF-Dateien durch Zusammenführen großer XML-Datendateien mit XDP-Vorlagen

Beim Zusammenführen großer XML-Datendateien mit einer XDP-Vorlage wird möglicherweise der folgende Fehler in der Protokolldatei angezeigt:

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Gehen Sie wie folgt vor, um den oben genannten Fehler zu beheben:

## Ändern des Aries-Timeouts

* Beenden Sie den AEM-Server
* Erstellen Sie einen Ordner namens **install** unter dem Ordner „crx-quickstart“ Ihrer AEM-Installation
* Erstellen Sie eine Datei namens **org.apache.aries.transaction.config** mit folgendem Inhalt
aries.transaction.timeout=&quot;1200&quot;
unter dem install-Ordner. Sie können den Timeout-Wert entsprechend Ihren Anforderungen ändern. Der Timeout-Wert wird in Sekunden angegeben.

>[!NOTE]
> Sobald Sie die org.apache.aries.transaction-Konfiguration erstellt haben, können Sie die Transaktions-Timeout-Werte über [configMgr](http://localhost:4502/system/console/configMgr) bearbeiten, anstatt die Datei selbst zu bearbeiten.


## Ändern der Jacorb ORB Provider-Einstellungen

* [Öffnen Sie die OSGi „ConfigMgr“](http://localhost:4502/system/console/configMgr)
* Suchen Sie nach **Jacorb ORB Provider**
* Fügen Sie folgenden Eintrag hinzu:
jacorb.connection.client.pending_reply_timeout=600000
Die obige Einstellung setzt den Timeout für ausstehende Antworten (auch bekannt als CORBA-Client-Timeout) auf 600 Sekunden.
* Speichern Sie Ihre Änderungen
