---
title: Schreiben von benutzerdefinierten Übermittlungen in AEM Forms
seo-title: Schreiben von benutzerdefinierten Übermittlungen in AEM Forms
description: Schnelle und einfache Möglichkeit, eigene benutzerdefinierte Übermittlungsaktionen für adaptive Formulare zu erstellen
seo-description: Schnelle und einfache Möglichkeit, eigene benutzerdefinierte Übermittlungsaktionen für adaptive Formulare zu erstellen
feature: Adaptive Formulare
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 4%

---


# Schreiben eines benutzerdefinierten Übermittlungsformulars in AEM Forms {#writing-a-custom-submit-in-aem-forms}

Schnelle und einfache Möglichkeit, eigene benutzerdefinierte Übermittlungsaktionen für adaptive Formulare zu erstellen

In diesem Artikel werden Sie durch die Schritte geführt, die zum Erstellen einer benutzerdefinierten Übermittlungsaktion für die Verarbeitung der adaptiven Forms-Übermittlung erforderlich sind.

* Bei CRX anmelden
* Erstellen Sie einen Knoten des Typs &quot;sling :folder&quot; unter Apps. Rufen wir diesen Knoten CustomSubmitHelpx auf.
* Speichern Sie den neu erstellten Knoten.
* hinzufügen die folgenden beiden Eigenschaften an den neu erstellten Knoten
* PropertyName       | Eigenschaftenwert
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* Speichern Sie die Änderungen
* Erstellen Sie eine neue Datei mit dem Namen post.POST.jsp unter dem Knoten CustomSubmitHelpx. Wenn ein adaptives Formular gesendet wird, wird dieses JSP aufgerufen. Sie können den JSP-Code gemäß Ihren Anforderungen in dieser Datei schreiben. Der folgende Code leitet die Anforderung an das Servlet weiter.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* Erstellen Sie die Datei addfields.jsp unter dem Knoten CustomSubmitHelpx. Diese Datei ermöglicht Ihnen den Zugriff auf das signierte Dokument.
* hinzufügen folgenden Code in diese Datei

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Speichern Sie Ihre Änderungen

Jetzt sehen Sie im Beginn &quot;CustomSubmitHelpx&quot;in den Übermittlungsaktionen Ihres adaptiven Formulars, wie in diesem Bild dargestellt.

![Adaptives Formular mit benutzerdefiniertem Senden](assets/capture-2.gif)

