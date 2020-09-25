---
title: Anforderungsparameter abrufen
description: Zugriff auf den Anforderungsparameter eines Formulardatenmodells auf den Vorausfülldienst
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 4%

---

# Anforderungsparameter abrufen

## Get empID-Parameter

Der nächste Schritt ist der Zugriff auf den Parameter empID aus der URL. Der Wert des Abfrageparameters empID wird dann an den **_get_** -Dienstvorgang des Formulardatenmodells weitergeleitet.
Für die Zwecke dieses Kurses haben wir die folgenden erstellt und bereitgestellt:

* Vorlage für adaptives Formular mit dem Namen **_FDMDemo_**
* Seitenkomponente **_fdmdemo_**
* Einschließlich benutzerdefinierter JSP-Dateien mit der Seitenkomponente
* Die Vorlage für das adaptive Formular wurde der Seitenkomponente zugeordnet.

Auf diese Weise wird unser Code in der benutzerdefinierten JSP nur ausgeführt, wenn das adaptive Formular, das auf dieser benutzerdefinierten Vorlage basiert, wiedergegeben wird

* [Paket](assets/template-page-component.zip) mit [Package Manager importieren](http://localhost:4502/crx/packmgr/index.jsp)
* [Öffnen Sie fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Entfernen Sie die Auskommentierung der kommentierten Zeilen.
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

Der Wert von empID ist mit dem Schlüssel empID in paraMap verknüpft. Diese Map wird dann an die slingRequest weitergeleitet

>[!NOTE]
>
>Die Schlüssel-empID muss mit dem Bindungswert des neuen get-Dienstes für Entitäten übereinstimmen
