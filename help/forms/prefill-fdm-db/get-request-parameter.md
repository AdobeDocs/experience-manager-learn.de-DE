---
title: Anforderungsparameter abrufen
description: Zugriff auf den Anforderungsparameter des Vorbefüllungs-Diensts eines Formulardatenmodells
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 6%

---

# Anforderungsparameter abrufen

## empID-Parameter abrufen

Der nächste Schritt besteht darin, über die URL auf den Parameter empID zuzugreifen. Der Wert des Anforderungsparameters empID wird dann an den Dienstvorgang **_get_** des Formulardatenmodells übergeben.
Im Rahmen dieses Kurses haben wir die folgenden Informationen erstellt und bereitgestellt:

* Adaptive Formularvorlage mit dem Namen **_FDMDemo_**
* Seitenkomponente mit dem Namen **_fdmdemo_**
* Benutzerdefinierte JSP mit der Seitenkomponente eingeschlossen
* Verknüpfen der adaptiven Formularvorlage mit der Seitenkomponente

Dadurch wird unser Code in der benutzerdefinierten JSP nur ausgeführt, wenn das adaptive Formular, das auf dieser benutzerdefinierten Vorlage basiert, wiedergegeben wird

* [Importieren Sie das ](assets/template-page-component.zip) Paket mit  [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Öffnen Sie fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
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

Der Wert von empID ist mit dem Schlüssel empID in paraMap verknüpft. Diese Zuordnung wird dann an slingRequest übergeben.

>[!NOTE]
>
>Die Schlüssel-empID muss mit dem Bindungswert des neuen get-Dienstes für Entitäten übereinstimmen
