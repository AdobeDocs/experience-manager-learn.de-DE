---
title: Bereitstellung des Dokuments zur interaktiven Kommunikation - Webkanal-AEM Forms
description: Versand des Webkanaldokuments über einen Link in der E-Mail
feature: Interactive Communication
audience: developer
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# E-Mail-Versand des Webkanaldokuments

Nachdem Sie Ihr Dokument zur interaktiven Kommunikation mit Webkanälen definiert und getestet haben, benötigen Sie einen Bereitstellungsmechanismus, um das Webkanaldokument an den Empfänger zu senden.

In diesem Artikel betrachten wir E-Mail als Bereitstellungsmechanismus für Webkanaldokumente. Der Empfänger erhält per E-Mail einen Link zum Webkanaldokument. Beim Klicken auf den Link wird der Benutzer aufgefordert, sich zu authentifizieren, und das Webkanaldokument wird mit den für den angemeldeten Benutzer spezifischen Daten gefüllt.

Sehen wir uns das folgende Codefragment an. Dieser Code ist Teil von GET.jsp , das ausgelöst wird, wenn der Benutzer auf den Link in der E-Mail klickt, um das Webkanaldokument anzuzeigen. Wir erhalten den angemeldeten Benutzer mit dem Jackrabbit UserManager. Sobald wir den angemeldeten Benutzer erhalten haben, erhalten wir den Wert der Eigenschaft accountNumber , die mit dem Profil des Benutzers verknüpft ist.

Dann verknüpfen wir den Wert accountNumber mit einem Schlüssel namens accountNumber in der Karte. Der Schlüssel **Kontonummer** wird im Formulardatenmodell als Anforderungsattribut definiert. Der Wert dieses Attributs wird als Eingabeparameter an die Methode des Lesedienstes für Formulardatenmodelle übergeben.

Zeile 7: Wir senden die empfangene Anfrage an ein anderes Servlet, basierend auf dem Ressourcentyp, der von der URL des interaktiven Kommunikationsdokuments identifiziert wird. Die von diesem zweiten Servlet zurückgegebene Antwort ist in der Antwort des ersten Servlets enthalten.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![Ansatz der Einschlussmethode](assets/includemethod.jpg)

Visuelle Darstellung des Codes für Zeile 7

![Konfiguration von Anforderungsparametern](assets/requestparameter.png)

Anforderungsattribut definiert für den Lesedienst des Formulardaten-Modals

[AEM](assets/webchanneldelivery.zip).
