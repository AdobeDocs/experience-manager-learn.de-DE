---
title: Auflisten benutzerdefinierter Asset-Typen in AEM Forms
seo-title: Auflisten benutzerdefinierter Asset-Typen in AEM Forms
description: Teil 2 der Auflistung benutzerdefinierter Asset-Typen in AEM Forms
seo-description: Teil 2 der Auflistung benutzerdefinierter Asset-Typen in AEM Forms
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---


# Auflisten benutzerdefinierter Asset-Typen in AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Erstellen einer benutzerdefinierten Vorlage {#creating-custom-template}


Für die Zwecke dieses Artikels erstellen wir eine benutzerdefinierte Vorlage, um die benutzerdefinierten Asset-Typen und OOTB-Asset-Typen auf derselben Seite anzuzeigen. Um eine benutzerdefinierte Vorlage zu erstellen, befolgen Sie die folgenden Anweisungen

1. Erstellen eines Sling: Ordner unter /apps. Benennen Sie es &quot;myportalcomponent &quot;
1. Fügen Sie eine fpContentType-Eigenschaft hinzu. Setzen Sie den Wert auf &quot;**/libs/fd/ fp/formTemplate&quot;.**
1. Fügen Sie die Eigenschaft &quot;title&quot;hinzu und legen Sie ihren Wert auf &quot;custom template&quot;fest. Dies ist der Name, den Sie in der Dropdown-Liste der Komponente &quot;Search &amp; Lister&quot;sehen
1. Erstellen Sie unter diesem Ordner eine &quot;template.html&quot;. Diese Datei enthält den Code zum Formatieren und Anzeigen der verschiedenen Asset-Typen.

![appsfolder](assets/appsfolder_.png)

Der folgende Code listet die verschiedenen Asset-Typen auf, die die Such- und Lister-Komponente verwenden. Wir erstellen separate HTML-Elemente für jeden Asset-Typ, wie durch das Tag &quot;data-type = &quot;videos&quot; gezeigt. Für den Asset-Typ &quot;Videos&quot;verwenden wir das Element &lt;video>, um das Video inline wiederzugeben. Für den Asset-Typ von &quot;worddocuments&quot;verwenden wir unterschiedliche HTML-Markierungen.

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>Zeile 11 - Ändern Sie die Bild-src so, dass sie auf ein Bild Ihrer Wahl in DAM verweist.
>
>Um Adaptive Forms in dieser Vorlage aufzulisten, erstellen Sie ein neues div und setzen Sie sein Datentypattribut auf &quot;guide&quot;. Sie können das div kopieren und einfügen, dessen data-type=&quot;printForm&quot;ist, und den Datentyp des neu kopierten div auf &quot;guide&quot;setzen

## Konfigurieren der Such- und Listener-Komponente {#configure-search-and-lister-component}

Nachdem wir die benutzerdefinierte Vorlage definiert haben, müssen wir diese benutzerdefinierte Vorlage nun mit der Komponente &quot;Search &amp; Lister&quot;verknüpfen. Zeigen Sie Ihren Browser [auf diese URL ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Wechseln Sie in den Designmodus und konfigurieren Sie das Absatzsystem so, dass die Komponente &quot;Search And Lister&quot;in die Gruppe der zulässigen Komponenten aufgenommen wird. Die Komponente &quot;Search &amp; Lister&quot;ist Teil der Gruppe &quot;Document Services&quot;.

Wechseln Sie in den Bearbeitungsmodus und fügen Sie die Komponente &quot;Search and Lister&quot;zu ParSys hinzu.

Öffnen Sie die Konfigurationseigenschaften der Komponente &quot;Search and Lister&quot;. Stellen Sie sicher, dass die Registerkarte &quot;Asset-Ordner&quot;ausgewählt ist. Wählen Sie die Ordner aus, aus denen Sie die Assets in der Such- und Lister-Komponente auflisten möchten. Für die Zwecke dieses Artikels habe ich

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Registerkarte zur Registerkarte &quot;Anzeige&quot;. Hier wählen Sie die Vorlage aus, die die Assets in der Such- und Lister-Komponente anzeigen soll.

Wählen Sie &quot;benutzerdefinierte Vorlage&quot;aus der Dropdown-Liste aus, wie unten dargestellt.

![searchandlister](assets/searchandlistercomponent.gif)

Konfigurieren Sie die Asset-Typen, die Sie im Portal auflisten möchten. So konfigurieren Sie die Asset-Typen auf der Registerkarte &quot;Asset-Auflistung&quot;und konfigurieren die Asset-Typen. In diesem Beispiel haben wir die folgenden Asset-Typen konfiguriert

1. MP4-Dateien
1. Word-Dokumente
1. Document(Dies ist der OOTB-Asset-Typ.)
1. Formularvorlage(Dies ist der OOTB-Asset-Typ)

Der folgende Screenshot zeigt die Asset-Typen, die für die Auflistung konfiguriert sind

![assettypes](assets/assettypes.png)

Nachdem Sie die Komponente &quot;Search &amp; Lister Portal&quot;konfiguriert haben, ist es an der Zeit, den Listener in Aktion zu sehen. Zeigen Sie Ihren Browser [auf diese URL ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). Die Ergebnisse sollten ungefähr dem unten gezeigten Bild entsprechen.

>[!NOTE]
>
>Wenn Ihr Portal benutzerdefinierte Asset-Typen auf einem Veröffentlichungsserver auflistet, stellen Sie sicher, dass Sie dem Benutzer &quot;fd-service&quot;die &quot;Leseberechtigung&quot;für den Knoten **/apps/fd/fp/extensions/querybuilder** erteilen.

![](assets/assettypeslistings.png)
[assettypesBitte laden Sie dieses Paket mit Package Manager herunter und installieren Sie es.](assets/customassettypekt1.zip) Dies enthält Beispiel-MP4- und Word-Dokumente sowie XDP-Dateien, die als Asset-Typen verwendet werden, um mithilfe der Such- und Lister-Komponente aufzulisten.
