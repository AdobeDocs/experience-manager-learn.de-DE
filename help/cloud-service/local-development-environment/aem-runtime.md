---
title: Einrichten von Local AEM Runtime für die Entwicklung von AEM als Cloud Service
description: Richten Sie die Local AEM Runtime mithilfe des Schnellstart-JAR des AEM as a Cloud Service SDK ein.
feature: Entwickler-Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d49ae402b332ba972a78cdbd8f5bf962b91c83b1
workflow-type: tm+mt
source-wordcount: '1734'
ht-degree: 3%

---


# Lokale AEM Runtime einrichten

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Lokale AEM Runtime"
>abstract="Adobe Experience Manager (AEM) kann mit dem Schnellstart-Jar des AEM as a Cloud Service SDK lokal ausgeführt werden. Auf diese Weise können Entwickler benutzerdefinierten Code, Konfigurationen und Inhalte bereitstellen und testen, bevor sie ihn an die Quell-Code-Verwaltung übergeben und in einer AEM als Cloud Service-Umgebung bereitstellen."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service-SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM als Cloud Service-SDK herunterladen"

Adobe Experience Manager (AEM) kann mit dem Schnellstart-Jar des AEM as a Cloud Service SDK lokal ausgeführt werden. Auf diese Weise können Entwickler benutzerdefinierten Code, Konfigurationen und Inhalte bereitstellen und testen, bevor sie ihn an die Quell-Code-Verwaltung übergeben und in einer AEM als Cloud Service-Umgebung bereitstellen.

Beachten Sie, dass `~` als Kurzbezeichnung für das Benutzerverzeichnis verwendet wird. Unter Windows entspricht dies `%HOMEPATH%`.

## Java installieren

Experience Manager ist eine Java-Anwendung und erfordert daher das Java-SDK, um die Entwicklungs-Tools zu unterstützen.

