---
title: Fehlerbehebung bei der CI/CD-Pipeline mit dem AEM Development Agent
description: Erfahren Sie, wie Sie eine fehlerhafte CI/CD-Pipeline mit dem AEM Development Agent beheben und beheben können.
version: Experience Manager as a Cloud Service
role: Developer, Admin
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20279
thumbnail: KT-20279.png
last-substantial-update: null
source-git-commit: 6fa0f88c231f7b68392a77a60491d4f741140a5a
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 3%

---


# Fehlerbehebung bei der CI/CD-Pipeline mit dem AEM Development Agent

Erfahren Sie, wie Sie eine fehlgeschlagene CI/CD-Pipeline mit dem AEM Development Agent beheben können.

Der AEM-Entwicklungsagent unterstützt technische Teams, einschließlich Entwickler, DevOps-Ingenieure und Administratoren, bei der **Beschleunigung ihrer Workflows** indem er _KI-gestützte Anleitungen und Aktionen“_.

>[!TIP]
>
> Siehe auch [Überblick über Agenten in AEM](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview), um eine vollständige Liste der in AEM as a Cloud Service verfügbaren Agenten, ihrer Funktionen und ihrer Zugriffsmöglichkeiten zu erhalten.


## Übersicht

Der AEM Development Agent bietet verschiedene Funktionen, einschließlich der Möglichkeit, fehlgeschlagene CI/CD-Pipelines aufzulisten, zu beheben und zu beheben. Sie können den AEM Development Agent über den KI-Assistenten für Ihre spezifischen Anwendungsfälle aufrufen.

In diesem Tutorial wird das [WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) verwendet, um zu demonstrieren, wie Sie eine fehlgeschlagene CI/CD-Pipeline mithilfe des AEM Development Agents beheben und beheben können. Die gleichen Prinzipien gelten für jedes AEM-Projekt.

