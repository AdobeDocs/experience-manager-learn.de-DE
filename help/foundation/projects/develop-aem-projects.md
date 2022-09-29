---
title: Entwickeln von Projekten in AEM
description: Ein Entwicklungs-Tutorial, das die Entwicklung für AEM Projekte veranschaulicht.  In diesem Tutorial erstellen wir eine benutzerdefinierte Projektvorlage, die zum Erstellen neuer Projekte in AEM für die Verwaltung von Workflows und Aufgaben zur Inhaltserstellung verwendet werden kann.
version: 6.4, 6.5
feature: Projects, Workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '4571'
ht-degree: 1%

---

# Entwickeln von Projekten in AEM

Dies ist ein Entwicklungs-Tutorial, in dem gezeigt wird, wie [!DNL AEM Projects].  In diesem Tutorial erstellen wir eine benutzerdefinierte Projektvorlage, die zum Erstellen neuer Projekte in AEM für die Verwaltung von Workflows und Aufgaben zur Inhaltserstellung verwendet werden kann.

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*In diesem Video erhalten Sie eine kurze Demo des fertigen Workflows, der im unten stehenden Tutorial erstellt wird.*

## Einführung    {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) ist eine Funktion von AEM, die die Verwaltung und Gruppierung aller Workflows und Aufgaben im Zusammenhang mit der Inhaltserstellung im Rahmen einer AEM Sites- oder Assets-Implementierung erleichtert.

AEM Projekte werden mit mehreren [OOTB-Projektvorlagen](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates). Beim Erstellen eines neuen Projekts können Autoren aus diesen verfügbaren Vorlagen auswählen. Große AEM Implementierungen mit einzigartigen Geschäftsanforderungen möchten benutzerdefinierte Projektvorlagen erstellen, die auf ihre Bedürfnisse zugeschnitten sind. Durch die Erstellung einer benutzerdefinierten Projektvorlage können Entwickler das Projekt-Dashboard konfigurieren, in benutzerdefinierte Workflows einbinden und zusätzliche Geschäftsrollen für ein Projekt erstellen. Wir werden uns die Struktur einer Projektvorlage ansehen und eine Beispielvorlage erstellen.

![Benutzerdefinierte Projektkarte](./assets/develop-aem-projects/custom-project-card.png)

## Einrichtung