1. [Herunterladen und Installieren des neuesten Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Stellen Sie sicher, dass das Java 11 SDK installiert ist, indem Sie den Befehl ausführen:
   + Windows:`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## AEM als Cloud Service-SDK herunterladen

Das AEM as a Cloud Service SDK oder AEM SDK enthält das Schnellstart-Jar, das zum lokalen Ausführen von AEM Author und Publish für die Entwicklung verwendet wird, sowie die kompatible Version der Dispatcher Tools.

1. Melden Sie sich mit Ihrer Adobe ID bei [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) an.
   + Beachten Sie, dass Ihre Adobe-Organisation __muss__ für AEM als Cloud Service bereitgestellt werden, um die AEM als Cloud Service-SDK herunterzuladen.
1. Navigieren Sie zur Registerkarte __AEM als Cloud Service__ .
1. Sortieren nach __Veröffentlichungsdatum__ in __Absteigende__ Reihenfolge
1. Klicken Sie auf die letzte Ergebniszeile __AEM SDK__
1. Überprüfen und akzeptieren Sie den EULA und tippen Sie auf die Schaltfläche __Download__ .

## Extrahieren Sie die Schnellstart-JAR-Datei aus der AEM SDK-ZIP-Datei

1. Dekomprimieren Sie die heruntergeladene Datei `aem-sdk-XXX.zip` .

## Lokalen AEM-Autorendienst einrichten{#set-up-local-aem-author-service}

Der lokale AEM-Autorendienst bietet Entwicklern ein lokales Erlebnis, das digitale Marketing-Experten/Inhaltsautoren für die Erstellung und Verwaltung von Inhalten freigeben.  Der AEM-Autorendienst ist sowohl als Authoring- als auch als Vorschau-Umgebung konzipiert, sodass die meisten Validierungen der Funktionsentwicklung dagegen durchgeführt werden können. Dadurch wird er zu einem wichtigen Element des lokalen Entwicklungsprozesses.

1. Erstellen Sie den Ordner `~/aem-sdk/author`
1. Kopieren Sie die Datei __Quickstart JAR__ in `~/aem-sdk/author` und benennen Sie sie in `aem-author-p4502.jar` um.
1. Starten Sie den lokalen AEM-Autorendienst, indem Sie Folgendes über die Befehlszeile ausführen:
   + `java -jar aem-author-p4502.jar`
      + Geben Sie das Administratorkennwort als `admin` an. Jedes Administratorkennwort ist akzeptabel. Es wird jedoch empfohlen, den Standard für die lokale Entwicklung zu verwenden, um eine Neukonfiguration zu vermeiden.

   Sie *können die AEM nicht* als Cloud Service-Schnellstart-JAR [starten, indem Sie auf](#troubleshooting-double-click) doppelklicken.
1. Greifen Sie in einem Webbrowser auf den lokalen AEM-Autorendienst unter [http://localhost:4502](http://localhost:4502) zu.

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Einrichten des lokalen AEM-Veröffentlichungsdienstes

Der lokale AEM-Veröffentlichungsdienst bietet Entwicklern das lokale Erlebnis, das Endbenutzer der AEM haben werden, z. B. das Durchsuchen der auf AEM gehosteten Website. Ein lokaler AEM-Veröffentlichungsdienst ist wichtig, da er mit den [Dispatcher-Tools](./dispatcher-tools.md) des AEM SDK integriert wird und es Entwicklern ermöglicht, das Endbenutzererlebnis zu rauchen und zu optimieren.

1. Erstellen Sie den Ordner `~/aem-sdk/publish`
1. Kopieren Sie die Datei __Quickstart JAR__ in `~/aem-sdk/publish` und benennen Sie sie in `aem-publish-p4503.jar` um.
1. Starten Sie den lokalen AEM-Veröffentlichungsdienst, indem Sie Folgendes über die Befehlszeile ausführen:
   + `java -jar aem-publish-p4503.jar`
      + Geben Sie das Administratorkennwort als `admin` an. Jedes Administratorkennwort ist akzeptabel. Es wird jedoch empfohlen, den Standard für die lokale Entwicklung zu verwenden, um eine Neukonfiguration zu vermeiden.

   Sie *können die AEM nicht* als Cloud Service-Schnellstart-JAR [starten, indem Sie auf](#troubleshooting-double-click) doppelklicken.
1. Greifen Sie in einem Webbrowser auf den lokalen AEM-Veröffentlichungsdienst unter [http://localhost:4503](http://localhost:4503) zu.

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Simulieren der Inhaltsverteilung {#content-distribution}

In einer echten Cloud Service-Umgebung werden Inhalte vom Autorendienst mithilfe von [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) und der Adobe Pipeline an den Veröffentlichungsdienst verteilt. Die [Adobe-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) ist ein isolierter Microservice, der nur in der Cloud-Umgebung verfügbar ist.

Bei der Entwicklung kann es wünschenswert sein, die Verteilung von Inhalten mithilfe des lokalen Autoren- und Veröffentlichungsdienstes zu simulieren. Dies kann durch Aktivierung der Legacy-Replikationsagenten erreicht werden.

>[!NOTE]
>
> Replikationsagenten sind nur für die Verwendung in der lokalen Schnellstart-JAR verfügbar und bieten nur eine Simulation der Inhaltsverteilung.

1. Melden Sie sich beim Dienst **Author** an und navigieren Sie zu [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Klicken Sie auf **Standardagent (publish)** , um den standardmäßigen Replikationsagenten zu öffnen.
1. Klicken Sie auf **Bearbeiten** , um die Konfiguration des Agenten zu öffnen.
1. Aktualisieren Sie auf der Registerkarte **Einstellungen** die folgenden Felder:

   + **Aktiviert**  - Prüfung &quot;true&quot;
   + **Agenten-Benutzer-ID**  - Lassen Sie dieses Feld leer.

   ![Konfiguration von Replikationsagenten - Einstellungen](assets/aem-runtime/settings-config.png)

1. Aktualisieren Sie auf der Registerkarte **Transport** die folgenden Felder:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Benutzer** - `admin`
   + **Kennwort** - `admin`

   ![Konfiguration von Replikationsagenten - Transport](assets/aem-runtime/transport-config.png)

1. Klicken Sie auf **OK** , um die Konfiguration zu speichern und den **Default** Replikationsagenten zu aktivieren.
1. Sie können jetzt Änderungen an Inhalten im Autorendienst vornehmen und sie im Veröffentlichungsdienst veröffentlichen.

![Seite veröffentlichen](assets/aem-runtime/publish-page-changes.png)

## Schnellstart-JAR-Startmodi

Die Benennung der Schnellstart-JAR-Datei `aem-<tier>_<environment>-p<port number>.jar` gibt an, wie sie gestartet wird. Sobald AEM in einer bestimmten Ebene, einem bestimmten Autor oder einer bestimmten Veröffentlichungsinstanz gestartet wurde, kann sie nicht mehr in die alternative Ebene geändert werden. Dazu muss der Ordner `crx-Quickstart`, der während der ersten Ausführung generiert wurde, gelöscht und die Schnellstart-JAR-Datei muss erneut ausgeführt werden. Umgebung und Ports können geändert werden, sie erfordern jedoch das Anhalten/Starten der lokalen AEM-Instanz.

Das Ändern von Umgebungen, `dev`, `stage` und `prod`, kann für Entwickler nützlich sein, um sicherzustellen, dass umgebungsspezifische Konfigurationen von AEM korrekt definiert und aufgelöst werden. Es wird empfohlen, die lokale Entwicklung in erster Linie gegen den standardmäßigen `dev`-Umgebungs-Ausführungsmodus durchzuführen.

Folgende Permutationen sind verfügbar:

+ `aem-author-p4502.jar`
   + Als Autor im Entwicklungs-Ausführungsmodus auf Port 4502
+ `aem-author_dev-p4502.jar`
   + Als Autor im Entwicklungs-Ausführungsmodus für Port 4502 (identisch mit `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Als Autor im Staging-Ausführungsmodus auf Port 4502
+ `aem-author_prod-p4502.jar`
   + Als Autor im Produktionsmodus am Port 4502
+ `aem-publish-p4503.jar`
   + Als Autor im Entwicklungs-Ausführungsmodus auf Port 4503
+ `aem-publish_dev-p4503.jar`
   + Als Autor im Entwicklungs-Ausführungsmodus für Port 4503 (identisch mit `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + Als Autor im Staging-Ausführungsmodus auf Port 4503
+ `aem-publish_prod-p4503.jar`
   + Als Autor im Produktions-Ausführungsmodus auf Port 4503

Beachten Sie, dass die Portnummer ein beliebiger verfügbarer Port auf dem lokalen Entwicklungscomputer sein kann, jedoch gemäß folgenden Richtlinien:

+ Port __4502__ wird für den __lokalen AEM-Autorendienst__ verwendet
+ Port __4503__ wird für den lokalen AEM-Veröffentlichungsdienst __verwendet__

Eine Änderung dieser Konfigurationen erfordert möglicherweise Anpassungen an AEM SDK-Konfigurationen

## Beenden einer lokalen AEM-Laufzeit

Um eine lokale AEM-Laufzeit zu stoppen, öffnen Sie entweder den AEM-Autoren- oder Veröffentlichungsdienst, das Befehlszeilenfenster, das zum Starten der AEM Runtime verwendet wurde, und tippen Sie auf `Ctrl-C`. Warten Sie, bis AEM heruntergefahren ist. Wenn der Prozess zum Herunterfahren abgeschlossen ist, ist die Eingabeaufforderung für die Befehlszeile verfügbar.

## Optionale Aufgaben zur Einrichtung lokaler AEM zur Laufzeit

+ __Variablen der OSGi-Konfigurationsumgebung und geheime__ Variablen werden  [speziell für die AEM lokale Laufzeit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#local-development) festgelegt, anstatt sie über die Befehlszeilenschnittstelle zu verwalten.

## Wann wird die Schnellstart-JAR aktualisiert?

Aktualisieren Sie das AEM-SDK mindestens monatlich am letzten Donnerstag jedes Monats oder kurz danach. Dies ist die Veröffentlichungsintervall für AEM als Cloud Service &quot;Feature Releases&quot;.

>[!WARNING]
>
> Die Aktualisierung des Schnellstart-JAR auf eine neue Version erfordert das Ersetzen der gesamten lokalen Entwicklungsumgebung, was zu einem Verlust von Code, Konfiguration und Inhalt in den lokalen AEM-Repositorys führt. Stellen Sie sicher, dass Code, Konfigurationen oder Inhalte, die nicht zerstört werden sollen, sicher in Git übertragen oder von der lokalen AEM als AEM Pakete exportiert werden.

### So vermeiden Sie Inhaltsverluste beim Aktualisieren des AEM SDK

Durch die Aktualisierung des AEM SDK wird effektiv eine brandneue AEM Laufzeitumgebung erstellt, einschließlich eines neuen Repositorys, d. h. alle Änderungen, die an einem früheren AEM SDK-Repository vorgenommen wurden, gehen verloren. Im Folgenden finden Sie praktikable Strategien zur Unterstützung der Inhaltserstellung zwischen AEM SDK-Upgrades und können diskret oder gemeinsam verwendet werden:

1. Erstellen Sie ein Inhaltspaket mit Beispielinhalten, um die Entwicklung zu unterstützen, und verwalten Sie es in Git. Alle Inhalte, die durch AEM SDK-Upgrades persistiert werden sollen, werden in diesem Paket beibehalten und nach der Aktualisierung des AEM SDK erneut bereitgestellt.
1. Verwenden Sie [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) mit der `includepaths`-Anweisung, um Inhalte aus dem vorherigen AEM SDK-Repository in das neue AEM SDK-Repository zu kopieren.
1. Sichern Sie alle Inhalte mit AEM Package Manager und Inhaltspaketen auf dem vorherigen AEM SDK und installieren Sie sie erneut auf dem neuen AEM SDK.

Beachten Sie, dass die Verwendung der oben genannten Ansätze zur Codepflege zwischen AEM SDK-Upgrades ein Anti-Muster der Entwicklung anzeigt. Nicht verfügbarer Code sollte aus Ihrer Entwicklungs-IDE stammen und über Implementierungen in AEM SDK fließen.

## Fehlerbehebung

## Doppelklicken auf die Schnellstart-JAR-Datei führt zu einem Fehler{#troubleshooting-double-click}

Wenn Sie auf die Schnellstart-JAR-Datei doppelklicken, wird ein Fehler-Modal angezeigt, das verhindert, dass AEM lokal gestartet werden.

![Fehlerbehebung - Doppelklicken Sie auf die Schnellstart-JAR-Datei](./assets/aem-runtime/troubleshooting__double-click.png)

Dies liegt daran, dass AEM als Cloud Service Quickstart Jar das Doppelklicken der Schnellstart-JAR-Datei nicht unterstützt, um AEM lokal zu starten. Stattdessen müssen Sie die JAR-Datei über diese Befehlszeile ausführen.

Um den AEM-Autorendienst zu starten, `cd` in den Ordner, der die Schnellstart-JAR-Datei enthält, und führen Sie den Befehl aus:

`$ java -jar aem-author-p4502.jar`

oder um den AEM-Veröffentlichungsdienst zu starten, `cd` in den Ordner, der die Schnellstart-JAR-Datei enthält, und führen Sie den Befehl aus:

`$ java -jar aem-publish-p4503.jar`

## Starten des Schnellstart-JAR über die Befehlszeile bricht sofort ab{#troubleshooting-java-8}

Beim Starten der Schnellstart-JAR-Datei über die Befehlszeile wird der Prozess sofort abgebrochen und der AEM-Dienst startet nicht, mit dem folgenden Fehler:

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

Dies liegt daran, dass AEM als Cloud Service Java SDK 11 erfordert und Sie eine andere Version ausführen, höchstwahrscheinlich Java 8. Um dieses Problem zu beheben, laden Sie [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) herunter und installieren Sie es.
Überprüfen Sie nach der Installation von Java SDK 11, ob es sich um die aktive Version handelt, indem Sie Folgendes über die Befehlszeile ausführen.

Überprüfen Sie nach der Installation des Java 11 SDK, ob es sich um die aktive Version handelt, indem Sie den Befehl über die Befehlszeile ausführen:

+ Windows: `java -version`
+ macOS/Linux: `java --version`

## Zusätzliche Ressourcen

+ [AEM SDK herunterladen](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker herunterladen](https://www.docker.com/)
+ [Experience Manager Dispatcher-Dokumentation](https://docs.adobe.com/content/help/de-DE/experience-manager-dispatcher/using/dispatcher.html)
