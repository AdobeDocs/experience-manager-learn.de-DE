---
title: Erstellen einer benutzerdefinierten Übermittlungsaktion in AEM Forms
description: Eine schnelle und einfache Möglichkeit, um benutzerdefinierte Übermittlungsaktionen für adaptive Formulare zu erstellen.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
duration: 59
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 100%

---

# Erstellen einer benutzerdefinierten Übermittlungsaktion in AEM Forms {#writing-a-custom-submit-in-aem-forms}

Eine schnelle und einfache Möglichkeit, um benutzerdefinierte Übermittlungsaktionen für adaptive Formulare zu erstellen.

Dieser Artikel führt Sie durch die Schritte, die zum Erstellen einer benutzerdefinierten Übermittlungsaktion für adaptive Formulare erforderlich sind.

* Melden Sie sich bei crx an.
* Erstellen Sie unter Apps einen Knoten vom Typ „sling:folder“. Dieser Knoten soll „CustomSubmitHelpx“ heißen.
* Speichern Sie den neu erstellten Knoten.
* Fügen Sie die drei folgenden Eigenschaften zum neu erstellten Knoten hinzu.

| Eigenschaftsname | Eigenschaftswert |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidetelephone |
| guideDataModel | xfa,xsd,basic |
| jcr:description | CustomSubmitHelpx |


* Speichern Sie die Änderungen
* Erstellen Sie eine neue Datei namens „post.POST.jsp“ unter dem Knoten „CustomSubmitHelpx“. Beim Übermitteln eines adaptiven Formulars wird diese JSP-Datei aufgerufen. Sie können den JSP-Code gemäß Ihren Anforderungen in diese Datei schreiben. Mit dem folgenden Code wird die Anfrage an das Servlet weitergeleitet.

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

* Erstellen Sie die Datei „addfields.jsp“ unter dem Knoten „CustomSubmitHelpx“. Mit dieser Datei können Sie auf das signierte Dokument zugreifen.
* Fügen Sie den folgenden Code zu dieser Datei hinzu:

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Speichern Sie Ihre Änderungen

Nun sehen Sie „CustomSubmitHelpx“ in den Übermittlungsaktionen Ihres adaptiven Formulars, wie in diesem Bild gezeigt.

![Adaptives Formular mit benutzerdefinierten Übermittlungsaktionen](assets/capture-2.gif)
