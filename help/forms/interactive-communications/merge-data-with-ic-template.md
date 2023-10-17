---
title: Generieren des Druckkanaldokuments durch Zusammenführen von Daten
description: Erfahren Sie, wie Sie ein Dokument für den Druckkanal generieren, indem Sie die im Eingabe-Stream enthaltenen Daten zusammenführen.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 3bfbb4ef-0c51-445a-8d7b-43543a5fa191
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '479'
ht-degree: 100%

---

# Generieren von Druckkanaldokumenten mithilfe gesendeter Daten

Druckkanaldokumente werden in der Regel durch Abrufen von Daten aus einer Backend-Datenquelle über den Get-Dienst des Formulardatenmodells generiert. In einigen Fällen müssen Sie möglicherweise Druckkanaldokumente mit den bereitgestellten Daten generieren. Beispiel: Die Kundin oder der Kunde füllt ein Formular zur Änderung der bzw. des Begünstigten aus, und Sie können ein Druckkanaldokument mit Daten aus dem gesendeten Formular generieren. Gehen Sie wie folgt vor, um dieses Anwendungsbeispiel zu übernehmen:

## Erstellen eines Vorbefüllungsdienstes

Der Dienstname „ccm-print-test“ wird für den Zugriff auf diesen Dienst verwendet. Sobald dieser Vorbefüllungsdienst definiert ist, können Sie in Ihrer Servlet- oder Workflow-Prozessschrittimplementierung darauf zugreifen, um das Druckkanaldokument zu generieren.

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### Erstellen der WorkflowProcess-Implementierung

Das Code-Snippet zur WorkflowProcess-Implementierung wird unten gezeigt. Dieser Code wird ausgeführt, wenn der Prozessschritt im AEM-Workflow mit dieser Implementierung verknüpft ist. Diese Implementierung erwartet 3 Prozessargumente, die unten beschrieben werden:

* Name des DataFile-Pfads, der beim Konfigurieren des adaptiven Formulars angegeben wird
* Name der Druckkanalvorlage
* Name des generierten Druckkanaldokuments

Zeile 98 – Da das adaptive Formular auf dem Formulardatenmodell basiert, werden die Daten, die sich im Datenknoten von afBoundData befinden, extrahiert.
Zeile 128 – Der Name des Datenoptionen-Dienstes wird festgelegt. Notieren Sie den Dienstnamen. Er muss mit dem in Zeile 45 der vorherigen Code-Auflistung zurückgegebenen Namen übereinstimmen.
Zeile 135 – Dokument wird mithilfe der Rendermethode des PrintChannel-Objekts generiert.


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

Führen Sie die folgenden Schritte aus, um dies auf Ihrem Server zu testen:

* [Konfigurieren Sie den Day CQ-Mail-Service.](https://helpx.adobe.com/de/experience-manager/6-5/communities/using/email.html)Dies ist erforderlich, um E-Mails mit dem als Anhang erstellten Dokument zu senden.
* [Stellen Sie das Bundle zur Entwicklung mit Dienstbenutzenden bereit](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Stellen Sie sicher, dass Sie den folgenden Eintrag in der Konfiguration des Apache Sling Service User Mapper Service hinzugefügt haben
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [Laden Sie die Assets, die mit diesem Artikel in Verbindung stehen, herunter und entpacken Sie sie in Ihr Dateisystem.](assets/prefillservice.zip)
* [Importieren Sie die folgenden Pakete mit AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Stellen Sie Folgendes mithilfe der AEM Felix-Web-Konsole bereit](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Dieses Bundle enthält den in diesem Artikel erwähnten Code.

* [Öffnen Sie ChangeOfBeneficiaryForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Stellen Sie sicher, dass das adaptive Formular für die Übermittlung an AEM Workflow konfiguriert ist, wie unten gezeigt.
  ![Bild](assets/generateic.PNG)
* [Konfigurieren Sie das Workflow-Modell.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Stellen Sie sicher, dass die Prozessschritte und E-Mail-Komponenten gemäß Ihrer Umgebung konfiguriert sind.
* [Erstellen Sie eine Vorschau der ChangeOfBeneficiaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)Füllen Sie einige Details aus und senden Sie es.
* Der Workflow sollte aufgerufen und das IC-Druckkanaldokument als Anlage an die Empfängerinnen bzw. Empfänger gesendet werden, die in der gesendeten E-Mail-Komponente angegeben sind.