Der Einfachheit halber wird in diesem Tutorial ein Modultest-Fehler in der `BylineImpl.java`-Datei vorgestellt, um die Funktionen zum Beheben von Pipeline-Fehlern des AEM-Entwicklungsagenten zu präsentieren.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- KI-Assistent und Agenten in AEM aktiviert. Siehe [Einrichten von KI in AEM](./setup.md) für Details, und beachten Sie, dass die in diesem Artikel erwähnten Playgrounds keine Funktionen eines AEM-Entwicklungsagenten haben.
- Zugriff auf Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) mit der Rolle Entwickler oder Programm-Manager. Weitere Informationen finden [&#x200B; unter &#x200B;](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-manager/content/requirements/users-and-roles#role-definitions)Rollendefinitionen“.
- Eine AEM as a Cloud Service-Umgebung
- Zugriff auf Agenten in AEM über das [Beta-Programm](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)
- Das [WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) wurde auf Ihren lokalen Computer geklont

### Aktuelle Funktionen des AEM Development Agents

Bevor wir in das Tutorial eintauchen, werfen wir einen Blick auf die aktuellen Funktionen des AEM Development Agent:

- Auflisten der CI/CD-Pipelines und ihres Status
- Fehlerbehebung bei fehlgeschlagenen **Full-Stack**-Pipelines, einschließlich _Code-Qualität_ und _Bereitstellung_.
- Die _Build_-Schritte (Kompilierung des Codes, um ein bereitstellbares Artefakt zu erzeugen) und _Code-Qualität_ (statische Code-Analyse über SonarQube-Regeln) der **Full-Stack**-Pipelines werden unterstützt.

Die Funktionen des AEM Development Agents werden kontinuierlich erweitert und regelmäßig aktualisiert. Feedback und Vorschläge erhalten Sie per E-Mail [aem-devagent@adobe.com](mailto:aem-devagent@adobe.com).

## Einrichtung

Führen Sie die folgenden allgemeinen Schritte aus, um dieses Tutorial abzuschließen:

1. Klonen Sie das [WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) und übertragen Sie es in Ihr Cloud Manager-Git-Repository.
2. Erstellen und Konfigurieren einer Code-Qualitäts-Pipeline
3. Ausführen der Pipeline und Beobachten der fehlgeschlagenen Ausführung
4. Verwenden Sie den AEM Development Agent, um die fehlgeschlagene Pipeline zu beheben

Gehen wir die einzelnen Schritte im Detail durch.

### Verwenden des WKND Sites-Projekts als Demoprojekt

In diesem Tutorial wird die `tutorial/dev-agent/unit-test-failure`-Verzweigung des WKND Sites-Projekts verwendet, um die Verwendung des AEM-Entwicklungsagenten zu demonstrieren. Die gleichen Prinzipien können auf jedes AEM-Projekt angewendet werden.

- Ein Fehler bei einem Modultest wurde wie folgt in die `BylineImpl.java`-Datei aufgenommen. Wenn Sie Ihr eigenes AEM-Projekt verwenden, können Sie einen ähnlichen Fehler bei Modultests einführen.

  ```java
  ...
  @Override
  public String getName() {
      if (name != null) {
          return "Author: " + name; // This line is intentionally incorrect to introduce a unit test failure.
      }
      return name;
  }
  ...
  ```

- Klonen Sie [WKND Sites-](https://github.com/adobe/aem-guides-wknd)) auf Ihrem lokalen Computer, navigieren Sie zum Projektverzeichnis und wechseln Sie zur `tutorial/dev-agent/unit-test-failure`.

  ```shell
  git clone https://github.com/adobe/aem-guides-wknd.git
  cd aem-guides-wknd
  git checkout tutorial/dev-agent/unit-test-failure
  ```

- Erstellen Sie ein neues Cloud Manager-Git-Repository für das WKND Sites-Projekt und fügen Sie es als Remote-Datenquelle zum lokalen Git-Repository hinzu:

   - Navigieren Sie zu Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) und wählen Sie Ihr Programm aus.
   - Klicken **in** linken Seitenleiste auf „Repositorys“.
   - Klicken Sie **oben** auf „Repository hinzufügen“.
   - Geben Sie einen **Repository-Namen** ein (z. B. „wknd-site-tutorial„) und klicken Sie auf **Speichern**. Warten Sie, bis das Repository erstellt wurde.

     ![Repository hinzufügen](./assets/dev-agent/add-repository.png)

   - Klicken Sie **oben rechts auf** Repository-Informationen abrufen“ und kopieren Sie die Repository-URL.

     ![Auf Repository-Informationen zugreifen](./assets/dev-agent/access-repo-info.png)

   - Fügen Sie das neu erstellte Cloud Manager-Git-Repository als Remote-Datenquelle zu Ihrem lokalen Git-Repository hinzu:

     ```shell
     git remote add adobe https://git.cloudmanager.adobe.com/<your-adobe-organization>/wknd-site-tutorial/
     ```

- Übertragen Sie Ihr lokales Git-Repository in das Cloud Manager-Git-Repository:

  ```shell
  git push adobe
  ```

  Wenn Sie nach Anmeldeinformationen gefragt werden, geben Sie den **Benutzernamen** und das **Kennwort** aus dem modalen Cloud Manager-Fenster **Repository-Informationen** ein.

### Erstellen und Konfigurieren einer Code-Qualitäts-Pipeline

In diesem Tutorial wird zur Fehlerbehebung eine Code-Qualitäts-Pipeline (produktionsfremd) verwendet, um den Pipeline-Trigger auszuführen. Weitere [&#x200B; zu Code-Qualitäts-Pipelines finden &#x200B;](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#introduction) unter „Einführung in CI/CD-Pipelines“.

- Navigieren Sie in Cloud Manager zum Abschnitt **Pipelines** und wählen Sie **Hinzufügen** > **Produktionsfremde Pipeline hinzufügen** aus.
- Konfigurieren **im Dialogfeld „Produktionsfremde Pipeline**&quot; Folgendes:

   - **Konfiguration** Schritt:
      - Behalten Sie die Standardwerte wie **Pipeline-Typ** als `Code Quality Pipeline` und **Bereitstellungs-Trigger** als `Manual` bei.
      - Geben **für „Name der produktionsfremden Pipeline** &quot;`Code Quality::Fullstack`&quot; ein

     ![Hinzufügen einer produktionsfremden Pipeline-Konfiguration](./assets/dev-agent/add-non-production-pipeline-configuration.png)

   - **Source-Code** Schritt:
      - Wählen Sie **Full-Stack-Code**
      - Wählen **für &quot;**&quot; das neu erstellte Cloud Manager-Git-Repository aus
      - Wählen **für „Git** Verzweigung“ `tutorial/dev-agent/unit-test-failure`
      - Klicken Sie auf **Speichern**.

     ![Hinzufügen des Source-Codes für produktionsfremde Pipelines](./assets/dev-agent/add-non-production-pipeline-source-code.png)

- Führen Sie die neu erstellte Code-Qualitäts **Pipeline aus, indem Sie** Menü mit den drei Punkten des Pipeline-Eintrags auf „Ausführen“ klicken.

  ![Ausführen der Code-Qualitäts-Pipeline](./assets/dev-agent/run-code-quality-pipeline.png)


>[!IMPORTANT]
>
> Die Bereitstellungs-Pipeline wird in diesem Tutorial nicht behandelt. Sie können jedoch dieselben Prinzipien anwenden, um eine fehlgeschlagene Bereitstellungs-Pipeline zu beheben.


### Beobachten der fehlgeschlagenen Pipeline-Ausführung

Die Code-Qualitäts-Pipeline schlägt im Schritt **Artefaktvorbereitung** mit einem Fehler fehl:

![Fehlgeschlagene Pipeline-Ausführung](./assets/dev-agent/failed-pipeline-execution.png)

Ohne den AEM Development Agent erfordert dieser Pipeline-Fehler eine manuelle Fehlerbehebung. Ein Entwickler müsste die Protokolle überprüfen und den Code überprüfen - ein mühsamer und zeitaufwendiger Prozess.

Als Nächstes sehen Sie, wie Agent AI Fehler beheben und die fehlgeschlagene Pipeline-Ausführung beheben kann.

## Verwenden Sie den AEM Development Agent, um die fehlgeschlagene Pipeline zu beheben und Fehler zu beheben

Sie können den AEM-Entwicklungsagenten mithilfe des KI-Assistenten in AEM aufrufen, indem Sie den Pipeline-Fehler in natürlicher Sprache beschreiben.

- Klicken Sie auf **KI** Assistent) oben rechts.

- Geben Sie die Details zum Pipeline-Fehler in natürlicher Sprache (**)**. Zum Beispiel:

  ```text
  I have a failed pipeline execution on %PROGRAM-NAME% program, help me to troubleshoot and fix it.
  ```

  ![AEM-Entwicklungsagenten aufrufen](./assets/dev-agent/invoke-aem-development-agent.png)

  Der **AEM Development Agent** wird aufgerufen, um Fehler bei der Pipeline-Ausführung zu beheben.

  >[!NOTE]
  >
  > Wenn die eingegebene Eingabeaufforderung nicht klar ist, bittet der KI-Assistent um Klarstellung und stellt Informationen bereit, die Ihnen dabei helfen, die Eingabeaufforderung zu verfeinern.

- Sobald die Argumentation abgeschlossen ist, klicken Sie auf das Symbol **Im Vollbildmodus öffnen**, um den detaillierten Fehlerbehebungsprozess anzuzeigen.

  ![Im Vollbildmodus öffnen](./assets/dev-agent/open-in-full-screen.png)

  Die Ergebnisse enthalten wertvolle Einblicke, einschließlich Fehlerdetails, der Quelldatei, der Zeilennummer und eines **Wie man** behebt“ mit klaren Schritten zur Lösung des Problems.

- In diesem Fall schlug der Agent korrekt vor, entweder die Implementierung zu ändern (`getName()`) oder den Modultest zu aktualisieren (`getNameTest()`), um das Problem zu beheben. Es vermied Halluzinationen und verwendete einen Human-in-the-Loop-Ansatz, während es verwertbare Code-Änderungen für den Entwickler bereitstellte.

  ![Code-Änderungen kopieren](./assets/dev-agent/copy-code-changes.png)

- Aktualisieren Sie die `BylineImpl.java` mit den vorgeschlagenen Code-Änderungen, übertragen Sie dann die Änderungen und übertragen Sie sie in das Cloud Manager-Git-Repository.

  ```java
  ...
  @Override
  public String getName() {
      return name;
  }
  ...
  ```

- Führen Sie die Pipeline erneut aus und beobachten Sie die erfolgreiche Ausführung.

## Weitere Beispiele

Das WKND Sites-Projekt enthält zusätzliche Beispiele für fehlerhaften Code und Konfigurationsprobleme, wie fehlende Abhängigkeiten und falsche Konfiguration. Sie können diese Beispiele erkunden, indem Sie sich die [Verzweigungen“ ansehen, die mit `tutorial/dev-agent/`](https://github.com/adobe/aem-guides-wknd/branches/all?query=tutorial%2Fdev-agent&lastTab=overview) beginnen. Um die grundlegenden Änderungen anzuzeigen, können Sie den `tutorial/dev-agent/unit-test-failure` mit dem `main` Verzweigung vergleichen, indem Sie auf die Schaltfläche **Vergleichen** klicken. Suchen Sie dann nach dem Abschnitt _Geänderte Datei_.

![Verzweigungen vergleichen](./assets/dev-agent/compare-branches.png)

Siehe auch [Beispielaufforderungen](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview#sample-prompts), um weitere Ideen zur Verwendung des AEM-Entwicklungsagenten zu erhalten.

## Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie mit dem AEM Development Agent eine fehlgeschlagene CI/CD-Pipeline mithilfe des KI-Assistenten beheben und beheben können. Außerdem haben Sie gelernt, wie die Agentic AI die technischen Workflows beschleunigt, indem sie umsetzbare Einblicke und Code-Änderungen bereitstellt.

Beginnen Sie mit der Verwendung des AEM-Entwicklungsagenten und anderer Agenten in AEM AEM, um Ihre Workflows zu beschleunigen. Weitere Informationen finden Sie [Übersicht über Agenten in &#x200B;](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)).

## Zusätzliche Ressourcen

- [KI in Experience Manager](./overview.md)
- [Überblick über Agenten in AEM](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
- [Übersicht über den Entwicklungsagenten](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview)
- [Überblick über Agenten in AEM](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
