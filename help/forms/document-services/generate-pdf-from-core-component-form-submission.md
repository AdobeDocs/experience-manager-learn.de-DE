---
title: Generieren von PDF mit Daten aus einem auf Kernkomponenten basierenden adaptiven Formular
description: Zusammenführen von Daten aus der Kernkomponentenbasierten Formularübermittlung mit der XDP-Vorlage im Workflow
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
source-git-commit: acfd982ce471c8510cb59ed4d353f1d47dcfb5dc
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 2%

---

# Generieren von PDF mit Daten aus der auf Kernkomponenten basierenden Formularübermittlung

Im Folgenden finden Sie den überarbeiteten Text mit großgeschriebenen &quot;Kernkomponenten&quot;:

Ein typisches Szenario besteht darin, eine PDF aus Daten zu generieren, die über ein auf Kernkomponenten basierendes adaptives Formular übermittelt werden. Diese Daten liegen immer im JSON-Format vor. Um eine PDF mithilfe der Render PDF API zu generieren, müssen die JSON-Daten in das XML-Format konvertiert werden. Die `toString` Methode `org.json.XML` wird für diese Konvertierung verwendet. Weitere Informationen finden Sie im Abschnitt [Dokumentation `org.json.XML.toString` method](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## Adaptives Formular basierend auf dem JSON-Schema

Führen Sie die folgenden Schritte aus, um ein JSON-Schema für Ihr adaptives Formular zu erstellen:

### Generieren von Beispieldaten für die XDP

Um den Prozess zu optimieren, führen Sie die folgenden verfeinerten Schritte aus:

1. Öffnen Sie die XDP-Datei in AEM Forms Designer.
1. Navigieren Sie zu &quot;Datei&quot;> &quot;Formulareigenschaften&quot;> &quot;Vorschau&quot;.
1. Wählen Sie &quot;Vorschaudaten generieren&quot;.
1. Klicken Sie auf &quot;Erzeugen&quot;.
1. Weisen Sie einen aussagekräftigen Dateinamen zu, z. B. `form-data.xml`.

### JSON-Schema aus den XML-Daten generieren

Sie können jedes kostenlose Online-Tool nutzen, um [XML in JSON konvertieren](https://jsonformatter.org/xml-to-jsonschema) unter Verwendung der im vorherigen Schritt generierten XML-Daten.

### Benutzerdefinierter Workflow-Prozess zum Konvertieren von JSON in XML

Der bereitgestellte Code konvertiert JSON in XML und speichert die resultierende XML in einer Workflow-Prozessvariablen mit dem Namen `dataXml`.

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### Erstellen eines Workflows

Erstellen Sie einen Workflow, der zwei Schritte umfasst, um Formularübermittlungen zu handhaben:

1. Der erste Schritt verwendet einen benutzerdefinierten Prozess, um die gesendeten JSON-Daten in XML umzuwandeln.
1. Der folgende Schritt generiert eine PDF, indem die XML-Daten mit der XDP-Vorlage kombiniert werden.

![json-to-xml](assets/json-to-xml-process-step.png)


## Bereitstellen des Beispielcodes

Führen Sie die folgenden rationalisierten Schritte aus, um dies auf Ihrem lokalen Server zu testen:

1. [Laden Sie das benutzerdefinierte Bundle über die AEM OSGi-Web-Konsole herunter und installieren Sie es.](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [Importieren des Workflow-Pakets](assets/workflow_to_render_pdf.zip).
1. [Importieren der Beispiel-Vorlage für adaptives Formular und XDP](assets/adaptive_form_and_xdp_template.zip).
1. [Vorschau des adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. Füllen Sie einige Formularfelder aus.
1. Senden Sie das Formular, um den AEM-Workflow zu starten.
1. Suchen Sie die gerenderte PDF im Payload-Ordner des Workflows.

