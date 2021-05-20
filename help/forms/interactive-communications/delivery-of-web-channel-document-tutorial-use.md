---
title: Bereitstellung des Dokuments zur interaktiven Kommunikation - Webkanal-AEM Forms
seo-title: Bereitstellung des Dokuments zur interaktiven Kommunikation - Webkanal-AEM Forms
description: Versand des Webkanaldokuments über einen Link in der E-Mail
seo-description: Versand des Webkanaldokuments über einen Link in der E-Mail
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---


# E-Mail-Versand des Webkanaldokuments

Nachdem Sie Ihr Dokument zur interaktiven Kommunikation mit Webkanälen definiert und getestet haben, benötigen Sie einen Bereitstellungsmechanismus, um das Webkanaldokument an den Empfänger zu senden.

In diesem Artikel betrachten wir E-Mail als Bereitstellungsmechanismus für Webkanaldokumente. Der Empfänger erhält per E-Mail einen Link zum Webkanaldokument. Beim Klicken auf den Link wird der Benutzer zur Authentifizierung aufgefordert und das Webkanaldokument wird mit den für den angemeldeten Benutzer spezifischen Daten gefüllt.

Sehen wir uns das folgende Codefragment an. Dieser Code ist Teil von GET.jsp , das ausgelöst wird, wenn der Benutzer auf den Link in der E-Mail klickt, um das Webkanaldokument anzuzeigen. Wir erhalten den angemeldeten Benutzer mit dem Jackrabbit UserManager. Sobald wir den angemeldeten Benutzer erhalten haben, erhalten wir den Wert der Eigenschaft accountNumber , die mit dem Profil des Benutzers verknüpft ist.

Dann verknüpfen wir den Wert accountNumber mit einem Schlüssel namens accountNumber in der Karte. Der Schlüssel **account tnumber** wird im Formulardatenmodell als Anforderungsattribut definiert. Der Wert dieses Attributs wird als Eingabeparameter an die Methode des Lesedienstes für Formulardatenmodelle übergeben.

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

![includeMethod](assets/includemethod.jpg)

Visuelle Darstellung des Codes für Zeile 7

![requestparameter](assets/requestparameter.png)

Anforderungsattribut definiert für den Lesedienst des Formulardaten-Modals


[Beispielpaket AEM](assets/webchanneldelivery.zip).
