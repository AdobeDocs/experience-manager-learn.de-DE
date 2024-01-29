---
title: Auflisten benutzerdefinierter Asset-Typen in AEM Forms
description: Teil 2 – Auflisten benutzerdefinierter Asset-Typen in AEM Forms
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
duration: 163
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '584'
ht-degree: 100%

---

# Auflisten benutzerdefinierter Asset-Typen in AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Erstellen einer benutzerdefinierten Vorlage {#creating-custom-template}

Für die Zwecke dieses Artikels erstellen wir eine benutzerdefinierte Vorlage, um die benutzerdefinierten Asset-Typen und die vordefinierten Asset-Typen auf derselben Seite anzuzeigen. Um eine benutzerdefinierte Vorlage zu erstellen, befolgen Sie die nachstehenden Anweisungen.

1. Erstellen Sie einen Ordner „sling:“ unter „/apps“. Nennen Sie ihn „myportalcomponent“.
1. Fügen Sie eine „fpContentType“-Eigenschaft hinzu. Setzen Sie deren Wert auf „**/libs/fd/fp/formTemplate**“.
1. Fügen Sie eine „title“-Eigenschaft hinzu und setzen Sie ihren Wert auf „benutzerdefinierte Vorlage“. Dies ist der Name, der in der Dropdown-Liste der Komponente „Suche und Auflister“ zu sehen sein wird.
1. Erstellen Sie unter diesem Ordner die Datei „template.html“. Diese Datei enthält den Code zum Formatieren und Anzeigen der verschiedenen Asset-Typen.

![Apps-Ordner](assets/appsfolder_.png)

Mit dem folgenden Code werden die verschiedenen Asset-Typen über die Komponente „Suche und Auflister“ aufgelistet. Wir erstellen separate HTML-Elemente für jeden Asset-Typ, wie durch das folgende Tag gezeigt: data-type = &quot;videos&quot;. Für den Asset-Typ „Videos“ verwenden wir das &lt;video>-Element, um das Video inline wiederzugeben. Für den Asset-Typ „worddocuments“ verwenden wir eine andere HTML-Kennzeichnung.

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
>Zeile 11: Ändern Sie „image src“ so, dass auf ein Bild Ihrer Wahl in DAM verwiesen wird.
>
>Um adaptive Formulare in dieser Vorlage aufzulisten, erstellen Sie ein neues div-Element und legen Sie sein Attribut „data-type“ auf &quot;guide&quot; fest. Sie können das div-Element, das über „printForm“ als Datentyp verfügt, kopieren und einfügen und den Datentyp des neu kopierten div-Elements wie folgt festlegen: data type = &quot;guide&quot;.

## Konfigurieren der Komponente „Suche und Auflister“ {#configure-search-and-lister-component}

Nachdem wir die benutzerdefinierte Vorlage definiert haben, müssen wir diese benutzerdefinierte Vorlage nun mit der Komponente „Suche und Auflister“ verknüpfen. Verweisen Sie den Browser [zu dieser URL](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Wechseln Sie in den Design-Modus und konfigurieren Sie das Absatzsystem so, dass die Komponente „Suche und Auflister“ in die Gruppe der zulässigen Komponenten aufgenommen wird. Die Komponente „Suche und Auflister“ ist Teil der Gruppe „Dokumentendienste“.

Wechseln Sie in den Bearbeitungsmodus und fügen Sie die Komponente „Suche und Auflister“ zu „ParSys“ hinzu.

Öffnen Sie die Konfigurationseigenschaften der Komponente „Suche und Auflister“. Stellen Sie sicher, dass die Registerkarte „Asset-Ordner“ ausgewählt ist. Wählen Sie die Ordner aus, deren Assets in der Komponente „Suche und Auflister“ aufgelistet werden sollen. Für die Zwecke dieses Artikels wurde Folgendes ausgewählt:

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![Asset-Ordner](assets/selectingassetfolders.png)

Wechseln Sie zur Registerkarte „Anzeige“. Hier wählen Sie die Vorlage aus, für die die Assets in der Komponente „Suche und Auflister“ angezeigt werden sollen.

Wählen Sie „benutzerdefinierte Vorlage“ aus der Dropdown-Liste aus, wie unten dargestellt.

![Suche und Auflister](assets/searchandlistercomponent.gif)

Konfigurieren Sie die Asset-Typen, die im Portal aufgelistet werden sollen. Wechseln Sie zur Registerkarte „Asset-Liste“, um die Asset-Typen zu konfigurieren. In diesem Beispiel haben wir die folgenden Asset-Typen konfiguriert:

1. MP4-Dateien
1. Word-Dokumente
1. Dokument (der vordefinierte Asset-Typ)
1. Formularvorlage (der vordefinierte Asset-Typ)

Der folgende Screenshot zeigt die Asset-Typen, die für die Auflistung konfiguriert sind.

![Asset-Typen](assets/assettypes.png)

Nun, da Sie Ihre Suche und Auflister-Portalkomponente konfiguriert haben, ist es an der Zeit, den Auflister in Aktion zu sehen. Rufen Sie mit Ihrem Browser [diese URL](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled) auf. Die Ergebnisse sollten ungefähr dem unten gezeigten Bild entsprechen.

>[!NOTE]
>
>Wenn Ihr Portal benutzerdefinierte Asset-Typen auf einem Veröffentlichungs-Server auflistet, stellen Sie sicher, dass Sie der Benutzerin bzw. dem Benutzer „fd-service“ eine Leseberechtigung für den Knoten **/apps/fd/fp/extensions/querybuilder** erteilen.

![assettypes](assets/assettypeslistings.png)
[Laden Sie dieses Paket mit Package Manager herunter und installieren Sie es.](assets/customassettypekt1.zip) Dies enthält Beispiel-MP4- und Word-Dokumente sowie XDP-Dateien, die als Asset-Typen verwendet werden, um sie mithilfe der Suche und Auflister-Komponente aufzulisten.
