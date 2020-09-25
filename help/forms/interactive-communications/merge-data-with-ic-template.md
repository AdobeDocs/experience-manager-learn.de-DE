---
title: Generieren von Print Kanal-Dokument durch Zusammenführen von Daten
seo-title: Generieren von Print Kanal-Dokument durch Zusammenführen von Daten
description: Erfahren Sie, wie Sie ein Dokument für den Druck von Kanal generieren, indem Sie Daten zusammenführen, die im Eingabestream enthalten sind
seo-description: Erfahren Sie, wie Sie ein Dokument für den Druck von Kanal generieren, indem Sie Daten zusammenführen, die im Eingabestream enthalten sind
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 2%

---

# Generieren von Print Kanal-Dokumenten mit gesendeten Daten

Print Kanal-Dokumente werden in der Regel durch Abrufen von Daten aus einer Back-End-Datenquelle über den get-Dienst des Formulardatenmodells generiert. In einigen Fällen müssen Sie eventuell Dokumente für den Druck mit den bereitgestellten Daten generieren. Beispiel: Der Kunde füllt das Formular des Empfängers aus und Sie können ein Dokument für den Kanal mit Daten aus dem gesendeten Formular generieren. Um diesen Verwendungsfall zu erreichen, müssen die folgenden Schritte befolgt werden

## Erstellen eines Prefill-Dienstes

Der Dienstname &quot;ccm-print-test&quot; wird verwendet, um auf diesen Dienst zuzugreifen. Sobald dieser Pre-fill-Dienst definiert ist, können Sie entweder in Ihrer Servlet- oder Workflow-Prozessschrittimplementierung auf diesen Dienst zugreifen, um das Print-Kanal-Dokument zu generieren.

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

### WorkflowProcess-Implementierung erstellen

Das Codefragment zur WorkflowProcess-Implementierung wird unten angezeigt. Dieser Code wird ausgeführt, wenn der Prozessschritt im AEM Workflow mit dieser Implementierung verknüpft ist. Diese Implementierung erwartet 3 Prozessargumente, die im Folgenden beschrieben werden:

* Name des DataFile-Pfads, der beim Konfigurieren des adaptiven Formulars angegeben wird
* Name der Druckvorlage für den Kanal
* Name des generierten Dokuments &quot;print Kanal&quot;

Zeile 98 - Da das adaptive Formular auf dem Formulardatenmodell basiert, werden die Daten, die sich in der Daten-Node von afBoundData befinden, extrahiert.
Zeile 128 - Der Dienstname für Datenoptionen ist festgelegt. Notieren Sie den Dienstnamen. Es muss mit dem in Zeile 45 der vorherigen Codeliste zurückgegebenen Namen übereinstimmen.
Zeile 135 - Dokument wird mithilfe der Render-Methode des PrintChannel-Objekts generiert


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

Gehen Sie wie folgt vor, um dies auf Ihrem Server zu testen:

* [Konfigurieren Sie den Day CQ Mail Service.](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) Dies ist erforderlich, um E-Mails mit dem als Anhang generierten Dokument zu senden.
* [Bereitstellen des Developer with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Vergewissern Sie sich, dass Sie den folgenden Eintrag in der Konfiguration des Apache Sling Service User Mapper Service hinzugefügt haben
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [Laden Sie die mit diesem Artikel verknüpften Assets auf Ihr Dateisystem herunter und dekomprimieren Sie sie.](assets/prefillservice.zip)
* [Importieren Sie die folgenden Pakete mit dem AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Stellen Sie Folgendes mithilfe AEM Felix Web Console bereit](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Dieses Bundle enthält den in diesem Artikel erwähnten Code.

* [Open ChangeOfBeneficiaryForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Vergewissern Sie sich, dass das adaptive Formular wie unten gezeigt für das Senden an AEM Workflow konfiguriert ist.
   ![image](assets/generateic.PNG)
* [Konfigurieren Sie das Workflow-Modell.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Stellen Sie sicher, dass Prozessschritt- und Senden-E-Mail-Komponenten gemäß Ihrer Umgebung konfiguriert sind.
* [Vorschau der ChangeOfBeneficiaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) Füllen Sie einige Details aus und senden Sie
* Der Workflow sollte aufgerufen werden, und das IC-Kanal-Dokument sollte an den Empfänger gesendet werden, der in der Komponente &quot;E-Mail senden&quot;als Anlage angegeben ist
