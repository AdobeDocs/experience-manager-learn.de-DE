---
title: Vorbefüllungsdienst in Adaptive Forms
seo-title: Vorbefüllungsdienst in Adaptive Forms
description: Vorausfüllen adaptiver Formulare durch Abrufen von Daten aus Back-End-Datenquellen.
seo-description: Vorausfüllen adaptiver Formulare durch Abrufen von Daten aus Back-End-Datenquellen.
sub-product: Formulare
feature: Adaptive Formulare
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 7%

---


# Verwenden des Vorbefüllungs-Dienstes in Adaptive Forms

Sie können die Felder eines adaptiven Formulars mit vorhandenen Daten vorbefüllen. Wenn ein Benutzer ein Formular öffnet, werden die Werte für diese Felder vorbefüllt. Es gibt mehrere Möglichkeiten, adaptive Formularfelder vorab auszufüllen. In diesem Artikel werden wir uns das Vorausfüllen adaptiver Formulare mithilfe des AEM Forms-Vorbefüllungs-Dienstes ansehen.

Weitere Informationen zu verschiedenen Methoden zum Vorausfüllen adaptiver Formulare finden Sie in [dieser Dokumentation](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Um das adaptive Formular mit dem Vorbefüllungs-Dienst vorab auszufüllen, müssen Sie eine Klasse erstellen, die die DataProvider-Schnittstelle implementiert. Die Methode getPrefillData verfügt über die Logik zum Erstellen und Zurückgeben von Daten, die das adaptive Formular zum Vorausfüllen der Felder benötigt. Bei dieser Methode können Sie die Daten aus einer beliebigen Quelle abrufen und den Eingabestream des Datendokuments zurückgeben. Der folgende Beispielcode ruft die Benutzerprofilinformationen des angemeldeten Benutzers ab und erstellt ein XML-Dokument, dessen Eingabestream zurückgegeben wird, um von den adaptiven Formularen genutzt zu werden.

Im folgenden Codefragment haben wir eine Klasse, die die DataProvider-Schnittstelle implementiert. Wir erhalten Zugriff auf den angemeldeten Benutzer und rufen dann die Profilinformationen des angemeldeten Benutzers ab. Anschließend erstellen wir ein XML-Dokument mit einem Stammknotenelement namens &quot;data&quot;und hängen entsprechende Elemente an diesen Datenknoten an. Nachdem das XML-Dokument erstellt wurde, wird der Eingabestream des XML-Dokuments zurückgegeben.

Diese Klasse wird dann in ein OSGi-Bundle umgewandelt und in AEM bereitgestellt. Sobald das Bundle bereitgestellt ist, ist dieser Vorbefüllungs-Dienst verfügbar, der als Vorbefüllungs-Dienst für Ihr adaptives Formular verwendet werden kann.

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

* [Laden Sie den Inhalt der ZIP-Datei herunter und extrahieren Sie ihn auf Ihren Computer](assets/prefillservice.zip)
* Vergewissern Sie sich, dass die angemeldeten [Benutzerprofile](http://localhost:4502/libs/granite/security/content/useradmin) vollständig ausgefüllt sind. Dies ist erforderlich, damit das Beispiel funktioniert. Für das Beispiel gibt es keine Fehlerprüfung auf fehlende Benutzerprofileigenschaften.
* Stellen Sie das Bundle mithilfe der [AEM Web-Konsole](http://localhost:4502/system/console/bundles) bereit.
* Erstellen eines adaptiven Formulars mit der XSD
* Verknüpfen Sie &quot;Custom AEM Form Pre Fill Service&quot;als Vorbefüllungs-Dienst für Ihr adaptives Formular
* Ziehen Sie Schemaelemente per Drag-and-Drop in das Formular
* Nutzen Sie die Funktion zur Formularvorschau.

>[!NOTE]
>
>Wenn das adaptive Formular auf XSD basiert, stellen Sie sicher, dass das vom Vorbefüllungs-Dienst zurückgegebene XML-Dokument mit der XSD übereinstimmt, auf der Ihr adaptives Formular basiert.
>
>Wenn das adaptive Formular nicht auf XSD basiert, müssen Sie die Felder manuell binden. Um beispielsweise ein Feld im adaptiven Formular an ein Fname-Element in den XML-Daten zu binden, verwenden Sie `/data/fname` in der Bindungsverweis des Felds für das adaptive Formular.

