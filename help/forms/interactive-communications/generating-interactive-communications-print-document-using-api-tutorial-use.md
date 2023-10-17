---
title: Generieren eines interaktiven Kommunikationsdokuments für den Druckkanal mit dem Mechanismus für überwachte Ordner
seo-title: Generating Interactive Communications Document for print channel using watch folder mechanism
description: Verwenden eines überwachten Ordners zum Generieren von Druckkanaldokumenten
seo-description: Use watched folder to generate print channel documents
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '463'
ht-degree: 100%

---

# Generieren eines interaktiven Kommunikationsdokuments für den Druckkanal mit dem Mechanismus für überwachte Ordner

Nach dem Entwerfen und Testen Ihres Druckkanaldokuments müssen Sie in der Regel das Dokument über einen REST-Aufruf oder Druckdokumente mithilfe des Mechanismus für überwachte Ordner generieren.

In diesem Artikel wird der Anwendungsfall zum Generieren von Druckkanaldokumenten mit dem Mechanismus für überwachte Ordner erläutert.

Wenn Sie eine Datei im überwachten Ordner ablegen, wird ein mit dem überwachten Ordner verknüpftes Skript ausgeführt. Dieses Skript wird im unten stehenden Artikel erläutert.

Die im überwachten Ordner abgelegte Datei weist die folgende Struktur auf. Der Code generiert Anweisungen für alle im XML-Dokument aufgelisteten accountnumber-Attribute.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

Der nachstehende Code bewirkt Folgendes:

Zeile 1: Gibt den Pfad zum „InteractiveCommunicationsDocument“ an.

Zeilen 15–20: Hiermit wird die accountnumber-Liste aus dem im überwachten Ordner abgelegten XML-Dokument abgerufen.

Zeilen 24–25: Hiermit werden der PrintChannelService und der Druckkanal abgerufen, die mit dem Dokument verknüpft sind.

Zeile 30: Übergibt die accountnumber als Schlüsselelement an das Formulardatenmodell.

Zeilen 32–36: Hiermit werden die Datenoptionen für das zu generierende Dokument festgelegt.

Zeile 38: Rendert das Dokument.

Zeilen 39-40: Hiermit wird das generierte Dokument im Dateisystem gespeichert.

Der REST-Endpunkt des Formulardatenmodells erwartet eine ID als Eingabeparameter. Diese ID wird dem Anfrageattribut „accountnumber“ zugeordnet, wie im Screenshot unten dargestellt.

![Anfrageattribut](assets/requestattributeprintchannel.gif)

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


**Um dies auf Ihrem lokalen System zu testen, befolgen Sie die nachstehenden Anweisungen:**

* Richten Sie Tomcat ein, wie in diesem [Artikel](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) beschrieben. Tomcat verfügt über die WAR-Datei, die die Beispieldaten generiert.
* Richten Sie den Dienst (die Systembenutzerin oder den Systembenutzer) ein, wie in diesem [Artikel](/help/forms/adaptive-forms/service-user-tutorial-develop.md) beschrieben.
Stellen Sie sicher, dass diese Systembenutzerin bzw. dieser Systembenutzer über Leseberechtigungen für die folgenden Knoten verfügt. Um diese Berechtigungen zu gewähren, melden Sie sich als [Benutzeradmin](https://localhost:4502/useradmin) an und suchen Sie nach den „Daten“ der Systembenutzerin bzw. des Systembenutzers und erteilen Sie die Leseberechtigungen für die folgenden Knoten, indem Sie die Registerkarte „Berechtigungen“ aufrufen:
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importieren Sie das folgende Paket mit Package Manager in AEM. Dieses Paket enthält Folgendes:


* [Beispieldokument zur interaktiven Kommunikation](assets/retirementstatementprint.zip)
* [Skript für überwachte Ordner](assets/printchanneldocumentusingwatchedfolder.zip)
* [Datenquellkonfiguration](assets/datasource.zip)

* Öffnen Sie die Datei „/etc/fd/watchfolder/scripts/PrintPDF.ecma“. Stellen Sie sicher, dass der Pfad zum „interactiveCommunicationsDocument“ in Zeile 1 auf das richtige Dokument verweist, das gedruckt werden soll.

* Ändern Sie den Speicherort („saveLocation“) wie gewünscht in Zeile 2.

* Erstellen Sie eine Datei „accountnumbers.xml“ mit folgendem Inhalt:

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


* Legen Sie die Datei „accountnumbers.xml“ im Ordner „C:\RenderPrintChannel\input“ ab.

* Die generierten PDF-Dateien werden an dem im ECMA-Skript angegebenen Speicherort („saveLocation“) geschrieben.

>[!NOTE]
>
>Wenn Sie dies auf einem Nicht-Windows-Betriebssystem verwenden möchten, navigieren Sie zu
>
>„/etc/fd/watchfolder/config/PrintChannelDocument“ und ändern Sie „folderPath“ wie gewünscht.
