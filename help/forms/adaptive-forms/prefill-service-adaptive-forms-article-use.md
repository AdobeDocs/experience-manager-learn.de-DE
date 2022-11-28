---
title: Vorbefüllungsdienst in Adaptive Forms
description: Vorausfüllen adaptiver Formulare durch Abrufen von Daten aus Back-End-Datenquellen.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: 381812397fa7d15f6ee34ef85ddf0aa0acc0af42
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 8%

---

# Verwenden des Vorbefüllungs-Dienstes in Adaptive Forms

Sie können die Felder eines adaptiven Formulars mit vorhandenen Daten vorbefüllen. Wenn ein Benutzer ein Formular öffnet, werden die Werte für diese Felder vorbefüllt. Es gibt mehrere Möglichkeiten, adaptive Formularfelder vorab auszufüllen. In diesem Artikel werden wir uns das Vorausfüllen adaptiver Formulare mithilfe des AEM Forms-Vorbefüllungs-Dienstes ansehen.

Weitere Informationen zu verschiedenen Methoden zum Vorausfüllen adaptiver Formulare erhalten Sie unter [folgen Sie dieser Dokumentation](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Um ein adaptives Formular mit dem Vorbefüllungs-Dienst vorab auszufüllen, müssen Sie eine Klasse erstellen, die die `com.adobe.forms.common.service.DataXMLProvider` -Schnittstelle. Die Methode `getDataXMLForDataRef` verfügt über die Logik zum Erstellen und Zurückgeben von Daten, die das adaptive Formular zum Vorausfüllen der Felder benötigt. Bei dieser Methode können Sie die Daten aus einer beliebigen Quelle abrufen und den Eingabestream des Datendokuments zurückgeben. Der folgende Beispielcode ruft die Benutzerprofilinformationen des angemeldeten Benutzers ab und erstellt ein XML-Dokument, dessen Eingabestream zurückgegeben wird, um von den adaptiven Formularen genutzt zu werden.

Im folgenden Codefragment haben wir eine Klasse, die die DataXMLProvider-Schnittstelle implementiert. Wir erhalten Zugriff auf den angemeldeten Benutzer und rufen dann die Profilinformationen des angemeldeten Benutzers ab. Anschließend erstellen wir ein XML-Dokument mit einem Stammknotenelement namens &quot;data&quot;und hängen entsprechende Elemente an diesen Datenknoten an. Nachdem das XML-Dokument erstellt wurde, wird der Eingabestream des XML-Dokuments zurückgegeben.

Diese Klasse wird dann in ein OSGi-Bundle umgewandelt und in AEM bereitgestellt. Sobald das Bundle bereitgestellt ist, ist dieser Vorbefüllungs-Dienst verfügbar, der als Vorbefüllungs-Dienst für Ihr adaptives Formular verwendet werden kann.

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

Um diese Funktion auf Ihrem Server zu testen, führen Sie die folgenden Schritte aus

* Stellen Sie sicher, dass Sie angemeldet sind. [Benutzerprofil](http://localhost:4502/security/users.html) ausgefüllt werden. Das Beispiel sucht nach den Eigenschaften FirstName, LastName und Email des angemeldeten Benutzers.
* [Laden Sie den Inhalt der ZIP-Datei herunter und extrahieren Sie ihn auf Ihren Computer](assets/prefillservice.zip)
* Stellen Sie das Bundle prefill.core-1.0.0-SNAPSHOT mithilfe des [AEM Web-Konsole](http://localhost:4502/system/console/bundles)
* Importieren Sie das adaptive Formular mit der Option Erstellen | Datei-Upload aus dem [Abschnitt &quot;FormsAndDocuments&quot;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Stellen Sie sicher, dass [Formular](http://localhost:4502/editor.html/content/forms/af/prefill.html) verwendet **&quot;Custom AEM Forms PreFill Service&quot;** als Vorbefüllungs-Dienst. Dies kann anhand der Konfigurationseigenschaften der **Formular-Container** Abschnitt.
* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Das Formular sollte mit den richtigen Werten ausgefüllt werden.

>[!NOTE]
>
>Wenn Sie das Debugging für com.aem.prefill.core.PrefillAdaptiveForm aktiviert haben, wird die generierte XML-Datendatei in den Installationsordner für den AEM-Server geschrieben.

