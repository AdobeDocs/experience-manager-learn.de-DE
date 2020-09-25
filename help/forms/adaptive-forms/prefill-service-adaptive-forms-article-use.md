---
title: Vorfülldienst im adaptiven Forms
seo-title: Vorfülldienst im adaptiven Forms
description: Vorauffüllen adaptiver Formulare durch Abrufen von Daten aus Back-End-Datenquellen
seo-description: Vorauffüllen adaptiver Formulare durch Abrufen von Daten aus Back-End-Datenquellen
sub-product: Formulare
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 7%

---


# UsingPrefill-Dienst im adaptiven Forms

Sie können die Felder eines adaptiven Formulars mit vorhandenen Daten vorbefüllen. Wenn ein Benutzer ein Formular öffnet, werden die Werte für diese Felder vorbefüllt. Es gibt mehrere Möglichkeiten, adaptive Formularfelder im Voraus auszufüllen. In diesem Artikel werden wir uns mit dem Vorausfüllen des adaptiven Formulars mit dem AEM Forms-Vorfülldienst beschäftigen.

Weitere Informationen zu den verschiedenen Methoden zum Vorausfüllen adaptiver Formulare erhalten [Sie in der folgenden Dokumentation](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Um ein adaptives Formular mit dem Vorfülldienst vorab auszufüllen, müssen Sie eine Klasse erstellen, die die DataProvider-Schnittstelle implementiert. Die Methode getPrefillData verfügt über die Logik zum Erstellen und Zurückgeben von Daten, die das adaptive Formular zum Vorausfüllen der Felder benötigt. Bei dieser Methode können Sie die Daten von einer beliebigen Quelle abrufen und den Eingabestream des Dokuments zurückgeben. Der folgende Beispielcode ruft die Benutzerdaten des angemeldeten Profils ab und erstellt ein XML-Dokument, dessen Eingabestream zurückgegeben wird, um von den adaptiven Formularen genutzt zu werden.

Im folgenden Codeausschnitt finden Sie eine Klasse, die die DataProvider-Schnittstelle implementiert. Wir erhalten Zugriff auf den angemeldeten Benutzer und rufen dann die Informationen zum Profil des angemeldeten Benutzers ab. Anschließend erstellen wir ein XML-Dokument mit einem Stamm-Node-Element namens &quot;data&quot;und hängen entsprechende Elemente an diese Daten-Node an. Nachdem das XML-Dokument erstellt wurde, wird der Eingabestream des XML-Dokuments zurückgegeben.

Diese Klasse wird dann zu OSGi-Bundle verarbeitet und in AEM bereitgestellt. Sobald das Bundle bereitgestellt ist, steht dieser Vorfülldienst als Vorfülldienst für Ihr adaptives Formular zur Verfügung.

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

Um diese Funktion auf Ihrem Server zu testen, führen Sie die folgenden Schritte aus

* [Herunterladen und Extrahieren des Inhalts der ZIP-Datei auf Ihren Computer](assets/prefillservice.zip)
* Vergewissern Sie sich, dass die Profil [des angemeldeten](http://localhost:4502/libs/granite/security/content/useradmin) Benutzers vollständig ausgefüllt ist. Dies ist eine Voraussetzung, damit das Beispiel funktioniert. Das Beispiel enthält keine Fehlerprüfung auf fehlende Eigenschaften des Profils.
* Bereitstellen des Bundles mithilfe der [AEM](http://localhost:4502/system/console/bundles)
* Adaptives Formular mit der XSD erstellen
* Verknüpfen Sie &quot;Custom AEM Form Pre Fill Service&quot;als Vorausfülldienst für Ihr adaptives Formular.
* Ziehen Sie Schema-Elemente per Drag &amp; Drop in das Formular
* Nutzen Sie die Funktion zur Formularvorschau.

>[!NOTE]
>
>Wenn das adaptive Formular auf XSD basiert, stellen Sie sicher, dass das vom Vorausfülldienst zurückgegebene XML-Dokument mit der XSD übereinstimmt, auf der Ihr adaptives Formular basiert.
>
>Wenn das adaptive Formular nicht auf XSD basiert, müssen Sie die Felder manuell binden. Beispiel: Um ein adaptives Formularfeld an ein fname-Element in den XML-Daten zu binden, die Sie `/data/fname` in der Bindungsreferenz des adaptiven Formularfelds verwenden werden.

