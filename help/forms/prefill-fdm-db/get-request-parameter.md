---
title: GET-Anfrage-Parameter
description: Zugriff auf den Anforderungsparameter des Vorbefüllungs-Diensts eines Formulardatenmodells
feature: Adaptive Forms
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 8%

---

# GET-Anfrage-Parameter

## empID-Parameter abrufen

Der nächste Schritt besteht darin, über die URL auf den Parameter empID zuzugreifen. Der Wert des empID-Anforderungsparameters wird dann an die **_get_** Dienstvorgang des Formulardatenmodells.
Im Rahmen dieses Kurses haben wir die folgenden Informationen erstellt und bereitgestellt:

* Adaptive Formularvorlage mit dem Namen **_FDMDemo_**
* Seitenkomponente namens **_fdmdemo_**
* Benutzerdefinierte JSP mit der Seitenkomponente eingeschlossen
* Verknüpfen der adaptiven Formularvorlage mit der Seitenkomponente

Dadurch wird unser Code in der benutzerdefinierten JSP nur ausgeführt, wenn das adaptive Formular, das auf dieser benutzerdefinierten Vorlage basiert, wiedergegeben wird

* [Package importieren](assets/template-page-component.zip) using [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
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

## Nächste Schritte

[Erstellen eines adaptiven Formulars basierend auf dem Formulardatenmodell](./create-adaptive-form.md)
