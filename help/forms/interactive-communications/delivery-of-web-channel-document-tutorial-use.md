---
title: Bereitstellung des Dokuments zur interaktiven Kommunikation – Web-Kanal – AEM Forms
description: Versand des Web-Kanaldokuments über einen Link in der E-Mail
feature: Interactive Communication
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 73
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 100%

---

# E-Mail-Versand des Web-Kanaldokuments

Nachdem Sie Ihr Web-Kanaldokument zur interaktiven Kommunikation definiert und getestet haben, benötigen Sie einen Versandmechanismus, um das Web-Kanaldokument an die Empfängerin oder den Empfänger zu senden.

In diesem Artikel betrachten wir E-Mail als Bereitstellungsmechanismus für Web-Kanaldokumente. Die Empfängerin oder der Empfänger erhält per E-Mail einen Link zum Web-Kanaldokument. Beim Klicken auf den Link wird die Person aufgefordert, sich zu authentifizieren, und das Web-Kanaldokument wird mit den für die angemeldete Person spezifischen Daten gefüllt.

Sehen wir uns das folgende Code-Fragment an. Dieser Code ist Teil von GET.jsp, das ausgelöst wird, wenn die Person auf den Link in der E-Mail klickt, um das Web-Kanaldokument anzuzeigen. Wir erhalten die angemeldete Person mithilfe von Jackrabbit UserManager. Sobald wir die angemeldete Person erhalten haben, erhalten wir den Wert der Eigenschaft accountNumber, die mit ihrem Profil verknüpft ist.

Dann verknüpfen wir den Wert accountNumber mit einem Schlüssel namens accountnumber in der Karte. Der Schlüssel **accountnumber** wird im Formulardatenmodell als Anfrageattribut definiert. Der Wert dieses Attributs wird als Eingabeparameter an die Methode des Lesedienstes für Formulardatenmodelle übergeben.

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

![Konfiguration von Anfrageparametern](assets/requestparameter.png)

Anfrageattribut definiert für den Lesedienst des Formulardatenmodells

[Beispiel-AEM-Paket](assets/webchanneldelivery.zip).
