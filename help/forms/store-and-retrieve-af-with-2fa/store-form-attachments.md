---
title: Formularanlagen speichern
description: Extrahieren Sie die Formularanhänge und speichern Sie sie an einem neuen Speicherort im CRX-Repository.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6537
thumbnail: 6537.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Formularanlagen speichern

Wenn Sie Anlagen zu einem adaptiven Formular hinzufügen, werden die Anlagen an einem temporären Speicherort im CRX-Repository gespeichert. Damit unser Anwendungsfall funktioniert, müssen wir die Formularanhänge an einem neuen Speicherort im CRX-Repository speichern.

Der OSGi-Dienst wird erstellt, um die Formularanlagen an einem neuen Speicherort im CRX-Repository zu speichern. Eine neue Dateizuordnung wird mit dem neuen Speicherort der Anlagen in der CRX erstellt und an die aufrufende Anwendung zurückgegeben.
Im Folgenden finden Sie die FileMap, die an das Servlet gesendet wird. Der Schlüssel ist das Feld für das adaptive Formular und der Wert ist der temporäre Speicherort der Anlage. In unserem Servlet werden wir die Anlage extrahieren und an einem neuen Speicherort im AEM-Repository speichern und die FileMap mit dem neuen Speicherort aktualisieren.

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

Der folgende Code extrahiert die Anlagen aus der Anforderung und speichert sie im Ordner **/content/afattachments**

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

Dies ist die neue FileMap mit dem aktualisierten Speicherort der Formularanhänge

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```
