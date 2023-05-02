---
title: Vorausfüllen des adaptiven Formulars mit dem ACS-Profil
description: Vorausfüllen des adaptiven Forms mithilfe des ACS-Profils
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 502f4bdf-d4af-409f-a611-62b7a1a6065a
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Vorausfüllen des adaptiven Formulars mit dem ACS-Profil {#prefilling-adaptive-form-using-acs-profile}

In diesem Teil füllen wir das adaptive Formular mit Profilinformationen aus ACS vorab aus. AEM Forms verfügt über diese leistungsstarke Funktion zum Vorausfüllen adaptiver Formulare.

Weitere Informationen zum Vorausfüllen adaptiver Formulare finden Sie in diesem [Tutorial](https://helpx.adobe.com/experience-manager/kt/forms/using/prefill-service-adaptive-forms-article-use.html).

Um das adaptive Formular durch Abrufen von Daten aus ACS im Voraus auszufüllen, gehen wir davon aus, dass es in ACS ein Profil gibt, das dieselbe E-Mail wie der angemeldete AEM hat. Wenn beispielsweise die E-Mail-ID der AEM angemeldeten Person csimms@adobe.com lautet, erwarten wir, ein Profil in ACS zu finden, dessen E-Mail-Adresse csimms@adobe.com lautet.

Die folgenden Schritte sind erforderlich, um Profilinformationen mithilfe der REST-API aus ACS abzurufen

* JWT generieren
* JWT für Zugriffstoken austauschen
* Führen Sie einen REST-Aufruf an ACS durch und rufen Sie das Profil per E-Mail ab
* XML-Dokument mit Profilinformationen erstellen
* Return InputStream des von AEM Forms verwendeten XML-Dokuments

![prefillService](assets/prefillserviceaf.gif)

Verknüpfen des Vorbefüllungs-Dienstes mit einem adaptiven Formular

Im Folgenden finden Sie den Code zum Abrufen und Zurückgeben von Profilinformationen aus ACS.

In Zeile 68 rufen wir die E-Mail-ID des AEM-Benutzers ab. Die Profildetails werden durch einen REST-Aufruf an Adobe Campaign Standard abgerufen. Aus den abgerufenen Profildetails wird das XML-Dokument so erstellt, dass es von AEM Forms verstanden wird. Der Eingabestream dieses Dokuments wird zur Verwendung durch AEM Forms zurückgegeben.

```java
package aemforms.campaign.core;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.TransformerFactoryConfigurationError;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

import formsandcampaign.demo.CampaignConfigurationService;

@Component
public class PrefillAdaptiveFormWithCampaignProfile implements DataXMLProvider {
private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveFormWithCampaignProfile.class);
private static final String SERVER_FQDN = "mc.adobe.io";
private static final String ENDPOINT = "/campaign/profileAndServices/profile/byEmail?email=";
@Reference
CampaignService jwtService;
@Reference
CampaignConfigurationService campaignConfig;

Session session = null;

public JsonObject getProfileDetails()
 {
    String jwtToken = null;
    String email = null;
    log.debug("$$$$ in getProfile Details");
    try
       {
          jwtToken = jwtService.getAccessToken();
          UserManager um = ((JackrabbitSession) session).getUserManager();
          Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
          email = loggedinUser.getProperty("profile/email")[0].getString();
          log.debug("####Got email..." + email);
        }
        catch (Exception e)
        {
          log.error("Unable to generate JWT!\n", e);
        }
    String tenant = campaignConfig.getTenant();
    String apikey = campaignConfig.getApiKey();
    String path = "/" + tenant + ENDPOINT + email;
    HttpHost server = new HttpHost(SERVER_FQDN, 443, "https");
    HttpGet getReq = new HttpGet(path);
    getReq.addHeader("Cache-Control", "no-cache");
    getReq.addHeader("Content-Type", "application/json");
    getReq.addHeader("X-Api-Key", apikey);
    getReq.addHeader("Authorization", "Bearer " + jwtToken);
    HttpClient httpClient = HttpClientBuilder.create().build();
    try
       {
          HttpResponse result = httpClient.execute(server, getReq);
          String responseJson = EntityUtils.toString(result.getEntity());
          log.debug("The response Json" + responseJson);
          JsonObject responseJsonProfiles = new JsonParser().parse(responseJson).getAsJsonObject();
          log.debug("The json array is " + responseJsonProfiles.toString());
          return responseJsonProfiles.get("content").getAsJsonArray().get(0).getAsJsonObject();
        }
        catch (ClientProtocolException e)
       {
            e.printStackTrace();
        } catch (IOException e) {

      e.printStackTrace();
}
    return null;

}

public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
// TODO Auto-generated method stub
    log.debug("Geting xml");
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    session = resolver.adaptTo(Session.class);
    JsonObject profile = getProfileDetails();
    log.debug("####profile last name ####" + profile.get("lastName").getAsString());
    try {
          DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
          DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
          Document doc = docBuilder.newDocument();
          Element rootElement = doc.createElement("data");
          doc.appendChild(rootElement);
          Element firstNameElement = doc.createElement("fname");
          firstNameElement.setTextContent(profile.get("firstName").getAsString());
          log.debug("created firstNameElement  " + profile.get("firstName").getAsString());
          Element lastNameElement = doc.createElement("lname");
          Element jobTitleElement = doc.createElement("jobTitle");
          jobTitleElement.setTextContent(profile.get("salutation").getAsString());
          Element cityElement = doc.createElement("city");
          cityElement.setTextContent(profile.get("location").getAsJsonObject().get("city").getAsString());
          log.debug("created cityElement  " + profile.get("location").getAsJsonObject().get("city").getAsString());
          Element countryElement = doc.createElement("country");
          countryElement.setTextContent(profile.get("location").getAsJsonObject().get("countryCode").getAsString());
          Element streetElement = doc.createElement("street");
          streetElement.setTextContent(profile.get("location").getAsJsonObject().get("address1").getAsString());
          Element postalCodeElement = doc.createElement("postalCode");
          postalCodeElement.setTextContent(profile.get("location").getAsJsonObject().get("zipCode").getAsString());
          Element genderElement = doc.createElement("gender");
          genderElement.setTextContent(profile.get("gender").getAsString());
          lastNameElement.setTextContent(profile.get("lastName").getAsString());
          Element emailElement = doc.createElement("email");
          emailElement.setTextContent(profile.get("email").getAsString());
          rootElement.appendChild(firstNameElement);
          rootElement.appendChild(lastNameElement);
          rootElement.appendChild(emailElement);
          rootElement.appendChild(streetElement);
          rootElement.appendChild(countryElement);
          rootElement.appendChild(cityElement);
          rootElement.appendChild(jobTitleElement);
          rootElement.appendChild(postalCodeElement);
          rootElement.appendChild(genderElement);
          ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
          DOMSource source = new DOMSource(doc);
          StreamResult outputTarget = new StreamResult(outputStream);
          TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
          xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
          return xmlDataStream;

}
    catch (ParserConfigurationException e)
    {
          e.printStackTrace();
    }catch (TransformerConfigurationException e)
    {
        e.printStackTrace();
    } 
    catch (TransformerException e)
   {
        e.printStackTrace();
  } catch (TransformerFactoryConfigurationError e) {
// TODO Auto-generated catch block
e.printStackTrace();
}

return null;
}

@Override
public String getServiceDescription() {

return "Custom Aem Form Pre Fill Service using campaign";
}

@Override
public String getServiceName() {
// TODO Auto-generated method stub
return "Pre Fill Forms Using Campaign Profile";
}

}
```

Gehen Sie wie folgt vor, um die Arbeit an Ihrem System zu ermöglichen:

* [Vergewissern Sie sich, dass Sie die hier beschriebenen Schritte ausgeführt haben](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Importieren des adaptiven Beispielformulars in AEM mithilfe des Paketmanagers](assets/pre-fill-af-from-campaign.zip)
* Melden Sie sich bei AEM mit einem Benutzer an, dessen E-Mail-Adresse von einem Profil in Adobe Campaign freigegeben wurde. Wenn die E-Mail-ID des AEM-Benutzers beispielsweise johndoe@adobe.com lautet, müssen Sie über ein Profil in ACS verfügen, dessen E-Mail-Adresse johndoe@adobe.com lautet.
* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/prefillfromcampaign/jcr:content?wcmmode=disabled).

## Nächste Schritte

[Adobe Campaign-Profil mithilfe des Formulardatenmodells erstellen](./partfour.md)

