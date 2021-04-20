---
title: Generieren von interaktivem Communications-Dokument für den Druck von Kanal mithilfe des Mechanismus für überwachte Ordner
seo-title: Generieren von interaktivem Communications-Dokument für den Druck von Kanal mithilfe des Mechanismus für überwachte Ordner
description: Verwenden des überwachten Ordners zum Generieren von Dokumenten für den Kanal
seo-description: Verwenden des überwachten Ordners zum Generieren von Dokumenten für den Kanal
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 1%

---


# Generieren von interaktivem Communications-Dokument für den Druck von Kanal mithilfe des Mechanismus für überwachte Ordner

Nachdem Sie Ihr Print-Kanal-Dokument entworfen und getestet haben, müssen Sie das Dokument in der Regel durch einen REST-Aufruf generieren oder mithilfe des Mechanismus für überwachte Ordner gedruckte Dokumente generieren.

In diesem Artikel wird das Verwendungsbeispiel für das Generieren von Dokumenten für den Druckordner mithilfe des Mechanismus für überwachte Kanäle erläutert.

Wenn Sie eine Datei im überwachten Ordner ablegen, wird ein Skript ausgeführt, das mit dem überwachten Ordner verknüpft ist. Dieses Skript wird im unten stehenden Artikel beschrieben.

Die Datei, die in den überwachten Ordner abgelegt wird, hat die folgende Struktur. Der Code generiert Anweisungen für alle im XML-Dokument aufgelisteten Kontonummern.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

Die nachstehende Codeliste umfasst Folgendes:

Zeile 1: Pfad zum InteractiveCommunicationsDocument

Zeilen 15-20: Abrufen der Liste der Kontonummern aus dem XML-Dokument, das im überwachten Ordner abgelegt wird

Zeilen 24-25: Besorgen Sie sich den PrintChannelService- und Print-Kanal, der mit dem Dokument verknüpft ist.

Linie 30: Übergeben Sie die Kontonummer als Schlüsselelement an das Formulardatenmodell.

Zeilen 32-36: Legen Sie die Datenoptionen für das zu generierende Dokument fest.

Linie 38: Dokument rendern

Zeilen 39-40 - Speichert das generierte Dokument im Dateisystem.

Der REST-Endpunkt des Formulardatenmodells erwartet eine ID als Eingabeparameter. Diese ID wird einem Anforderungsattribut mit dem Namen accountNumber zugeordnet, wie im folgenden Screenshot dargestellt.

![requestattribute](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**Um dies auf Ihrem lokalen System zu testen, befolgen Sie die folgenden Anweisungen:**

* Setup Tomcat wie in diesem [Artikel beschrieben.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat verfügt über die Kriegsdatei, die die Musterdaten generiert.
* Richten Sie den Dienst als Systembenutzer ein, wie in diesem [Artikel](/help/forms/adaptive-forms/service-user-tutorial-develop.md) beschrieben.
Vergewissern Sie sich, dass dieser Systembenutzer über Leserechte für den folgenden Knoten verfügt. So melden Sie sich mit den Berechtigungen bei [user admin](https://localhost:4502/useradmin) an und suchen Sie nach den Systembenutzern &quot;data&quot;. Weisen Sie den folgenden Knoten Leserechte zu, indem Sie auf die Registerkarte &quot;Berechtigungen&quot;klicken
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importieren Sie die folgenden Pakete mit dem Package Manager in AEM. Dieses Paket enthält Folgendes:


* [Beispiel für ein interaktives Kommunikationsnetzwerk](assets/retirementstatementprint.zip)
* [Skript für überwachte Ordner](assets/printchanneldocumentusingwatchedfolder.zip)
* [Datenquellkonfiguration](assets/datasource.zip)

* Öffnen Sie die Datei &quot;/etc/fd/watchfolder/scripts/PrintPDF.ecma&quot;. Stellen Sie sicher, dass der Pfad zum interactiveCommunicationsDocument in Zeile 1 auf das richtige Dokument verweist, das Sie drucken möchten.

* Ändern Sie die saveLocation gemäß Ihrer Voreinstellung in Zeile 2

* Erstellen Sie die Datei &quot;accountnumbers.xml&quot;mit folgendem Inhalt

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* Legen Sie die Datei &quot;accountnumbers.xml&quot;im Ordner &quot;C:\RenderPrintChannel\input folder&quot;ab.

* Die generierten PDF-Dateien werden wie im ECMA-Skript angegeben in saveLocation geschrieben.

>[!NOTE]
>
>Wenn Sie dies auf einem nicht Windows-Betriebssystem verwenden möchten, navigieren Sie zu
>
>/etc/fd/watchfolder/config/PrintChannelDocument und ändern Sie folderPath entsprechend Ihren Voreinstellungen

