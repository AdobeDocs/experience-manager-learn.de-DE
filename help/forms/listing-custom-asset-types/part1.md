---
title: Registrieren benutzerdefinierter Asset-Typen
seo-title: Registering Custom Asset Types
description: Aktivieren benutzerdefinierter Asset-Typen für die Auflistung in AEM Forms Portal
seo-description: Enabling custom asset types for listing in AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 3%

---

# Registrieren benutzerdefinierter Asset-Typen {#registering-custom-asset-types}

Aktivieren benutzerdefinierter Asset-Typen für die Auflistung in AEM Forms Portal

>[!NOTE]
>
>Stellen Sie sicher, dass Sie AEM 6.3 mit SP1 und das entsprechende AEM Forms Add On installiert haben. Diese Funktion funktioniert nur mit AEM Forms 6.3 SP1 und höher

## Basispfad angeben {#specify-base-path}

Der Basispfad ist der Repository-Pfad der obersten Ebene, der alle Assets enthält, die ein Benutzer in der Such- und Listener-Komponente auflisten möchte. Falls gewünscht, kann der Benutzer auch bestimmte Speicherorte im Basispfad im Dialogfeld zum Bearbeiten von Komponenten konfigurieren, sodass die Suche an bestimmten Speicherorten ausgelöst wird, anstatt alle Knoten im Basispfad zu durchsuchen. Standardmäßig wird der Basispfad als Suchpfadkriterium zum Abrufen der Assets verwendet, es sei denn, der Benutzer konfiguriert eine Reihe spezifischer Pfade von diesem Speicherort aus. Es ist wichtig, einen optimalen Wert für diesen Pfad zu haben, um eine leistungsstarke Suche durchzuführen. Der Standardwert des Basispfads bleibt als **_/content/dam/formsanddocuments_** weil sich alle AEM Forms-Assets in **_/content/dam/formsanddocuments._**

Schritte zum Konfigurieren des Basispfads

1. Bei crx anmelden
1. Navigieren Sie zu **/libs/fd/fp/extensions/querybuilder/basepath**

1. Klicken Sie in der Symbolleiste auf &quot;Überlagerungsknoten&quot;
1. Stellen Sie sicher, dass der Überlagerungsspeicherort &quot;/apps/&quot;lautet.
1. Klicken Sie auf OK
1. Klicken Sie auf „Speichern“.
1. Navigieren Sie zur neuen Struktur, die unter **/apps/fd/fp/extensions/querybuilder/basepath**

1. Ändern Sie den Wert der Pfadeigenschaft in **&quot;/content/dam&quot;**
1. Klicken Sie auf „Speichern“.

Durch Angabe der Pfadeigenschaft zu **&quot;/content/dam&quot;** Sie legen im Grunde den Basispfad auf /content/dam fest. Dies kann durch Öffnen der Komponente &quot;Search &amp; Lister&quot;überprüft werden.

![Basepath](assets/basepath.png)

## Benutzerdefinierte Asset-Typen registrieren {#register-custom-asset-types}

Wir haben der Such- und Auflistungskomponente eine neue Registerkarte (Asset-Auflistung) hinzugefügt. Auf dieser Registerkarte werden vordefinierte Asset-Typen und zusätzliche von Ihnen konfigurierte Asset-Typen aufgelistet. Standardmäßig werden die folgenden Asset-Typen aufgelistet

1. Adaptive Formulare
1. Formularvorlagen
1. PDF-Formulare
1. Document(Static PDF)

**Schritte zum Registrieren eines benutzerdefinierten Asset-Typs**

1. Erstellen Sie den Überlagerungsknoten von **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Legen Sie den Überlagerungsspeicherort auf &quot;/apps&quot;fest.
1. Navigieren Sie zur neuen Struktur, die unter `/apps/fd/fp/extensions/querybuilder/assettypes`

1. Erstellen Sie unter diesem Speicherort den Knoten &quot;nt:unstructured&quot;für den zu registrierenden Typ und geben Sie ihm einen Namen. **mp4files. Fügen Sie diesem mp4files-Knoten die folgenden beiden Eigenschaften hinzu**

   1. Fügen Sie die Eigenschaft jcr:title hinzu, um den Anzeigenamen des Asset-Typs anzugeben. Setzen Sie den Wert von jcr:title auf &quot;MP4-Dateien&quot;.
   1. Fügen Sie die Eigenschaft &quot;type&quot;hinzu und legen Sie ihren Wert auf &quot;videos&quot;fest. Dies ist der Wert, den wir in unserer Vorlage verwenden, um Assets vom Typ &quot;Videos&quot;aufzulisten. Speichern Sie Ihre Änderungen.

1. Erstellen Sie einen Knoten des Typs &quot;nt:unstructured&quot;unter mp4files. Geben Sie diesem Knoten den Namen &quot;Suchkriterien&quot;.
1. Fügen Sie unter Suchkriterien einen oder mehrere Filter hinzu. Angenommen, der Benutzer möchte einen Suchfilter haben, um mp4Files aufzulisten, deren MIME-Typ &quot;video/mp4&quot;ist, können Sie dies hier tun.
1. Erstellen Sie einen Knoten vom Typ &quot;nt:unstructured&quot;unter den Knoten-Suchkriterien. Nennen Sie diesen Knoten &quot;filetypes&quot;.
1. Fügen Sie diesem Knoten &quot;filetypes&quot;die folgenden 2 Eigenschaften hinzu

   1. name: ./jcr:content/metadata/dc:format
   1. Wert: video/mp4

1. Das bedeutet, dass Assets, deren Eigenschaft dc:format gleich video/mp4 ist, als Asset-Typ &quot;MP4-Videos&quot;betrachtet werden. Sie können jede Eigenschaft, die im Knoten &quot;jcr:content/metadata&quot;aufgeführt ist, für die Suchkriterien verwenden

1. **Speichern Sie Ihre Arbeit**

Nachdem Sie die oben genannten Schritte ausgeführt haben, wird der neue Asset-Typ (MP4-Dateien) in der Dropdown-Liste &quot;Asset-Typen&quot;der Komponente &quot;Search &amp; Lister&quot;angezeigt, wie unten dargestellt.

![mp4files](assets/mp4files.png)

[Wenn Sie Probleme damit haben, dies zu erreichen, können Sie das folgende Package importieren.](assets/assettypeskt1.zip) Für das Paket sind zwei benutzerdefinierte Asset-Typen definiert. MP4-Dateien und Worddocuments. Sehen Sie sich die **/apps/fd/fp/extensions/querybuilder/assettypes**

[Installieren des customeportal-Pakets](assets/customportalpage.zip). Dieses Paket enthält eine Beispielportalseite. Diese Seite wird in Teil 2 dieses Tutorials verwendet
