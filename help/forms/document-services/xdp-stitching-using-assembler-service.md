---
title: Zuordnen von XDP-Dokumenten mit dem Assembler-Dienst
description: Zuordnen von XDP-Dokumenten mit dem Assembler-Dienst in AEM Forms
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 130
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 100%

---

# Zuordnen von XDP-Dokumenten mit dem Assembler-Dienst

In diesem Artikel finden Sie die Assets, um auszuprobieren, wie XDP-Dokumente mithilfe des Assembler-Dienstes zugeordnet werden können.
Der folgende JSP-Code wurde geschrieben, um das Teilformular **address** aus dem XDP-Dokument „address.xdp“ in den Einfügepunkt **address** im Dokument „master.xdp“ einzufügen. Die resultierende XDP-Datei wurde im Stammordner Ihrer AEM-Installation gespeichert.

Der Assembler-Dienst ist auf gültige DDX-Dokumente angewiesen, um die Änderung von PDF-Dokumenten zu beschreiben. Weitere Informationen finden Sie im [DDX-Referenzdokument](assets/ddxRef.pdf). Seite 40 enthält Informationen zum Zuordnen von XDP-Dokumenten.

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

Die DDX-Datei zum Einfügen von Fragmenten in eine andere XDP-Datei ist unten aufgeführt. Die DDX fügt das Teilformular **address** aus „address.xdp“ in den Einfügepunkt **address** in der Datei „master.xdp“ ein. Das resultierende Dokument namens **stitched.xdp** wird im Dateisystem gespeichert.

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

Sie können diese Funktion wie folgt auf Ihrem AEM-Server verwenden:

* Laden Sie das [xdp-stitching-Paket](assets/xdp-stitching.zip) auf Ihr lokales System herunter.
* Verwenden Sie [Package Manager](http://localhost:4502/crx/packmgr/index.jsp), um das Paket hochzuladen und zu installieren.
* [Extrahieren Sie den Inhalt dieser ZIP-Datei](assets/xdp-and-ddx.zip), um die XDP-Beispieldateien und die entsprechende DDX-Datei zu erhalten.

**Nachdem Sie das Paket installiert haben, müssen Sie die folgenden URLs unter „Adobe Granite CSRF Filter“ auf die Zulassungsliste setzen.**

1. Führen Sie die folgenden Schritte aus, um die oben genannten Pfade auf die Zulassungsliste zu setzen.
1. [Melden Sie sich bei configMgr an.](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach „Adobe Granite CSRF Filter“.
1. Fügen Sie den folgenden Pfad in den ausgeschlossenen Abschnitten hinzu und speichern Sie `/content/AemFormsSamples/assemblerservice`.
1. Suchen Sie nach „Sling Referrer filter“.
1. Aktivieren Sie das Kontrollkästchen „Leere zulassen“. (Diese Einstellung sollte nur zu Testzwecken verwendet werden.)
Es gibt verschiedene Möglichkeiten, den Beispiel-Code zu testen. Am schnellsten und einfachsten lässt sich hier die Postman-App verwenden. Mit Postman können Sie POST-Anfragen an Ihren Server richten. Installieren Sie die Postman-App auf Ihrem System.
Starten Sie die App und geben Sie die folgende URL ein, um die Export-Daten-API zu testen:
http://localhost:4502/content/AemFormsSamples/assemblerservice.html.

Geben Sie die folgenden Eingabeparameter an, wie im Screenshot zu sehen. Sie können die zuvor heruntergeladenen Beispieldokumente verwenden.
![Postman – Zusammenfügen von XDP-Dateien](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Stellen Sie sicher, dass Sie Ihre AEM Forms-Installation abgeschlossen haben. Alle Bundles müssen sich in einem aktiven Status befinden.
>