In diesem Tutorial wird der Code erläutert, der zum Erstellen einer benutzerdefinierten Projektvorlage erforderlich ist. Sie können die [angehängtes Paket](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) in eine lokale Umgebung klicken, um sie zusammen mit dem Tutorial zu verfolgen. Sie können auch auf das gesamte Maven-Projekt zugreifen, das auf gehostet wird. [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Abgeschlossenes Tutorial-Paket](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Vollständiges Code-Repository auf GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

In diesem Tutorial werden grundlegende Kenntnisse vorausgesetzt [AEM](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/the-basics.html) und eine gewisse Vertrautheit mit [AEM Maven-Projekteinrichtung](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/ht-projects-maven.html). Der gesamte erwähnte Code sollte als Referenz verwendet und nur in einer [Lokale AEM](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted).

## Struktur einer Projektvorlage

Projektvorlagen sollten unter die Quell-Code-Verwaltung gestellt und unter dem Anwendungsordner unter /apps gespeichert werden. Idealerweise sollten sie in einen Unterordner mit der Namenskonvention von **&#42;/projects/templates/**&lt;my-template>. Durch Befolgung dieser Benennungskonvention stehen Autoren beim Erstellen eines Projekts automatisch alle neuen benutzerdefinierten Vorlagen zur Verfügung. Die Konfiguration der verfügbaren Projektvorlagen ist festgelegt unter: **/content/projects/jcr:content** Knoten durch **cq:allowedTemplates** -Eigenschaft. Standardmäßig ist dies ein regulärer Ausdruck: **/(apps|libs)/.&#42;/projects/templates/.&#42;**

Der Stammknoten einer Projektvorlage weist eine **jcr:primaryType** von **cq:Template**. Unter dem Stammknoten befinden sich drei Knoten: **Gadgets**, **Rollen** und **Workflows**. Diese Knoten sind alle **nt:unstructured**. Unter dem Stammknoten kann auch eine Datei &quot;thumbnail.png&quot;sein, die angezeigt wird, wenn die Vorlage im Assistenten &quot;Projekt erstellen&quot;ausgewählt wird.

Die vollständige Knotenstruktur:

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### Projektvorlagenstamm

Der Stammknoten der Projektvorlage ist vom Typ **cq:Template**. Auf diesem Knoten können Sie Eigenschaften konfigurieren **jcr:title** und **jcr:description** wird im Assistenten zum Erstellen von Projekten angezeigt. Es gibt auch eine Eigenschaft namens **Assistent** , das auf ein Formular verweist, das die Eigenschaften des Projekts ausfüllt. Der Standardwert von: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** sollte in den meisten Fällen problemlos funktionieren, da der Benutzer die grundlegenden Projekteigenschaften ausfüllen und Gruppenmitglieder hinzufügen kann.

*&#42;Beachten Sie, dass der Assistent zum Erstellen von Projekten nicht das Sling-POST-Servlet verwendet. Stattdessen werden Werte an ein benutzerdefiniertes Servlet gesendet:**com.adobe.cq.projects.impl.servlet.ProjectServlet**. Dies sollte beim Hinzufügen benutzerdefinierter Felder berücksichtigt werden.*

Ein Beispiel für einen benutzerdefinierten Assistenten finden Sie für die Vorlage eines Übersetzungsprojekts: **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**.

### Gadgets {#gadgets}

Es gibt keine zusätzlichen Eigenschaften auf diesem Knoten, aber die untergeordneten Elemente des Knotens gadgets steuern, welche Projektkacheln beim Erstellen eines neuen Projekts das Dashboard des Projekts ausfüllen. [Die Projektkacheln](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) (auch als Gadgets oder Pods bezeichnet) sind einfache Karten, die den Arbeitsplatz eines Projekts füllen. Eine vollständige Liste der OOTB-Kacheln finden Sie unter: **/libs/cq/gui/components/projects/admin/pod. **Projekteigentümer können Kacheln immer hinzufügen/entfernen, nachdem ein Projekt erstellt wurde.

### Rollen {#roles}

Es gibt 3 [Standardrollen](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) für jedes Projekt: **Beobachter**, **Bearbeiter** und **Eigentümer**. Durch Hinzufügen von untergeordneten Knoten unter dem Benutzerknoten können Sie zusätzliche geschäftsspezifische Projektrollen für die Vorlage hinzufügen. Anschließend können Sie diese Rollen mit bestimmten Workflows verknüpfen, die mit dem Projekt verknüpft sind.

### Workflows {#workflows}

Einer der verlockendsten Gründe für die Erstellung einer benutzerdefinierten Projektvorlage besteht darin, dass Sie damit die verfügbaren Workflows für die Verwendung mit dem Projekt konfigurieren können. Diese können OOTB-Workflows oder benutzerdefinierte Workflows verwenden. Unter dem **Workflows** -Knoten muss ein **models** Knoten (auch `nt:unstructured`) und untergeordneten Knoten unter geben Sie die verfügbaren Workflow-Modelle an. Die Eigenschaft **modelId **verweist auf das Workflow-Modell unter /etc/workflow und die Eigenschaft **Assistent** verweist auf das Dialogfeld, das beim Starten des Workflows verwendet wird. Ein großer Vorteil von Projekten besteht darin, dass zu Beginn des Workflows ein benutzerdefiniertes Dialogfeld (Assistent) hinzugefügt werden kann, um geschäftsspezifische Metadaten zu erfassen, die weitere Aktionen innerhalb des Workflows ermöglichen.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Erstellen einer Projektvorlage {#creating-project-template}

Da wir hauptsächlich Knoten kopieren/konfigurieren, wird CRXDE Lite verwendet. Öffnen Sie in Ihrer lokalen AEM-Instanz [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Erstellen Sie zunächst einen neuen Ordner unter `/apps/&lt;your-app-folder&gt;` benannt `projects`. Erstellen Sie einen weiteren Ordner unter dem Namen `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Um die Arbeit zu erleichtern, starten wir unsere benutzerdefinierte Vorlage aus der vorhandenen einfachen Projektvorlage.

   1. Kopieren und Einfügen des Knotens **/libs/cq/core/content/projects/templates/default** unterhalb der *templates* Ordner, der in Schritt 1 erstellt wurde.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. Sie sollten jetzt einen Pfad wie **/apps/aem-guides/projects-tasks/projects/templates/authoring-project**.

   1. Bearbeiten Sie die **jcr:title** und **jcr:description** Eigenschaften des Knotens &quot;author-project&quot;in benutzerdefinierte Titel- und Beschreibungswerte.

      1. Lassen Sie die **Assistent** -Eigenschaft, die auf die standardmäßigen Projekteigenschaften verweist.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Für diese Projektvorlage möchten wir Aufgaben verwenden.
   1. Hinzufügen neuer **nt:unstructured** Knoten unter authoring-project/gadgets namens **Aufgaben**.
   1. Hinzufügen von String-Eigenschaften zum Aufgabenknoten für **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Aufgaben&quot; und **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Jetzt [Aufgabenkachel](https://experienceleague.adobe.com/docs/#Tasks) wird standardmäßig angezeigt, wenn ein neues Projekt erstellt wird.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. Wir fügen unserer Projektvorlage eine benutzerdefinierte Rolle &quot;Genehmiger&quot;hinzu.

   1. Fügen Sie unter dem Knoten der Projektvorlage (authoring-project) einen neuen hinzu. **nt:unstructured** Knoten beschriftet **Rollen**.
   1. Weitere hinzufügen **nt:unstructured** -Knoten, der Genehmiger als untergeordnetes Element des Benutzerknotens bezeichnet.
   1. Hinzufügen von String-Eigenschaften **jcr:title** = &quot;**Genehmiger**&quot;, **roleclass** =&quot;**owner**&quot;, **roleid**=&quot;**Genehmiger**&quot;.
      1. Der Name des Genehmigungsknotens sowie jcr:title und roleid können beliebige Zeichenfolgenwerte sein (solange roleid eindeutig ist).
      1. **roleclass** legt die für diese Rolle beantragten Berechtigungen anhand der Variablen [3 OOTB-Rollen](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User Rollen in einem Projekt): **owner**, **editor** und **observer**.
      1. Wenn die benutzerdefinierte Rolle mehr eine Führungsrolle ist, kann die Rollout **Eigentümer;** Wenn es sich um eine spezifischere Authoring-Rolle wie Fotograf oder Designer handelt, wird **editor** roleclass sollte ausreichen. Der große Unterschied zwischen **owner** und **editor** ist, dass Projekteigenschaften von Projekteigenschaften aktualisiert und dem Projekt neue Benutzer hinzugefügt werden können.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Durch Kopieren der Vorlage Einfaches Projekt werden 4 OOTB-Workflows konfiguriert. Jeder Knoten unter Workflows/Modelle verweist auf einen bestimmten Workflow und einen Assistenten für das Starten eines Dialogfelds für diesen Workflow. Später in diesem Tutorial erstellen wir einen benutzerdefinierten Workflow für dieses Projekt. Löschen Sie zunächst die Knoten unter Workflow/Modelle:

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Um es Inhaltsautoren zu erleichtern, die Projektvorlage zu identifizieren, können Sie eine benutzerdefinierte Miniaturansicht hinzufügen. Die empfohlene Größe beträgt 319 x 319 Pixel.
   1. Erstellen Sie in CRXDE Lite eine neue Datei als gleichrangiges Element mit Knoten für Gadgets, Rollen und Workflows mit dem Namen **thumbnail.png**.
   1. Speichern und navigieren Sie dann zum `jcr:content` Knoten und doppelklicken Sie auf `jcr:data` -Eigenschaft (vermeiden Sie das Klicken auf &quot;Ansicht&quot;).
      1. Dies sollte Sie zur Bearbeitung auffordern `jcr:data` -Dateidialogfeld und Sie können eine benutzerdefinierte Miniaturansicht hochladen.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Abgeschlossene XML-Darstellung der Projektvorlage:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## Testen der benutzerdefinierten Projektvorlage

Jetzt können wir unsere Projektvorlage testen, indem wir ein neues Projekt erstellen.

1. Sie sollten die benutzerdefinierte Vorlage als eine der Optionen für die Projekterstellung sehen.

   ![Vorlage auswählen](./assets/develop-aem-projects/choose-template.png)

1. Nachdem Sie die benutzerdefinierte Vorlage ausgewählt haben, klicken Sie auf &quot;Weiter&quot;und beachten Sie, dass Sie sie beim Ausfüllen der Projektmitglieder als Genehmigerrolle hinzufügen können.

   ![Genehmigen](./assets/develop-aem-projects/user-approver.png)

1. Klicken Sie auf &quot;Erstellen&quot;, um das Projekt anhand der benutzerdefinierten Vorlage zu erstellen. Sie werden im Projekt-Dashboard feststellen, dass die Aufgabenkachel und die anderen unter Gadgets konfigurierten Kacheln automatisch angezeigt werden.

   ![Aufgabenkachel](./assets/develop-aem-projects/tasks-tile.png)


## Warum Workflow?

Traditionell AEM Workflows, die sich um einen Validierungsprozess drehen, verwendet die Workflow-Schritte &quot;Teilnehmer&quot;. AEM Posteingang enthält Details zu Aufgaben und Workflows sowie eine verbesserte Integration in AEM Projekte. Diese Funktionen machen die Verwendung der Prozessschritte Aufgabe erstellen zu einer attraktiveren Option.

### Warum Aufgaben?

Die Verwendung eines Schritts zur Aufgabenerstellung anstelle der herkömmlichen Teilnehmer-Schritte bietet einige Vorteile:

* **Start- und Fälligkeitsdatum** - erleichtert es Autoren, ihre Zeit zu verwalten, nutzt die neue Kalenderfunktion diese Daten.
* **Priorität** - integrierte Prioritäten von &quot;Niedrig&quot;, &quot;Normal&quot;und &quot;Hoch&quot;ermöglichen es Autoren, die Arbeit zu priorisieren.
* **Klartext-Kommentare** - Wenn Autoren an einer Aufgabe arbeiten, können sie Kommentare hinterlassen, die die Zusammenarbeit verstärken.
* **Sichtbarkeit** - Aufgabenkacheln und -ansichten mit Projekten ermöglichen es Managern, die Besuchszeit anzuzeigen
* **Projektintegration** - Aufgaben sind bereits in Projektrollen und Dashboards integriert.

Aufgaben können wie Teilnehmer-Schritte dynamisch zugewiesen und weitergeleitet werden. Aufgabenmetadaten wie Titel und Priorität können auch dynamisch basierend auf vorherigen Aktionen festgelegt werden, wie im folgenden Tutorial gezeigt wird.

Aufgaben haben zwar einige Vorteile gegenüber Teilnehmer-Schritten, sind aber außerhalb eines Projekts nicht so nützlich, sondern mit zusätzlichen Mehraufwand verbunden. Zusätzlich muss das dynamische Verhalten von Aufgaben mit Ecma-Skripten mit eigenen Einschränkungen codiert werden.

## Beispielanwendungsfallanforderungen {#goals-tutorial}

![Workflow-Prozessdiagramm](./assets/develop-aem-projects/workflow-process-diagram.png)

Im obigen Diagramm werden die allgemeinen Anforderungen für unseren Beispielvalidierungs-Workflow beschrieben.

Der erste Schritt besteht darin, eine Aufgabe zu erstellen, um die Bearbeitung eines Inhaltselements abzuschließen. Wir werden es dem Workflow-Initiator ermöglichen, den Verantwortlichen für diese erste Aufgabe zu wählen.

Sobald die erste Aufgabe abgeschlossen ist, hat der Bevollmächtigte drei Optionen für die Weiterleitung des Workflows:

**Normal ** - Normales Routing erstellt eine Aufgabe, die der Gruppe Genehmiger des Projekts zugewiesen ist, um sie zu überprüfen und zu genehmigen. Die Priorität der Aufgabe ist Normal und das Fälligkeitsdatum liegt 5 Tage nach ihrer Erstellung.

**Rush** - Das Routing mit Eile erstellt auch eine Aufgabe, die der Gruppe Genehmiger des Projekts zugewiesen ist. Die Priorität der Aufgabe ist hoch und das Fälligkeitsdatum beträgt nur 1 Tag.

**Umgehen** - In diesem Beispiel-Workflow hat der ursprüngliche Teilnehmer die Möglichkeit, die Validierungsgruppe zu umgehen. (Ja, dies könnte den Zweck eines &#39;Genehmigungs&#39;-Workflows beeinträchtigen, ermöglicht uns jedoch, zusätzliche Routing-Funktionen zu veranschaulichen)

Die Gruppe Genehmiger kann den Inhalt genehmigen oder ihn zur erneuten Bearbeitung an den ursprünglichen Bevollmächtigten zurücksenden. Im Falle einer erneuten Bearbeitung wird eine neue Aufgabe erstellt und entsprechend mit der Bezeichnung &quot;Zur erneuten Arbeit zurückgesendet&quot;bezeichnet.

Der letzte Schritt des Workflows nutzt den OTB-Prozessschritt Seite/Asset aktivieren und repliziert die Payload.

## Workflow-Modell erstellen

1. Navigieren Sie im AEM Startmenü zu Tools > Workflow > Modelle . Klicken Sie oben rechts auf &quot;Erstellen&quot;, um ein neues Workflow-Modell zu erstellen.

   Geben Sie dem neuen Modell einen Titel: &quot;Inhaltsvalidierungs-Workflow&quot;und ein URL-Name: &quot;content-approval-workflow&quot;.

   ![Dialogfeld &quot;Workflow erstellen&quot;](./assets/develop-aem-projects/workflow-create-dialog.png)

   Weitere Informationen zu [Workflows erstellen, lesen Sie hier](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. Als Best Practice sollten benutzerdefinierte Workflows in einem eigenen Ordner unter /etc/workflow/models gruppiert werden. Erstellen Sie in CRXDE Lite eine neue **&#39;nt:folder&#39;** unter /etc/workflow/models mit dem Namen **&quot;aem-guides&quot;**. Durch Hinzufügen eines Unterordners wird sichergestellt, dass benutzerdefinierte Workflows bei Upgrades oder Service Pack-Installationen nicht versehentlich überschrieben werden.

   &#42;Beachten Sie, dass es wichtig ist, den Ordner oder die benutzerdefinierten Workflows nie unter den obersten Unterordnern wie /etc/workflow/models/dam oder /etc/workflow/models/projects zu platzieren, da der gesamte Unterordner auch durch Upgrades oder Service Packs überschrieben werden kann.

   ![Speicherort des Workflow-Modells in 6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Speicherort des Workflow-Modells in 6.3

   >[!NOTE]
   >
   >Bei Verwendung von AEM 6.4+ hat sich der Speicherort des Workflows geändert. Weitere Informationen dazu finden Sie [hier.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   Bei Verwendung von AEM 6.4+ wird das Workflow-Modell unter `/conf/global/settings/workflow/models`. Wiederholen Sie die obigen Schritte mit dem Ordner /conf und fügen Sie einen Unterordner mit dem Namen `aem-guides` und verschieben Sie die `content-approval-workflow` darunter.

   ![Speicherort der modernen Workflow-Definition](./assets/develop-aem-projects/modern-workflow-definition-location.png)
Speicherort des Workflow-Modells in 6.4+

1. In AEM 6.3 wurde die Möglichkeit eingeführt, Workflow-Phasen zu einem bestimmten Workflow hinzuzufügen. Die Phasen werden dem Benutzer vom Posteingang auf der Registerkarte Workflow-Informationen angezeigt. Er zeigt dem Benutzer die aktuelle Phase des Workflows sowie die Phasen vor und nach dem Workflow an.

   Um die Schritte zu konfigurieren, öffnen Sie das Dialogfeld Seiteneigenschaften im SideKick. Die vierte Registerkarte trägt die Bezeichnung &quot;Phasen&quot;. Fügen Sie die folgenden Werte hinzu, um die drei Phasen dieses Workflows zu konfigurieren:

   1. Inhalt bearbeiten
   1. Genehmigung
   1. Veröffentlichen 

   ![Konfiguration der Workflow-Phasen](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Konfigurieren Sie die Workflow-Phasen im Dialogfeld Seiteneigenschaften .

   ![Workflow-Statusleiste](./assets/develop-aem-projects/workflow-info-progress.png)

   Die Workflow-Fortschrittsleiste, die im AEM Posteingang angezeigt wird.

   Optional können Sie eine **Bild** in die Seiteneigenschaften, die als Workflow-Miniaturansicht verwendet werden, wenn Benutzer sie auswählen. Die Bildabmessungen sollten 319 x 319 Pixel betragen. Hinzufügen einer **Beschreibung** in den Seiteneigenschaften wird auch angezeigt, wenn ein Benutzer den Workflow auswählt.

1. Der Workflow-Prozess &quot;Projektaufgabe erstellen&quot;dient der Erstellung einer Aufgabe als Schritt im Workflow. Erst nach Abschluss der Aufgabe wird der Workflow fortgesetzt. Ein wichtiger Aspekt des Schritts &quot;Aufgabe erstellen&quot;besteht darin, dass er Workflow-Metadatenwerte lesen und diese zum dynamischen Erstellen der Aufgabe verwenden kann.

   Löschen Sie zunächst den Teilnehmer-Schritt , der standardmäßig erstellt wird. Erweitern Sie über den Sidekick im Menü &quot;Komponenten&quot;den **&quot;Projekte&quot;** Unterüberschrift und Drag-and-Drop **&quot;Create Project Task&quot;** auf das Modell.

   Doppelklicken Sie auf den Schritt &quot;Projektaufgabe erstellen&quot;, um das Workflow-Dialogfeld zu öffnen. Konfigurieren Sie die folgenden Eigenschaften:

   Diese Registerkarte ist für alle Workflow-Prozessschritte üblich. Wir legen den Titel und die Beschreibung fest (diese sind für den Endbenutzer nicht sichtbar). Die wichtige Eigenschaft, die wir festlegen werden, ist die Workflow-Staging-Umgebung auf **&quot;Inhalt bearbeiten&quot;** aus dem Dropdown-Menü.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   Der Workflow-Prozess &quot;Projektaufgabe erstellen&quot;dient der Erstellung einer Aufgabe als Schritt im Workflow. Auf der Registerkarte Aufgabe können wir alle Werte der Aufgabe festlegen. In unserem Fall möchten wir, dass der Verantwortliche dynamisch ist, damit wir ihn leer lassen. Der Rest der Eigenschaftswerte:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   Die Registerkarte Routing ist ein optionales Dialogfeld, in dem die verfügbaren Aktionen für den Benutzer angegeben werden können, der die Aufgabe abschließt. Diese Aktionen sind lediglich Zeichenfolgenwerte und werden in den Metadaten des Workflows gespeichert. Diese Werte können von Skripten und/oder Prozessschritten später im Workflow gelesen werden, um den Workflow dynamisch &quot;weiterzuleiten&quot;. Basierend auf [Workflow-Ziele](#goals-tutorial) Fügen wir diesem Tab drei Aktionen hinzu:

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Auf dieser Registerkarte können wir ein &quot;Task-Skript vor der Erstellung&quot;konfigurieren, in dem wir programmatisch verschiedene Werte der Aufgabe festlegen können, bevor sie erstellt wird. Wir haben die Möglichkeit, das Skript auf eine externe Datei zu verweisen oder ein kurzes Skript direkt in den Dialog einzubetten. In unserem Fall verweisen wir das Skript &quot;Aufgabe vor Erstellung&quot;auf eine externe Datei. In Schritt 5 erstellen wir dieses Skript.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. Im vorherigen Schritt haben wir ein Skript zur Vorab-Erstellung referenziert. Wir erstellen dieses Skript jetzt, in dem wir den Verantwortlichen der Aufgabe auf Grundlage des Werts eines Workflow-Metadatenwerts festlegen &quot;**Bevollmächtigter**&quot;. Die **&quot;Bevollmächtigter&quot;** festgelegt wird, wenn der Workflow gestartet wird. Außerdem werden die Workflow-Metadaten gelesen, um die Priorität der Aufgabe dynamisch auszuwählen, indem Sie die **taskPriority&quot;** -Wert der Metadaten des Workflows sowie das **&quot;taskDueDate&quot;**, das dynamisch festgelegt wird, wenn die erste Aufgabe fällig wird.

   Zu organisatorischen Zwecken haben wir einen Ordner unter unserem Anwendungsordner erstellt, in dem alle unsere projektbezogenen Skripte gespeichert werden: **/apps/aem-guides/projects-tasks/projects/scripts**. Erstellen Sie eine neue Datei unter diesem Ordner mit dem Namen **&quot;start-task-config.ecma&quot;**. &#42;Beachten Sie, dass der Pfad zu Ihrer Datei start-task-config.ecma mit dem Pfad übereinstimmt, der in Schritt 4 auf der Registerkarte &quot;Erweiterte Einstellungen&quot;festgelegt wurde.

   Fügen Sie Folgendes als Inhalt der Datei hinzu:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. Navigieren Sie zurück zum Arbeitsablauf für die Inhaltsvalidierung . Ziehen und ablegen **ODER-Teilung** -Komponente (im Sidekick unter der Kategorie &quot;Workflow&quot;zu finden) unter der **Aufgabe starten** Schritt. Wählen Sie im Dialogfeld Allgemein das Optionsfeld für 3 Verzweigungen aus. Die ODER-Teilung liest den Metadatenwert des Workflows **&quot;lastTaskAction&quot;** um die Route des Workflows zu bestimmen. Die **&quot;lastTaskAction&quot;** -Eigenschaft auf einen der Werte aus der Registerkarte &quot;Routing&quot;festgelegt ist, die in Schritt 4 konfiguriert wurde. Füllen Sie für jede der Registerkarten Verzweigung die **Skript** Textbereich mit den folgenden Werten:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   &#42;Beachten Sie, dass wir eine direkte String-Übereinstimmung ausführen, um die Route zu bestimmen. Daher ist es wichtig, dass die in den Branch-Skripten festgelegten Werte mit den in Schritt 4 festgelegten Route-Werten übereinstimmen.

1. Ziehen und Ablegen eines weiteren &quot;**Projektaufgabe erstellen**&quot; auf das Modell nach links (Verzweigung 1) unter der ODER-Teilung. Füllen Sie das Dialogfeld mit den folgenden Eigenschaften aus:

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Da dies die Route für die normale Genehmigung ist, ist die Priorität der Aufgabe auf &quot;Mittel&quot;eingestellt. Zusätzlich geben wir der Gruppe der Genehmigenden fünf Tage Zeit, um die Aufgabe abzuschließen. Der Verantwortliche wird auf der Registerkarte &quot;Aufgabe&quot;leer gelassen, da wir dies auf der Registerkarte &quot;Erweiterte Einstellungen&quot;dynamisch zuweisen. Wir geben der Gruppe der Genehmigenden zwei mögliche Routen an, wenn diese Aufgabe abgeschlossen wird: **&quot;Genehmigen und veröffentlichen&quot;** ob sie den Inhalt validieren und ihn veröffentlichen können und **&quot;Zur Revision zurücksenden&quot;** wenn Probleme vorliegen, die der ursprüngliche Editor korrigieren muss. Der Genehmiger kann Kommentare hinterlassen, die der ursprüngliche Editor sieht, ob der Workflow an ihn zurückgegeben wird.

Zuvor haben wir in diesem Tutorial eine Projektvorlage erstellt, die eine Rolle &quot;Genehmiger&quot;enthielt. Jedes Mal, wenn ein neues Projekt aus dieser Vorlage erstellt wird, wird eine projektspezifische Gruppe für die Rolle Genehmiger erstellt. Wie ein Teilnehmer-Schritt kann eine Aufgabe nur einem Benutzer oder einer Gruppe zugewiesen werden. Wir möchten diese Aufgabe der Projektgruppe zuweisen, die der Gruppe der Genehmiger entspricht. Alle Workflows, die innerhalb eines Projekts gestartet werden, verfügen über Metadaten, die die Projektrollen der projektspezifischen Gruppe zuordnen.

Kopieren und Fügen Sie den folgenden Code in die **Skript** Textbereich der Registerkarte &quot;Erweiterte Einstellungen&quot;. Dieser Code liest die Workflow-Metadaten und weist die Aufgabe der Gruppe Genehmiger des Projekts zu. Wenn der Wert der Gruppe &quot;Genehmigende Person&quot;nicht gefunden wird, wird die Aufgabe der Gruppe &quot;Administratoren&quot;zugewiesen.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Ziehen und Ablegen eines weiteren &quot;**Projektaufgabe erstellen**&quot; zum Modell in den mittleren Zweig (Zweig 2) unter der ODER-Teilung wechseln. Füllen Sie das Dialogfeld mit den folgenden Eigenschaften aus:

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Da dies die Route &quot;Rush Approval&quot;ist, ist die Priorität der Aufgabe auf &quot;Hoch&quot;eingestellt. Darüber hinaus geben wir der Gruppe der Genehmiger nur einen einzigen Tag, um die Aufgabe abzuschließen. Der Verantwortliche wird auf der Registerkarte &quot;Aufgabe&quot;leer gelassen, da wir dies auf der Registerkarte &quot;Erweiterte Einstellungen&quot;dynamisch zuweisen.

   Wir können dasselbe Skript-Snippet wie in Schritt 7 erneut verwenden, um die **Skript** Textbereich auf der Registerkarte &quot;Erweiterte Einstellungen&quot;. Kopieren Sie den folgenden Code:

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Ziehen Sie eine Komponente vom Typ &quot;Keine Operation&quot;per Drag-and-Drop in den ganz rechten Zweig (Verzweigung 3). Die Komponente &quot;Kein Vorgang&quot;führt keine Aktion durch und wird sofort erweitert, was dem Wunsch des ursprünglichen Editors entspricht, den Genehmigungsschritt zu umgehen. Technisch gesehen könnten wir diese Verzweigung ohne Workflow-Schritte verlassen. Als Best Practice werden wir jedoch einen Schritt &quot;Kein Vorgang&quot;hinzufügen. Dies macht anderen Entwicklern deutlich, was der Zweck von Branch 3 ist.

   Doppelklicken Sie auf den Workflow-Schritt und konfigurieren Sie den Titel und die Beschreibung:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![Workflow-Modell ODER Aufspaltung](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   Das Workflow-Modell sollte wie folgt aussehen, nachdem alle drei Zweige in der ODER-Teilung konfiguriert wurden.

1. Da die Gruppe Genehmiger die Möglichkeit hat, den Workflow für weitere Änderungen an den ursprünglichen Editor zurückzusenden, verlassen wir uns auf die **Goto** -Schritt, um die letzte durchgeführte Aktion zu lesen und den Workflow an den Anfang zu leiten oder fortzufahren.

   Ziehen Sie die Komponente Gehe zu Schritt (im Sidekick unter Workflow) unter die ODER-Teilung, wo sie sich neu verbindet. Doppelklicken Sie auf und konfigurieren Sie die folgenden Eigenschaften im Dialogfeld:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   Das letzte Element, das wir konfigurieren werden, ist das Skript als Teil des Prozessschritts &quot;Gehe zu&quot;. Der Skriptwert kann über das Dialogfeld eingebettet oder so konfiguriert werden, dass er auf eine externe Datei verweist. Das Goto-Skript muss eine **function check()** und &quot;true&quot;zurückgeben, wenn der Workflow zum angegebenen Schritt wechseln soll. Die Rückgabe von Fehlern führt in Zukunft zum Workflow.

   Wenn die Genehmigergruppe die **&quot;Zur Revision zurücksenden&quot;** -Aktion (konfiguriert in Schritt 7 und 8), dann möchten wir den Workflow an die **&quot;Task-Erstellung starten&quot;** Schritt.

   Fügen Sie auf der Registerkarte &quot;Prozess&quot;das folgende Snippet zum Skript-Textbereich hinzu:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. Zum Veröffentlichen der Payload verwenden wir das OOTB **Seite/Asset aktivieren** Prozessschritt Dieser Prozessschritt erfordert wenig Konfiguration und fügt die Payload des Workflows zur Replikationswarteschlange zur Aktivierung hinzu. Wir fügen den Schritt unter dem Schritt Gehe zu hinzu und können auf diese Weise nur erreicht werden, wenn die Gruppe Genehmiger den Inhalt zur Veröffentlichung genehmigt hat oder der ursprüngliche Editor die Route Genehmigung umgehen ausgewählt hat.

   Ziehen und ablegen **Seite/Asset aktivieren** Prozessschritt (im Sidekick unter WCM-Workflow) unter &quot;Zum Schritt wechseln&quot;im Modell.

   ![Workflow-Modell abgeschlossen](assets/develop-aem-projects/workflow-model-final.png)

   Wie das Workflow-Modell aussehen sollte, nachdem der Schritt Gehe zu und der Schritt Seite/Asset aktivieren hinzugefügt wurden.

1. Wenn die Gruppe Genehmiger den Inhalt zur Revision zurücksendet, möchten wir dies dem ursprünglichen Editor mitteilen. Dazu können wir die Eigenschaften zur Aufgabenerstellung dynamisch ändern. Wir werden den Schlüssel für den Wert der lastActionTaken-Eigenschaft von **&quot;Zur Revision zurücksenden&quot;**. Wenn dieser Wert vorhanden ist, ändern wir den Titel und die Beschreibung, um anzugeben, dass diese Aufgabe das Ergebnis ist, dass Inhalte zur Revision zurückgesendet werden. Wir werden auch die Priorität auf **&quot;Hoch&quot;** sodass es das erste Element ist, an dem der Editor arbeitet. Schließlich legen wir das Fälligkeitsdatum der Aufgabe auf einen Tag fest, ab dem der Workflow zur Revision zurückgesendet wurde.

   Start ersetzen `start-task-config.ecma` Skript (erstellt in Schritt 5) mit folgendem Inhalt:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## Erstellen des Assistenten &quot;Workflow starten&quot; {#start-workflow-wizard}

Beim Starten eines Workflows innerhalb eines Projekts müssen Sie einen Assistenten angeben, um den Workflow zu starten. Der Standardassistent: `/libs/cq/core/content/projects/workflowwizards/default_workflow` ermöglicht dem Benutzer die Eingabe eines Workflow-Titels, eines Start-Kommentars und eines Payload-Pfads für die Ausführung des Workflows. Es gibt auch einige weitere Beispiele unter: `/libs/cq/core/content/projects/workflowwizards`.

Die Erstellung eines benutzerdefinierten Assistenten kann sehr leistungsstark sein, da Sie wichtige Informationen vor Beginn des Workflows erfassen können. Die Daten werden gespeichert, da die Metadaten und Workflow-Prozesse des Workflows dies lesen und das Verhalten dynamisch anhand der eingegebenen Werte ändern können. Wir erstellen einen benutzerdefinierten Assistenten, um die erste Aufgabe im Workflow dynamisch basierend auf einem Wert des Starterassistenten zuzuweisen.

1. In CRXDE-Lite erstellen wir einen Unterordner unter `/apps/aem-guides/projects-tasks/projects` Ordner &quot;wizards&quot;. Kopieren Sie den Standardassistenten aus: `/libs/cq/core/content/projects/workflowwizards/default_workflow` Benennen Sie den Ordner unter dem neu erstellten Assistenten-Ordner um und benennen Sie ihn in **content-approval-start**. Der vollständige Pfad sollte jetzt sein: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   Der Standardassistent ist ein zweispaltiger Assistent, der in der ersten Spalte den Titel, die Beschreibung und die Miniaturansicht des ausgewählten Workflow-Modells anzeigt. Die zweite Spalte enthält Felder für den Workflow-Titel, den Startkommentar und den Payload-Pfad. Der Assistent ist ein standardmäßiges Touch-optimiertes Benutzeroberflächenformular und verwendet Standard [Granite-Benutzeroberfläche Formularkomponenten](https://experienceleague.adobe.com/docs/?lang=de) , um die Felder auszufüllen.

   ![Arbeitsablauf für die Inhaltsvalidierung](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Wir fügen dem Assistenten ein zusätzliches Feld hinzu, mit dem der Verantwortliche der ersten Aufgabe im Workflow festgelegt wird (siehe [Erstellen des Workflow-Modells](#create-workflow-model): Schritt 5).

   darunter `../content-approval-start/jcr:content/items/column2/items` Erstellen Sie einen neuen Knoten des Typs `nt:unstructured` benannt **&quot;assign&quot;**. Wir verwenden die Komponente &quot;User Picker für Projekte&quot;(basierend auf der [Granite-Benutzerauswahlkomponente](https://experienceleague.adobe.com/docs/)). Dieses Formularfeld ermöglicht es, die Benutzer- und Gruppenauswahl auf diejenigen zu beschränken, die zum aktuellen Projekt gehören.

   Nachfolgend finden Sie die XML-Darstellung der **zuweisen** node:

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. Außerdem wird ein Feld für die Prioritätsauswahl hinzugefügt, das die Priorität der ersten Aufgabe im Workflow bestimmt (siehe [Erstellen des Workflow-Modells](#create-workflow-model): Schritt 5).

   darunter `/content-approval-start/jcr:content/items/column2/items` Erstellen Sie einen neuen Knoten des Typs `nt:unstructured` benannt **priority**. Wir werden die [Granite-Benutzeroberfläche Auswahlkomponente](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=de) , um das Formularfeld auszufüllen.

   Unter dem **priority** Knoten, den wir hinzufügen werden **items** Knoten von **nt:unstructured**. Unter dem **items** -Knoten 3 weitere Knoten hinzufügen, um die Auswahloptionen für &quot;Hoch&quot;, &quot;Mittel&quot;und &quot;Niedrig&quot;auszufüllen. Jeder Knoten ist vom Typ **nt:unstructured** und sollte **text** und **value** -Eigenschaft. Text und Wert sollten denselben Wert aufweisen:

   1. Hoch
   1. Mittel
   1. Niedrig

   Fügen Sie für den Knoten Medium eine zusätzliche boolesche Eigenschaft mit dem Namen **selected&quot;** mit einem Wert, der auf **true**. Dadurch wird sichergestellt, dass Medium der Standardwert im Auswahlfeld ist.

   Nachfolgend finden Sie eine XML-Darstellung der Knotenstruktur und -eigenschaften:

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. Wir erlauben es dem Workflow-Initiator, das Fälligkeitsdatum der ersten Aufgabe festzulegen. Wir werden die [Granite UI DatePicker](https://experienceleague.adobe.com/docs/) Formularfeld, um diese Eingabe zu erfassen. Wir fügen auch ein ausgeblendetes Feld mit einem [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) , um sicherzustellen, dass die Eingabe als Eigenschaft vom Typ Datum im JCR gespeichert wird.

   Zwei hinzufügen **nt:unstructured** -Knoten mit den folgenden Eigenschaften, die unten in XML dargestellt werden:

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. Sie können den vollständigen Code für das Dialogfeld des Starterassistenten anzeigen [here](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## Workflow und Projektvorlage verbinden {#connecting-workflow-project}

Als Letztes müssen wir sicherstellen, dass das Workflow-Modell innerhalb eines Projekts gestartet werden kann. Dazu müssen wir die Projektvorlage erneut besuchen, die wir in Teil 1 dieser Reihe erstellt haben.

Die Workflow-Konfiguration ist ein Bereich einer Projektvorlage, der die verfügbaren Workflows angibt, die mit diesem Projekt verwendet werden sollen. Die Konfiguration ist auch dafür verantwortlich, den Workflow-Assistenten zum Start anzugeben, wenn der Workflow gestartet wird (den wir im Abschnitt [vorherige Schritte)](#start-workflow-wizard). Die Workflow-Konfiguration einer Projektvorlage ist &quot;live&quot;, d. h. die Aktualisierung der Workflow-Konfiguration wirkt sich auf neu erstellte Projekte sowie auf vorhandene Projekte aus, die die Vorlage verwenden.

1. Navigieren Sie in CRXDE-Lite zur zuvor erstellten Vorlage für das Authoring-Projekt unter `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   Fügen Sie unter dem Knoten &quot;models&quot;einen neuen Knoten mit dem Namen hinzu. **contentapproval** mit dem Knotentyp **nt:unstructured**. Fügen Sie dem Knoten folgende Eigenschaften hinzu:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Bei Verwendung von AEM 6.4 hat sich der Speicherort des Workflows geändert. Punkt `modelId` -Eigenschaft zum Speicherort des Laufzeitarbeitsablaufmodells unter `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >Siehe [hier finden Sie weitere Informationen zur Änderung des Speicherorts des Workflows.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Nachdem der Workflow Inhaltsvalidierung der Projektvorlage hinzugefügt wurde, sollte er in der Kachel Workflow des Projekts verfügbar sein. Starten Sie und spielen Sie mit den verschiedenen Routings, die wir erstellt haben.

## Unterstützende Materialien

* [Herunterladen des abgeschlossenen Tutorial-Pakets](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Vollständiges Code-Repository auf GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM Projektdokumentation](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
