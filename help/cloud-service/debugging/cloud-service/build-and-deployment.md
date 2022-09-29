---
title: Erstellen und Bereitstellen
description: Adobe Cloud Manager erleichtert die Codeerstellung und -bereitstellungen für AEM as a Cloud Service. Fehler können während der Schritte im Build-Prozess auftreten, die Maßnahmen erfordern, um sie zu beheben. In diesem Handbuch werden häufige Fehler bei der Implementierung und der optimale Ansatz erläutert.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2523'
ht-degree: 1%

---

# Debuggen AEM as a Cloud Service Builds und Bereitstellungen

Adobe Cloud Manager erleichtert die Codeerstellung und -bereitstellungen für AEM as a Cloud Service. Fehler können während der Schritte im Build-Prozess auftreten, die Maßnahmen erfordern, um sie zu beheben. In diesem Handbuch werden häufige Fehler bei der Implementierung und der optimale Ansatz erläutert.

![Cloud-Verwaltungs-Build-Pipeline](./assets/build-and-deployment/build-pipeline.png)

## Validierung

Der Validierungsschritt stellt einfach sicher, dass grundlegende Cloud Manager-Konfigurationen gültig sind. Häufige Validierungsfehler umfassen:

### Die Umgebung weist einen ungültigen Status auf

+ __Fehlermeldung:__ Die Umgebung weist einen ungültigen Status auf.
   ![Die Umgebung weist einen ungültigen Status auf](./assets/build-and-deployment/validation__invalid-state.png)
+ __Ursache:__ Die Zielumgebung der Pipeline befindet sich in einem Übergangsstatus, zu dem sie keine neuen Builds akzeptieren kann.
+ __Auflösung:__ Warten Sie, bis der Status in den Status &quot;Wird ausgeführt&quot;aufgelöst (oder verfügbar aktualisiert) wurde. Wenn die Umgebung gelöscht wird, erstellen Sie die Umgebung neu oder wählen Sie eine andere Umgebung aus, für die Sie erstellen möchten.

### Die mit der Pipeline verknüpfte Umgebung kann nicht gefunden werden

