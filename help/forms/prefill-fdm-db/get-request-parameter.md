---
title: GET-Anfrage-Parameter
description: Zugreifen auf den Anfrageparameter des Vorbefüllungsdienstes eines Formulardatenmodells
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 100%

---

# GET-Anfrage-Parameter

## Abrufen des empID-Parameters

Der nächste Schritt besteht darin, über die URL auf den empID-Parameter zuzugreifen. Der Wert des empID-Anfragesparameters wird dann an den **_GET_**-Dienstvorgang des Formulardatenmodells weitergegeben.
Im Rahmen dieses Kurses haben wir Folgendes erstellt und bereitgestellt:

* adaptive Formularvorlage mit dem Namen **_FDMDemo_**
* Seitenkomponente mit dem Namen **_fdmdemo_**
* Einschließen der benutzerdefinierten JSP-Datei in der Seitenkomponente
* Verknüpfen der adaptiven Formularvorlage mit der Seitenkomponente

Dadurch wird unser Code in der benutzerdefinierten JSP-Datei nur ausgeführt, wenn das adaptive Formular, das auf dieser benutzerdefinierten Vorlage basiert, wiedergegeben wird

* [Importieren Sie das Paket](assets/template-page-component.zip) mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).
* [Öffnen Sie „fdmrequest.jsp“.](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Heben Sie die Auskommentierung der kommentierten Zeilen auf.
* Speichern Sie Ihre Änderungen

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

Der empID-Wert ist mit dem Schlüssel „empID“ in „paraMap“ verknüpft. Diese Zuordnung wird dann an „slingRequest“ übergeben.

>[!NOTE]
>
>Die Schlüssel „empID“ muss mit dem Bindungswert des neuen GET-Dienstes der Entität „Neueinstellungen“ übereinstimmen.

## Nächste Schritte

[Erstellen eines adaptiven Formulars auf der Basis eines Formulardatenmodells](./create-adaptive-form.md)
