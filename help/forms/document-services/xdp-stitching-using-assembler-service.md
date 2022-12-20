---
title: XDP-Stitching mit Assembler-Dienst
description: Verwenden des Assembler-Dienstes in AEM Forms zum Zuordnen von xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# XDP-Zuordnung mit Assembler-Dienst

In diesem Artikel finden Sie die Assets, mit denen Sie die Fähigkeit demonstrieren können, XDP-Dokumente mithilfe des Assembler-Dienstes zuzuordnen.
Der folgende JSP-Code wurde geschrieben, um ein Teilformular namens **Adresse** aus dem xdp-Dokument address.xdp in einen Einfügepunkt namens **Adresse** im Dokument Übergeordnet.xdp . Die resultierende xdp wurde im Stammordner Ihrer AEM-Installation gespeichert.

Der Assembler-Dienst nutzt gültige DDX-Dokumente, um die Manipulation von PDF-Dokumenten zu beschreiben. Weitere Informationen finden Sie unter [DDX-Referenzdokument hier](assets/ddxRef.pdf).Seite 40 enthält Informationen zum XDP-Stitching.

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

Die DDX-Datei zum Einfügen von Fragmenten in eine andere xdp ist unten aufgeführt. Das DDX fügt das Teilformular ein.  **Adresse** von address.xdp in den Einfügepunkt namens **Adresse** in der Datei Übergeordnet.xdp. Das resultierende Dokument namens **stitched.xdp** wird im Dateisystem gespeichert.

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

So können Sie diese Funktion auf Ihrem AEM-Server verwenden

* Download [XDP-Stitching-Paket](assets/xdp-stitching.zip) auf Ihr lokales System.
* Laden Sie das Paket hoch und installieren Sie es mithilfe des [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Extrahieren Sie den Inhalt dieser ZIP-Datei.](assets/xdp-and-ddx.zip) Abrufen der Beispiel-XDP- und DDX-Datei

**Nachdem Sie das Paket installiert haben, müssen Sie die folgenden URLs in Adobe Granite CSRF Filter in Zulassungsliste setzen.**

1. Führen Sie die unten genannten Schritte aus, um die oben genannten Pfade in Zulassungslisten anzuzeigen.
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Adobe Granite CSRF Filter .
1. Fügen Sie den folgenden Pfad in die ausgeschlossenen Abschnitte hinzu und speichern Sie `/content/AemFormsSamples/assemblerservice`
1. Suchen Sie nach &quot;Sling Referrer filter&quot;.
1. Aktivieren Sie das Kontrollkästchen &quot;Leere erlauben&quot;. (Diese Einstellung sollte nur zu Testzwecken verwendet werden.) Es gibt verschiedene Möglichkeiten, den Beispielcode zu testen. Am schnellsten und einfachsten ist die Verwendung der Postman-App. Mit Postman können Sie POST-Anfragen an Ihren Server richten. Installieren Sie das Postman-Programm auf Ihrem System.
Starten Sie die App und geben Sie die folgende URL ein, um die Export-Daten-API http://localhost:4502/content/AemFormsSamples/assemblerservice.html zu testen.

Geben Sie die folgenden Eingabeparameter an, wie im Screenshot angegeben. Sie können die zuvor heruntergeladenen Beispieldokumente verwenden.
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Stellen Sie sicher, dass die AEM Forms-Installation abgeschlossen ist. Alle Pakete müssen sich im aktiven Status befinden.