+ __Fehlermeldung:__ Die Umgebung wird als gelöscht markiert.
   ![Die Umgebung wird als gelöscht markiert](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Ursache:__ Die Umgebung, für die die Pipeline konfiguriert ist, wurde gelöscht.
Selbst wenn eine neue Umgebung mit demselben Namen neu erstellt wird, ordnet Cloud Manager die Pipeline nicht automatisch dieser Umgebung mit demselben Namen erneut zu.
+ __Auflösung:__ Bearbeiten Sie die Pipeline-Konfiguration und wählen Sie erneut die Umgebung aus, für die bereitgestellt werden soll.

### Die mit der Pipeline verknüpfte Git-Verzweigung kann nicht gefunden werden

+ __Fehlermeldung:__ Ungültige Pipeline: XXXXXX. Grund=Verzweigung=xxxx im Repository nicht gefunden.
   ![Ungültige Pipeline: XXXXXX. Grund=Verzweigung=xxxx nicht im Repository gefunden](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Ursache:__ Die Git-Verzweigung, für die die Pipeline konfiguriert ist, wurde gelöscht.
+ __Auflösung:__ Erstellen Sie die fehlende Git-Verzweigung mit exakt demselben Namen neu oder konfigurieren Sie die Pipeline neu, um sie von einer anderen, vorhandenen Verzweigung zu erstellen.

## Test- und Unit-Tests

![Build- und Unit-Tests](./assets/build-and-deployment/build-and-unit-testing.png)

In der Phase des Build- und Unit-Tests wird ein Maven-Build (`mvn clean package`) des Projekts aus der konfigurierten Git-Verzweigung der Pipeline ausgecheckt.

Fehler, die in dieser Phase identifiziert werden, sollten beim lokalen Erstellen des Projekts reproduzierbar sein, mit folgenden Ausnahmen:

+ Eine Maven-Abhängigkeit ist nicht verfügbar für [Maven Central](https://search.maven.org/) verwendet wird und das Maven-Repository, das die Abhängigkeit enthält, lautet entweder:
   + Unerreichbar über Cloud Manager, z. B. ein privates internes Maven-Repository, oder das Maven-Repository erfordert Authentifizierung und die falschen Anmeldeinformationen wurden bereitgestellt.
   + Nicht explizit im Projekt registriert `pom.xml`. Beachten Sie, dass von der Verwendung von Maven-Repositorys abgeraten wird, da dies die Erstellungszeit verlängert.
+ Unit-Tests schlagen aufgrund von Timing-Problemen fehl. Dies kann vorkommen, wenn bei Komponententests zeitabhängig vorgegangen wird. Ein starker Indikator setzt auf `.sleep(..)` im Testcode.
+ Die Verwendung nicht unterstützter Maven-Plug-ins.

## Codescans

![Codescans](./assets/build-and-deployment/code-scanning.png)

Beim Codescan wird eine statische Codeanalyse durchgeführt, bei der eine Mischung aus Java- und AEM-spezifischen Best Practices verwendet wird.

Das Prüfen von Code führt zu einem Build-Fehler, wenn im Code kritische Sicherheitslücken vorhanden sind. Weniger Verstöße können überschrieben werden, es wird jedoch empfohlen, sie zu beheben. Beachten Sie, dass das Codescan nicht perfekt ist und zu [Falsch-Positiv-Werte](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives).

Laden Sie den CSV-formatierten Bericht herunter, der von Cloud Manager über das **Download-Details** und überprüfen Sie alle Einträge.

Weitere Informationen finden Sie AEM spezifischen Regeln unter Cloud Manager-Dokumentation . [benutzerdefinierte AEM-spezifische Regeln zum Scannen von Code](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Build-Images

![Build-Images](./assets/build-and-deployment/build-images.png)

Das Build-Bild ist für die Kombination der im Schritt Build- und Unit-Tests erstellten integrierten Code-Artefakte mit der AEM-Version verantwortlich, um ein einzelnes bereitstellbares Artefakt zu bilden.

Während beim Build- und Unit-Test Probleme beim Erstellen und Kompilieren von Code auftreten, können beim Versuch, das benutzerdefinierte Build-Artefakt mit der AEM-Version zu kombinieren, Konfigurations- oder Strukturprobleme auftreten.

### OSGi-Konfigurationen duplizieren

Wenn mehrere OSGi-Konfigurationen über den Ausführungsmodus für die Ziel-AEM-Umgebung aufgelöst werden, schlägt der Schritt Bild erstellen mit dem Fehler fehl:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### Ursache 1

+ __Ursache:__ Das Paket &quot;all&quot;des AEM-Projekts enthält mehrere Code-Pakete. Dieselbe OSGi-Konfiguration wird von mehreren Code-Paketen bereitgestellt, was zu einem Konflikt führt, was dazu führt, dass der Schritt Bild erstellen nicht entscheiden kann, welches Paket verwendet werden soll, sodass der Build fehlschlägt. Beachten Sie, dass dies nicht für OSGi-Werkskonfigurationen gilt, sofern sie eindeutige Namen haben.
+ __Auflösung:__ Überprüfen Sie alle Code-Pakete (einschließlich aller enthaltenen Code-Pakete von Drittanbietern), die als Teil der AEM-Anwendung bereitgestellt werden, und suchen Sie nach doppelten OSGi-Konfigurationen, die über den Ausführungsmodus in die Zielumgebung aufgelöst werden. Die Anleitung der Fehlermeldung &quot;Setzen Sie das Flag mergeConfigurations auf true&quot;ist in AEM as a Cloud Service nicht möglich und sollte ignoriert werden.

#### Ursache 2

+ __Ursache:__ Das AEM-Projekt enthält fälschlicherweise dasselbe Code-Paket zweimal, was zur Duplizierung einer beliebigen OSGi-Konfiguration führt, die in dem Paket enthalten ist.
+ __Auflösung:__ Überprüfen Sie alle pom.xml -Pakete, die in das gesamte Projekt eingebettet sind, und stellen Sie sicher, dass sie über die `filevault-package-maven-plugin` [Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) auf `<cloudManagerTarget>none</cloudManagerTarget>`.

### Fehlerhaftes repoinit-Skript

Repoinit-Skripte definieren Grundlinien-Inhalte, Benutzer, ACLs usw. In AEM as a Cloud Service werden Repoinit-Skripte während des Build-Image angewendet. Auf AEM lokalen Schnellstart des SDK werden sie jedoch angewendet, wenn die OSGi-Repoinit-Werkskonfiguration aktiviert wird. Aus diesem Grund schlagen Repoinit-Skripte im lokalen Schnellstart des SDK möglicherweise leise fehl (mit Protokollierung), führen aber dazu, dass der Schritt Bild erstellen fehlschlägt und die Bereitstellung angehalten wird.

+ __Ursache:__ Ein Repoinit-Skript ist fehlerhaft. Dadurch kann Ihr Repository unvollständig bleiben, da alle Repoinit-Skripte nach Ausführung des fehlerhaften Skripts nicht für das Repository ausgeführt werden.
+ __Auflösung:__ Überprüfen Sie den lokalen Schnellstart des AEM SDK, wenn die OSGi-Konfiguration des Repoinit-Skripts bereitgestellt wird, um festzustellen, ob und was die Fehler sind.

### Unzufriedene repoinit-Inhaltsabhängigkeit

Repoinit-Skripte definieren Grundlinien-Inhalte, Benutzer, ACLs usw. In AEM lokalen Schnellstart des SDK werden Repoinit-Skripte angewendet, wenn die repoinit-OSGi-Werkskonfiguration aktiviert ist, d. h. nachdem das Repository aktiv ist, und können Inhaltsänderungen direkt oder über Inhaltspakete vorgenommen haben. In AEM as a Cloud Service werden während des Erstellens eines Bilds Repoinit-Skripte auf ein Repository angewendet, von dem das repoinit-Skript möglicherweise keine Inhalte enthält.

+ __Ursache:__ Ein Repoinit-Skript hängt von nicht vorhandenen Inhalten ab.
+ __Auflösung:__ Stellen Sie sicher, dass der Inhalt, von dem das Repoinit-Skript abhängt, vorhanden ist. Häufig deutet dies auf eine ungenügend definierte Repoinit-Skripte hin, die fehlende Anweisungen sind, die diese fehlenden, aber erforderlichen Inhaltsstrukturen definieren. Dies kann lokal reproduziert werden, indem AEM gelöscht, das JAR entpackt und die repoinit-OSGi-Konfiguration mit dem repoinit-Skript zum Installationsordner hinzugefügt und AEM gestartet werden. Der Fehler wird selbst im &quot;error.log&quot;des lokalen Schnellstarts des AEM SDK angezeigt.


### Die Version der Kernkomponenten der Anwendung ist größer als die bereitgestellte Version

_Dieses Problem betrifft nur Nicht-Produktionsumgebungen, die NICHT automatisch auf die neueste AEM aktualisieren._

AEM as a Cloud Service enthält automatisch die neueste Version der Kernkomponenten in jeder AEM, d. h. nachdem eine AEM as a Cloud Service Umgebung automatisch oder manuell aktualisiert wurde, wurde die neueste Version der Kernkomponenten bereitgestellt.

Ist möglich, schlägt der Schritt Bild erstellen fehl, wenn:

+ Die bereitstellende Anwendung aktualisiert die Maven-Abhängigkeitsversion der Kernkomponenten im `core` (OSGi-Bundle)-Projekt
+ Die bereitstellende Anwendung wird dann in einer Sandbox (Nicht-Produktion) AEM as a Cloud Service Umgebung bereitgestellt, die nicht für die Verwendung einer AEM Version mit dieser neuen Kernkomponentenversion aktualisiert wurde.

Um diesen Fehler zu vermeiden, fügen Sie bei jeder verfügbaren Aktualisierung der as a Cloud Service AEM-Umgebung die Aktualisierung als Teil des nächsten Builds/der nächsten Bereitstellung ein und stellen Sie sicher, dass die Aktualisierungen nach dem Inkrementieren der Kernkomponentenversion in der Codebasis der Anwendung eingeschlossen sind.

+ __Symptome:__
Der Schritt Bild erstellen schlägt mit einem FEHLER-Reporting fehl, der Folgendes angibt: 
`com.adobe.cq.wcm.core.components...` Pakete in bestimmten Versionsbereichen konnten nicht von der `core` Projekt.

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __Ursache:__  Das OSGi-Bundle der Anwendung (definiert im `core` -Projekt) importiert Java-Klassen aus der Kernabhängigkeit der Kernkomponenten auf einer anderen Versionsebene als der, die AEM as a Cloud Service bereitgestellt wird.
+ __Auflösung:__
   + Mit Git können Sie zu einem funktionierenden Commit zurückkehren, der vor der Erhöhung der Kernkomponenten-Version existiert. Übertragen Sie diesen Commit in eine Cloud Manager-Git-Verzweigung und führen Sie eine Aktualisierung der Umgebung von dieser Verzweigung durch. Dadurch wird AEM auf die neueste AEM Version aktualisiert, die die spätere Version der Kernkomponenten enthält. Sobald das AEM-as a Cloud Service auf die neueste AEM Version aktualisiert wurde, die die neueste Kernkomponentenversion enthält, stellen Sie den ursprünglich fehlerhafter Code erneut bereit.
   + Um dieses Problem lokal zu reproduzieren, stellen Sie sicher, dass die AEM SDK-Version dieselbe AEM Versionsversion ist, die die AEM as a Cloud Service Umgebung verwendet.


### Support-Fall für Adobe erstellen

Wenn die oben genannten Ansätze zur Fehlerbehebung das Problem nicht beheben, erstellen Sie bitte einen Support-Fall für Adoben über:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Registerkarte &quot;Support&quot;> &quot;Anwendungsfall erstellen&quot;

   _Wenn Sie mehreren Adobe-Org. angehören, stellen Sie sicher, dass die Adobe-Org, die eine fehlgeschlagene Pipeline hat, im Umschalter &quot;Adobe Orgs&quot;ausgewählt ist, bevor Sie die Groß-/Kleinschreibung erstellen._

## Bereitstellen in

Der Schritt Bereitstellen in ist dafür verantwortlich, das im Build-Bild generierte Code-Artefakt zu übernehmen, neue AEM-Autoren- und Veröffentlichungsdienste mit diesem zu starten und bei Erfolg alle alten AEM-Autoren- und Veröffentlichungsdienste zu entfernen. Veränderliche Inhaltspakete und Indizes werden ebenfalls in diesem Schritt installiert und aktualisiert.

Machen Sie sich mit [as a Cloud Service Protokolle AEM](./logs.md) vor dem Debugging des Schritts Bereitstellung für . Die `aemerror` log enthält Informationen zum Starten und Herunterfahren von Pods, die für Probleme beim Bereitstellen von Pods relevant sein können. Beachten Sie, dass das Protokoll, das über die Schaltfläche Protokoll herunterladen im Schritt Bereitstellen von Cloud Manager verfügbar ist, nicht der `aemerror` protokollieren und keine detaillierten Informationen zu den von Ihnen initiierten Anwendungen enthalten.

![Bereitstellen in](./assets/build-and-deployment/deploy-to.png)

Die drei Hauptgründe, warum der Schritt Bereitstellung auf fehlschlagen kann:

### Die Cloud Manager-Pipeline enthält eine alte AEM.

+ __Ursache:__ Eine Cloud Manager-Pipeline enthält eine ältere Version von AEM, als in der Zielumgebung bereitgestellt wird. Dies kann vorkommen, wenn eine Pipeline wiederverwendet wird und auf eine neue Umgebung zeigt, in der eine spätere Version von AEM ausgeführt wird. Dies lässt sich erkennen, indem geprüft wird, ob die AEM Version der Umgebung größer ist als die AEM der Pipeline.
   ![Die Cloud Manager-Pipeline enthält eine alte AEM.](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Auflösung:__
   + Wenn in der Zielumgebung die Option Update verfügbar ist, wählen Sie die Option Aktualisieren aus den Aktionen der Umgebung und führen Sie den Build erneut aus.
   + Wenn in der Zielumgebung keine Aktualisierung verfügbar ist, wird die neueste Version der AEM ausgeführt. Um dies zu beheben, löschen Sie die Pipeline und erstellen Sie sie erneut.


### Zeitüberschreitung bei Cloud Manager

Der Code, der während des Starts des neu bereitgestellten AEM-Dienstes ausgeführt wird, dauert so lange, bis Cloud Manager eine Zeitüberschreitung aufweist, bevor die Bereitstellung abgeschlossen werden kann. In diesen Fällen kann die Implementierung erfolgreich sein, selbst wenn der Cloud Manager-Status als fehlgeschlagen gemeldet wurde.

+ __Ursache:__ Benutzerdefinierter Code kann Vorgänge wie große Abfragen oder Inhaltstreuvorgänge ausführen, die frühzeitig im OSGi-Bundle oder in Lebenszyklen von Komponenten ausgelöst werden und die Startzeit von AEM erheblich verzögern.
+ __Auflösung:__ Überprüfen Sie die Implementierung auf Code, der frühzeitig im Lebenszyklus des OSGi-Pakets ausgeführt wird, und überprüfen Sie die `aemerror` Protokolle für die AEM-Autoren- und Veröffentlichungsdienste um den Zeitpunkt des Fehlers (Protokollzeit in GMT), wie vom Cloud Manager angezeigt, und suchen Sie nach Protokollmeldungen, die auf benutzerdefinierte Prozesse zur Protokollausführung hinweisen.

### Inkompatibler Code oder inkompatible Konfiguration

Die meisten Code- und Konfigurationsverletzungen werden zu einem früheren Zeitpunkt im Build erfasst. Es ist jedoch möglich, dass benutzerdefinierter Code oder eine benutzerdefinierte Konfiguration mit dem AEM-as a Cloud Service inkompatibel sind und nicht erkannt werden, bis er im Container ausgeführt wird.

+ __Ursache:__ Benutzerspezifischer Code kann längere Vorgänge aufrufen, z. B. große Abfragen oder Inhaltstreuvorgänge, die frühzeitig im OSGi-Bundle oder in Lebenszyklen von Komponenten ausgelöst werden, was die Startzeit von AEM erheblich verzögert.
+ __Auflösung:__ Überprüfen Sie die `aemerror` Protokolle für die AEM-Autoren- und Veröffentlichungsdienste um die Zeit (Protokollzeit in GMT) des Fehlers, wie von Cloud Manager angezeigt.
   1. Überprüfen Sie die Protokolle auf alle FEHLER, die von den von der benutzerdefinierten Anwendung bereitgestellten Java-Klassen ausgegeben werden. Wenn Probleme gefunden werden, beheben Sie den festen Code und erstellen Sie die Pipeline neu.
   1. Überprüfen Sie die Protokolle auf FEHLER, die von Aspekten von AEM gemeldet werden, mit denen Sie in Ihrer benutzerdefinierten Anwendung erweitern/interagieren, und untersuchen Sie diese. Diese FEHLER werden möglicherweise nicht direkt Java-Klassen zugeordnet. Wenn Probleme gefunden werden, beheben Sie den festen Code und erstellen Sie die Pipeline neu.

### Einfügen von /var in das Inhaltspaket

`/var` ist veränderlich und enthält eine Vielzahl von transienten Laufzeitinhalten. Einschließlich `/var` in Inhaltspaketen (z. B. `ui.content`), die über Cloud Manager bereitgestellt werden, kann dazu führen, dass der Schritt Bereitstellung fehlschlägt.

Dieses Problem lässt sich nur schwer erkennen, da es nicht zu einem Fehler bei der ersten Bereitstellung führt, sondern nur bei nachfolgenden Implementierungen. Wichtige Symptome sind:

+ Die anfängliche Bereitstellung ist zwar erfolgreich, neue oder geänderte veränderliche Inhalte, die Teil der Bereitstellung sind, scheinen jedoch nicht im AEM-Veröffentlichungsdienst vorhanden zu sein.
+ Aktivierung/Deaktivierung von Inhalten in AEM Author ist blockiert
+ Nachfolgende Implementierungen schlagen im Schritt Bereitstellen fehl, wobei der Schritt Bereitstellen nach etwa 60 Minuten fehlschlägt.

Die Validierung dieses Problems ist die Ursache für das fehlerhafte Verhalten:

1. Wenn Sie feststellen, dass mindestens ein Inhaltspaket, das Teil der Bereitstellung ist, an schreibt `/var`.
1. Stellen Sie sicher, dass die primäre (fett gedruckte) Verteilungswarteschlange blockiert ist unter:
   + AEM-Autor > Tools > Bereitstellung > Verteilung
      ![Blockierte Verteilungswarteschlange](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. Laden Sie bei fehlgeschlagener nachfolgender Bereitstellung die &quot;Bereitstellen in&quot;-Protokolle von Cloud Manager mithilfe der Schaltfläche Protokoll herunterladen herunter:

   ![Bereitstellung in Protokolle herunterladen](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... und überprüfen Sie, dass zwischen den Protokollanweisungen etwa 60 Minuten liegen:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... und ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Beachten Sie, dass dieses Protokoll diese Indikatoren nicht für die ersten Implementierungen enthält, die als erfolgreich gemeldet werden, sondern nur für nachfolgende fehlgeschlagene Bereitstellungen.

+ __Ursache:__ AEM Replikationsdienstbenutzer, der zum Bereitstellen von Inhaltspaketen für den AEM Publish-Dienst verwendet wird, kann nicht schreiben in `/var` auf AEM Publish. Dies führt dazu, dass die Bereitstellung des Inhaltspakets für den AEM-Veröffentlichungsdienst fehlschlägt.
+ __Auflösung:__ Die folgenden Methoden zur Lösung dieser Probleme werden in der Reihenfolge ihrer Präferenz aufgelistet:
   1. Wenn die Variable `/var` -Ressourcen sind nicht erforderlich, um Ressourcen zu entfernen, die unter `/var` aus Inhaltspaketen, die als Teil Ihrer Anwendung bereitgestellt werden.
   2. Wenn die Variable `/var` -Ressourcen erforderlich sind, definieren Sie die Knotenstrukturen mithilfe von [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). Repoinit-Skripte können über OSGi-Ausführungsmodi auf AEM Author, AEM Publish oder beides ausgerichtet werden.
   3. Wenn die Variable `/var` -Ressourcen sind nur für AEM Autor erforderlich und können nicht vernünftigerweise mit [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit), verschieben Sie sie in ein eigenständiges Inhaltspaket, das nur in der AEM-Autoreninstanz von [Einbetten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=de#embeddeds) im `all` -Paket in einem AEM Author-Runmode-Ordner (`<target>/apps/example-packages/content/install.author</target>`).
   4. Stellen Sie geeignete ACLs für die `sling-distribution-importer` Dienstbenutzer wie in diesem [Adobe KB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Support-Fall für Adobe erstellen

Wenn die oben genannten Ansätze zur Fehlerbehebung das Problem nicht beheben, erstellen Sie bitte einen Support-Fall für Adoben über:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Registerkarte &quot;Support&quot;> &quot;Anwendungsfall erstellen&quot;

   _Wenn Sie mehreren Adobe-Org. angehören, stellen Sie sicher, dass die Adobe-Org, die eine fehlgeschlagene Pipeline hat, im Umschalter &quot;Adobe Orgs&quot;ausgewählt ist, bevor Sie die Groß-/Kleinschreibung erstellen._
