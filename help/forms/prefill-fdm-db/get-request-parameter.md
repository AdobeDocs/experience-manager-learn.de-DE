---
title: Anforderungsparameter abrufen
description: Zugriff auf den Anforderungsparameter eines Formulardatenmodells auf den Vorausfülldienst
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Entwicklung
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 6%

---

# Anforderungsparameter abrufen

## Get empID-Parameter

Der nächste Schritt ist der Zugriff auf den Parameter empID aus der URL. Der Wert des Anforderungsparameters empID wird dann an den Dienstvorgang **_get_** des Formulardatenmodells übergeben.
Für die Zwecke dieses Kurses haben wir die folgenden erstellt und bereitgestellt:

* Adaptive Formularvorlage mit dem Namen **_FDMDemo_**
* Seitenkomponente mit dem Namen **_fdmdemo_**
* Einschließlich benutzerdefinierter JSP-Dateien mit der Seitenkomponente
* Die Vorlage für das adaptive Formular wurde der Seitenkomponente zugeordnet.

Auf diese Weise wird unser Code in der benutzerdefinierten JSP nur ausgeführt, wenn das adaptive Formular, das auf dieser benutzerdefinierten Vorlage basiert, wiedergegeben wird

* [Importieren des ](assets/template-page-component.zip) Pakets mit dem  [Paketmanager](http://localhost:4502/crx/packmgr/index.jsp)
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
