---
title: Generieren des interaktiven Kommunikationsdokuments für den Druckkanal mithilfe des Mechanismus für überwachte Ordner
seo-title: Generieren des interaktiven Kommunikationsdokuments für den Druckkanal mithilfe des Mechanismus für überwachte Ordner
description: Verwenden des überwachten Ordners zum Generieren von Druckkanaldokumenten
seo-description: Verwenden des überwachten Ordners zum Generieren von Druckkanaldokumenten
feature: Interaktive Kommunikation
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 1%

---


# Generieren des interaktiven Kommunikationsdokuments für den Druckkanal mithilfe des Mechanismus für überwachte Ordner

Nachdem Sie Ihr Druckkanaldokument entworfen und getestet haben, müssen Sie in der Regel das Dokument generieren, indem Sie einen REST-Aufruf durchführen oder Druckdokumente mithilfe des Mechanismus zum Überwachen des Ordners generieren.

In diesem Artikel wird der Anwendungsfall zum Generieren von Druckkanaldokumenten mit dem Mechanismus für überwachte Ordner erläutert.

Wenn Sie eine Datei im überwachten Ordner ablegen, wird ein Skript ausgeführt, das mit dem überwachten Ordner verknüpft ist. Dieses Skript wird im unten stehenden Artikel erläutert.

Die Datei, die im überwachten Ordner abgelegt wird, weist die folgende Struktur auf. Der Code generiert Anweisungen für alle im XML-Dokument aufgelisteten Kontonummern.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

Der folgende Code führt Folgendes aus:

Zeile 1: Pfad zum InteractiveCommunicationsDocument

Zeilen 15-20: Abrufen der Liste der Kontonummern aus dem XML-Dokument, das im überwachten Ordner abgelegt wurde

Zeilen 24-25: Rufen Sie den PrintChannelService und den Druckkanal ab, die mit dem Dokument verknüpft sind.

Zeile 30: Übergeben Sie die Kontonummer als Schlüsselelement an das Formulardatenmodell.

Zeilen 32-36: Legen Sie die Datenoptionen für das zu erzeugende Dokument fest.

Zeile 38: Rendern Sie das Dokument.

Zeilen 39-40 - Speichert das generierte Dokument im Dateisystem.

Der REST-Endpunkt des Formulardatenmodells erwartet eine ID als Eingabeparameter. Diese ID wird einem Anforderungsattribut mit dem Namen account number zugeordnet, wie im Screenshot unten dargestellt.

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


**Um dies auf Ihrem lokalen System zu testen, folgen Sie den folgenden Anweisungen:**

* Richten Sie Tomcat wie in diesem [Artikel beschrieben ein.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat verfügt über die War-Datei, die die Beispieldaten generiert.
* Richten Sie den Dienst als Systembenutzer ein, wie in diesem [Artikel](/help/forms/adaptive-forms/service-user-tutorial-develop.md) beschrieben.
Stellen Sie sicher, dass dieser Systembenutzer über Leseberechtigungen für den folgenden Knoten verfügt. Um die Berechtigungen anzugeben, melden Sie sich bei [user admin](https://localhost:4502/useradmin) an und suchen Sie nach den &quot;Daten&quot;des Systembenutzers und geben Sie die Leseberechtigungen für den folgenden Knoten durch Navigieren zur Registerkarte &quot;Berechtigungen&quot;an
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importieren Sie die folgenden Pakete mit dem Package Manager in AEM. Dieses Paket enthält Folgendes:


* [Beispieldokument zur interaktiven Kommunikation](assets/retirementstatementprint.zip)
* [Skript für überwachte Ordner](assets/printchanneldocumentusingwatchedfolder.zip)
* [Datenquellkonfiguration](assets/datasource.zip)

* Öffnen Sie die Datei /etc/fd/watchfolder/scripts/PrintPDF.ecma . Stellen Sie sicher, dass der Pfad zum interaktiven Dokument in Zeile 1 auf das richtige Dokument verweist, das Sie drucken möchten

* Ändern Sie die saveLocation entsprechend Ihrer Voreinstellung in Zeile 2.

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


* Ziehen Sie die Datei &quot;account tnumbers.xml&quot;in den Ordner &quot;C:\RenderPrintChannel\input folder&quot;.

* Die generierten PDF-Dateien werden in die saveLocation geschrieben, wie im ECMA-Skript angegeben.

>[!NOTE]
>
>Wenn Sie dies auf einem Nicht-Windows-Betriebssystem verwenden möchten, navigieren Sie zu
>
>/etc/fd/watchfolder/config/PrintChannelDocument und ändern Sie den OrdnerPath gemäß Ihren Einstellungen

