---
title: Vorbefüllungsdienst in adaptiven Formularen
description: Vorausfüllen adaptiver Formulare durch Abrufen von Daten aus Backend-Datenquellen.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
duration: 136
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 100%

---

# UsingPrefill-Dienst in adaptiven Formularen

Sie können die Felder eines adaptiven Formulars mit vorhandenen Daten vorbefüllen. Wenn eine Benutzerin oder ein Benutzer ein Formular öffnet, werden die Werte für diese Felder vorbefüllt. Es gibt mehrere Möglichkeiten, adaptive Formularfelder vorab auszufüllen. In diesem Artikel werden wir uns das Vorausfüllen adaptiver Formulare mithilfe des Vorbefüllungs-Dienstes in AEM Forms ansehen.

Weitere Informationen über verschiedene Methoden zum Vorausfüllen von adaptiven Formularen erfahren Sie in [dieser Dokumentation](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Um ein adaptives Formular mithilfe des Prefill-Dienstes vorauszufüllen, müssen Sie eine Klasse erstellen, die die Schnittstelle `com.adobe.forms.common.service.DataXMLProvider` implementiert.  Die Methode `getDataXMLForDataRef` enthält die Logik zum Erstellen und Zurückgeben von Daten, die das adaptive Formular zum Vorausfüllen der Felder verwenden wird. Bei dieser Methode können Sie die Daten aus einer beliebigen Quelle abrufen und den Eingabe-Stream des Datendokuments zurückgeben. Der folgende Beispiel-Code ruft die Benutzerprofilinformationen der angemeldeten Benutzenden ab und erstellt ein XML-Dokument, dessen Eingabe-Stream zurückgegeben wird, um von den adaptiven Formularen genutzt zu werden.

Im folgenden Codesnippet haben wir eine Klasse, die die DataXMLProvider-Schnittstelle implementiert.  Wir erhalten Zugriff auf die angemeldeten Benutzenden und rufen dann deren Profilinformationen ab. Anschließend erstellen wir ein XML-Dokument mit einem Stammknotenelement namens „data“ und hängen entsprechende Elemente an diesen Datenknoten an. Nachdem das XML-Dokument erstellt wurde, wird der Eingabe-Stream des XML-Dokuments zurückgegeben.

Diese Klasse wird dann in ein OSGi-Bundle umgewandelt und in AEM bereitgestellt. Sobald das Bundle bereitgestellt ist, ist dieser Vorbefüllungsdienst verfügbar, der als Vorbefüllungsdienst für Ihr adaptives Formular verwendet werden kann.

>[!NOTE]
>
>Sie können Formulare mit XML- oder JSON-Daten vorab ausfüllen, indem Sie den in diesem Artikel aufgeführten Ansatz verwenden.

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

Um diese Funktion auf Ihrem Server zu testen, führen Sie die folgenden Schritte aus:

* Vergewissern Sie sich, dass die Informationen [im Profil](http://localhost:4502/security/users.html) der angemeldeten Benutzenden ausgefüllt sind.  Im Beispiel wird nach den Eigenschaften „FirstName“, „LastName“ und der E-Mail der angemeldeten Person gesucht.
* [Laden Sie den Inhalt der Zip-Datei herunter und entpacken Sie ihn auf Ihren Computer](assets/prefillservice.zip)
* Stellen Sie das Bundle „prefill.core-1.0.0-SNAPSHOT“ über die [AEM-Web-Konsole](http://localhost:4502/system/console/bundles) bereit
* Importieren Sie das adaptive Formular mithilfe der Funktion „Erstellen“ > „Datei-Upload“ aus dem Abschnitt [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Vergewissern Sie sich, dass das [Formular](http://localhost:4502/editor.html/content/forms/af/prefill.html) den **„benutzerdefinierten AEM Forms-Vorbefüllungsdienst“** als Vorbefüllungsdienst verwendet.  Dies kann anhand der Konfigurationseigenschaften des Abschnitts **Formular-Container** überprüft werden.
* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Das Formular sollte mit den richtigen Werten ausgefüllt sein.

>[!NOTE]
>
>Wenn Sie das Debuggen für com.aem.prefill.core.PrefillAdaptiveForm aktiviert haben, wird die generierte XML-Datendatei in den Installationsordner für den AEM-Server geschrieben.

