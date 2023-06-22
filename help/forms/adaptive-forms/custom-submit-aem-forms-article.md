---
title: Schreiben eines benutzerdefinierten Versands in AEM Forms
description: Schnelle und einfache Erstellung einer eigenen benutzerdefinierten Übermittlungsaktion für adaptive Formulare
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 5%

---

# Schreiben eines benutzerdefinierten Versands in AEM Forms {#writing-a-custom-submit-in-aem-forms}

Schnelle und einfache Erstellung einer eigenen benutzerdefinierten Übermittlungsaktion für adaptive Formulare

Dieser Artikel führt Sie durch die Schritte, die zum Erstellen einer benutzerdefinierten Übermittlungsaktion für die Verarbeitung der adaptiven Forms-Übermittlung erforderlich sind.

* Bei crx anmelden
* Erstellen Sie einen Knoten des Typs &quot;sling:folder &quot;unter Apps. Rufen wir diesen Knoten CustomSubmitHelpx auf.
* Speichern Sie den neu erstellten Knoten.
* Fügen Sie die folgenden drei Eigenschaften zum neu erstellten Knoten hinzu

| Eigenschaftsname | Eigenschaftswert |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidesubmittype |
| guideDataModel | xfa,xsd,basic |
| jcr:description | CustomSubmitHelpx |


* Speichern Sie die Änderungen
* Erstellen Sie eine neue Datei namens post.POST.jsp unter dem Knoten CustomSubmitHelpx . Wenn ein adaptives Formular gesendet wird, wird diese JSP aufgerufen. Sie können den JSP-Code gemäß Ihren Anforderungen in diese Datei schreiben. Der folgende Code leitet die Anfrage an das Servlet weiter.

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

* Erstellen Sie die Datei addfields.jsp unter dem Knoten CustomSubmitHelpx . Diese Datei ermöglicht Ihnen den Zugriff auf das signierte Dokument.
* Fügen Sie dieser Datei den folgenden Code hinzu

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Speichern Sie Ihre Änderungen

Jetzt sehen Sie &quot;CustomSubmitHelpx&quot;in den Sendeaktionen Ihres adaptiven Formulars, wie in diesem Bild dargestellt.

![Adaptives Formular mit benutzerdefiniertem Senden](assets/capture-2.gif)
