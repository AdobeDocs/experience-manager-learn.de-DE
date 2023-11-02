---
title: Einrichten des lokalen AEM SDK für die AEM as a Cloud Service-Entwicklung
description: Richten Sie das lokale AEM SDK mithilfe der Schnellstart-JAR des AEM as a Cloud Service-SDK ein.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: 2a412126ac7a67a756d4101d56c1715f0da86453
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 100%

---

# Einrichten des lokalen AEM SDK {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Lokale AEM-Laufzeit"
>abstract="Adobe Experience Manager (AEM) kann mit dem QuickStart Jar des AEM as a Cloud Service SDK lokal ausgeführt werden. Auf diese Weise können Entwickelnde benutzerdefinierten Code, Konfigurationen und Inhalte bereitstellen und testen, bevor sie sie der Quell-Code-Kontrolle übergeben und in einer AEM as a Cloud Service-Umgebung bereitstellen."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=de" text="AEM as a Cloud Service-SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/de/aemcloud.html" text="Herunterladen des AEM as a Cloud Service SDK"

Adobe Experience Manager (AEM) kann mit dem QuickStart Jar des AEM as a Cloud Service SDK lokal ausgeführt werden. Auf diese Weise können Entwickelnde benutzerdefinierten Code, Konfigurationen und Inhalte bereitstellen und testen, bevor sie sie der Quell-Code-Kontrolle übergeben und in einer AEM as a Cloud Service-Umgebung bereitstellen.

Beachten Sie, dass `~` als Abkürzung für das Benutzerverzeichnis verwendet wird. Unter Windows entspricht dies `%HOMEPATH%`.

## Installieren von Java

Experience Manager ist eine Java-Anwendung und erfordert daher das Oracle Java SDK, um die Entwicklungs-Tools zu unterstützen.

1. [Laden Sie das neueste Java SDK 11 herunter und installieren Sie es](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Prüfen Sie, ob das Java 11 SDK installiert ist, indem Sie den folgenden Befehl ausführen:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## Herunterladen des AEM as a Cloud Service-SDK

Das AEM as a Cloud Service SDK bzw. AEM SDK enthält die Schnellstart-JAR, die zum lokalen Ausführen von AEM Author und Publish für die Entwicklung verwendet wird, sowie die kompatible Version der Dispatcher-Tools.

1. Melden Sie sich mit Ihrer Adobe ID auf [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) an.
   + Beachten Sie, dass Ihre Adobe-Organisation für AEM as a Cloud Service bereitgestellt werden __muss__, um das AEM as a Cloud Service-SDK herunterladen zu können.
1. Navigieren Sie zur Registerkarte __AEM as a Cloud Service__
1. Sortieren Sie nach __Veröffentlichungsdatum__ in __absteigender__ Reihenfolge
1. Klicken Sie auf die neueste __AEM SDK__-Ergebniszeile
1. Überprüfen und akzeptieren Sie die EULA und tippen Sie auf die Schaltfläche __Herunterladen__

## Extrahieren Sie die Schnellstart-JAR-Datei aus der AEM SDK-ZIP

1. Entpacken Sie die heruntergeladene Datei `aem-sdk-XXX.zip`

## Einrichten eines lokalen AEM-Author-Service{#set-up-local-aem-author-service}

Der lokale AEM-Author-Service bietet Entwicklerinnen und Entwicklern ein lokales Erlebnis, das digitale Marketing-Fachkräfte/Inhaltsautorinnen bzw. Inhaltsautoren für die Erstellung und Verwaltung von Inhalten freigeben. Der AEM-Author-Service ist sowohl als Verfassungs- als auch als Vorschau-Umgebung konzipiert, sodass die meisten Validierungen der Funktionsentwicklung damit durchgeführt werden können. Dadurch wird er zu einem wichtigen Element des lokalen Entwicklungsprozesses.

1. Erstellen Sie den Ordner `~/aem-sdk/author`.
1. Kopieren Sie die __Schnellstart-JAR__-Datei nach `~/aem-sdk/author` und benennen Sie sie in `aem-author-p4502.jar` um
1. Starten Sie den lokalen AEM-Author-Service, indem Sie Folgendes über die Befehlszeile ausführen:
   + `java -jar aem-author-p4502.jar`
      + Geben Sie das Admin-Passwort als `admin` an. Jedes Admin-Passwort ist akzeptabel, es wird jedoch empfohlen, für die lokale Entwicklung das Standardpasswort zu verwenden, um die Notwendigkeit einer Neukonfiguration zu verringern.

   Sie können die Schnellstart-JAR für AEM as a Cloud Service *nicht* [durch einen Doppelklick](#troubleshooting-double-click) starten.
1. Greifen Sie in einem Webbrowser auf den lokalen AEM-Author-Service unter [http://localhost:4502](http://localhost:4502) zu

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## Einrichten des lokalen AEM-Publish-Service

Der lokale AEM-Publish-Service bietet Entwicklerinnen und Entwicklern das lokale Erlebnis, das Endbenutzende von AEM erfahren werden, z. B. das Durchsuchen der auf AEM gehosteten Website. Ein lokaler AEM-Publish-Service ist wichtig, da er in die [Dispatcher-Tools](./dispatcher-tools.md) des AEM SDK integriert ist und es den Entwicklerinnen und Entwicklern ermöglicht, die endgültige Endbenutzererfahrung zu testen und zu optimieren.

1. Erstellen Sie den Ordner `~/aem-sdk/publish`.
1. Kopieren Sie die __Schnellstart-JAR__ Datei nach `~/aem-sdk/publish` und benennen Sie sie in `aem-publish-p4503.jar` um.
1. Starten Sie den lokalen AEM-Publish-Service, indem Sie Folgendes über die Befehlszeile ausführen:
   + `java -jar aem-publish-p4503.jar`
      + Geben Sie das Admin-Passwort als `admin` an. Jedes Admin-Passwort ist akzeptabel, es wird jedoch empfohlen, für die lokale Entwicklung das Standardpasswort zu verwenden, um die Notwendigkeit einer Neukonfiguration zu verringern.

   Sie können die Schnellstart-JAR für AEM as a Cloud Service *nicht* [durch einen Doppelklick](#troubleshooting-double-click) starten.
1. Greifen Sie in einem Webbrowser auf den lokalen AEM-Veröffentlichungs-Service unter [http://localhost:4503](http://localhost:4503) zu

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## Einrichten lokaler AEM-Dienste im Vorabversionsmodus

Die lokale AEM-Laufzeit kann im [Vorabversionsmodus](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=de) gestartet werden, sodass eine Entwicklerin oder ein Entwickler die Funktionen der nächsten Version von AEM as a Cloud Service nutzen kann. Die Vorabversion wird aktiviert, indem das Argument `-r prerelease` beim ersten Start der lokalen AEM-Laufzeit übergeben wird. Dies kann sowohl mit lokalen AEM-Author- als auch AEM-Publish-Services verwendet werden.


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## Simulieren der Inhaltsverteilung {#content-distribution}

In einer echten Cloud-Service-Umgebung werden die Inhalte mithilfe von [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) und der Adobe-Pipeline vom Author-Service zum Publish-Service verteilt. Die [Adobe-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=de#content-distribution) ist ein isolierter Microservice, der nur in der Cloud-Umgebung verfügbar ist.

Bei der Entwicklung kann es wünschenswert sein, die Verteilung von Inhalten mithilfe des lokalen Author- und Publish-Service zu simulieren. Dies kann durch Aktivierung der Legacy-Replikationsagenten erreicht werden.

>[!NOTE]
>
> Replikationsagenten sind nur für die Verwendung in der lokalen Schnellstart-JAR verfügbar und bieten nur eine Simulation der Inhaltsverteilung.

1. Melden Sie sich beim **Authoring**-Service an und navigieren Sie zu [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Klicken Sie auf **Standardagent (Publish)**, um den Standard-Replikationsagenten zu öffnen.
1. Klicken Sie auf **Bearbeiten**, um die Konfiguration des Agenten zu öffnen.
1. Aktualisieren Sie auf der Registerkarte **Einstellungen** die folgenden Felder:

   + **Aktiviert**: Setzen Sie ein Häkchen
   + **Agenten-Benutzer-ID**: Lassen Sie dieses Feld leer

   ![Konfiguration des Replikationsagenten – Einstellungen](assets/aem-runtime/settings-config.png)

1. Aktualisieren Sie auf der Registerkarte **Transport** die folgenden Felder:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Benutzer** - `admin`
   + **Passwort** – `admin`

   ![Konfiguration des Replikationsagenten – Transport](assets/aem-runtime/transport-config.png)

1. Klicken Sie auf **Ok**, um die Konfiguration zu speichern und den **standardmäßigen** Replikationsagenten zu aktivieren.
1. Sie können jetzt Änderungen an Inhalten im Author-Service vornehmen und sie im Publish-Service veröffentlichen.

![Seite veröffentlichen](assets/aem-runtime/publish-page-changes.png)

## Schnellstart-JAR-Startmodi

Die Benennung der Schnellstart-JAR-Datei, `aem-<tier>_<environment>-p<port number>.jar` gibt an, wie es gestartet wird. Sobald AEM in einer bestimmten Ebene (Author oder Publish) gestartet wurde, kann es nicht mehr in die alternative Ebene geändert werden. Dazu muss der beim ersten Lauf erzeugte Ordner `crx-Quickstart` gelöscht und die Schnellstart-JAR erneut ausgeführt werden. Umgebung und Ports können geändert werden, dies erfordert jedoch das Anhalten/Starten der lokalen AEM-Instanz.

Das Ändern von Umgebungen, `dev`, `stage` und `prod`, kann für Entwicklerinnen und Entwickler nützlich sein, um sicherzustellen, dass umgebungsspezifische Konfigurationen korrekt definiert und von AEM aufgelöst werden. Es wird empfohlen, dass die lokale Entwicklung in erster Linie mit dem Standardausführungsmodus der Umgebung `dev` erfolgt.

Folgende Permutationen sind verfügbar:

| Schnellstart-JAR-Dateiname | Modusbeschreibung |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Als Author im Entwicklungs-Ausführungsmodus auf Port 4502 |
| `aem-author_dev-p4502.jar` | Als Author im Entwicklungs-Ausführungsmodus auf Port 4502 (identisch mit `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Als Author im Staging-Ausführungsmodus auf Port 4502 |
| `aem-author_prod-p4502.jar` | Als Author im Produktionsmodus auf Port 4502 |
| `aem-publish-p4503.jar` | Als Publish im Entwicklungs-Ausführungsmodus auf Port 4503 |
| `aem-publish_dev-p4503.jar` | Als Publish im Entwicklungs-Ausführungsmodus auf Port 4503 (identisch mit `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Als Publish im Staging-Ausführungsmodus auf Port 4503 |
| `aem-publish_prod-p4503.jar` | Als Publish im Produktionsmodus auf Port 4503 |

Beachten Sie, dass die Port-Nummer ein beliebiger verfügbarer Port auf dem lokalen Entwicklungs-Computer sein kann, jedoch gemäß folgenden Richtlinien:

+ Port __4502__ wird für den __lokalen AEM-Author-Service__ verwendet
+ Port __4503__ wird für den __lokalen AEM-Publish-Service__ verwendet

Eine Änderung dieser Einstellungen kann Anpassungen der AEM SDK-Konfigurationen erfordern

## Beenden einer lokalen AEM-Laufzeit

Um eine lokale AEM-Laufzeit zu beenden, entweder den AEM-Author- oder -Publish-Service, öffnen Sie das Befehlszeilenfenster, das zum Starten der AEM-Laufzeit verwendet wurde, und drücken Sie `Ctrl-C`. Warten Sie, bis AEM heruntergefahren ist. Wenn der Prozess des Herunterfahrens abgeschlossen ist, ist die Eingabeaufforderung für die Befehlszeile verfügbar.

## Optionale Aufgaben zur Einrichtung der lokalen AEM-Laufzeit

+ __OSGi-Konfigurationsumgebungsvariablen und geheime Variablen__ werden [speziell für die lokale AEM-Laufzeit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#local-development) festgelegt, anstatt sie über die AIO-CLI zu verwalten.

## Wann die Schnellstart-JAR aktualisiert werden sollte

Aktualisieren Sie das AEM SDK mindestens einmal im Monat am oder kurz nach dem letzten Donnerstag des Monats, was der Veröffentlichungshäufigkeit für „Funktionsveröffentlichungen“ für AEM as a Cloud Service entspricht.

>[!WARNING]
>
> Die Aktualisierung der Schnellstart-JAR auf eine neue Version erfordert das Ersetzen der gesamten lokalen Entwicklungsumgebung, was zu einem Verlust des gesamten Codes, der Konfiguration und des Inhalts in den lokalen AEM-Repositorys führt. Stellen Sie deswegen sicher, dass Code, Konfigurationen oder Inhalte, die nicht zerstört werden sollen, sicher in Git übertragen oder aus der lokalen AEM-Instanz als AEM-Pakete exportiert werden.

### Vermeiden von Inhaltsverlusten beim Aktualisieren des AEM SDK

Durch die Aktualisierung des AEM SDK wird effektiv eine brandneue AEM-Laufzeit erstellt, einschließlich eines neuen Repositorys, d. h. alle Änderungen, die an einem früheren AEM SDK-Repository vorgenommen wurden, gehen verloren. Im Folgenden finden Sie praktikable Strategien zur Unterstützung der Inhaltserstellung zwischen AEM SDK-Upgrades und können diskret oder gemeinsam verwendet werden:

1. Erstellen Sie ein Inhaltspaket mit Beispielinhalten, um die Entwicklung zu unterstützen, und verwalten Sie es in Git. Alle Inhalte, die durch AEM SDK-Upgrades hindurch persistiert werden sollen, werden in diesem Paket persistiert und nach der Aktualisierung des AEM SDK erneut bereitgestellt.
1. Verwenden Sie [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) mit der Direktive `includepaths`, um Inhalte aus dem vorherigen AEM SDK-Repository in das neue AEM SDK-Repository zu kopieren.
1. Sichern Sie alle Inhalte und Inhaltspakete mit AEM Package Manager auf dem vorherigen AEM SDK und installieren Sie sie erneut auf dem neuen AEM SDK.

Denken Sie daran, dass die Verwendung der oben genannten Ansätze zur Wartung von Code zwischen AEM SDK-Upgrades ein negatives Verhaltensmuster bei der Entwicklung darstellt. Nicht verfügbarer Code sollte aus Ihrer Entwicklungs-IDE stammen und über Implementierungen in das AEM SDK fließen.

## Fehlerbehebung

### Beim Doppelklicken auf die Schnellstart-JAR-Datei tritt ein Fehler auf{#troubleshooting-double-click}

Wenn Sie auf die Schnellstart-JAR-Datei doppelklicken, wird ein Fehler-Modal angezeigt, das verhindert, dass AEM lokal gestartet wird.

![Fehlerbehebung – Doppelklicken der Schnellstart-JAR-Datei](./assets/aem-runtime/troubleshooting__double-click.png)

Dies liegt daran, dass AEM as a Cloud Service das Doppelklicken der Schnellstart-JAR-Datei nicht unterstützt, um AEM lokal zu starten. Stattdessen müssen Sie die JAR-Datei über diese Befehlszeile ausführen.

Um den AEM-Author-Service zu starten, wechseln Sie mit `cd` in das Verzeichnis, das die Schnellstart-JAR enthält, und führen Sie den folgenden Befehl aus:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

Oder, um den AEM-Publish-Service zu starten, wechseln Sie mit `cd` in das Verzeichnis, das die Schnellstart-JAR enthält, und führen Sie den folgenden Befehl aus:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4503.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-author-p4503.jar
```

>[!ENDTABS]

### Ein Starten der Schnellstart-JAR-Datei über die Befehlszeile wird sofort abgebrochen{#troubleshooting-java-8}

Beim Starten der Schnellstart-JAR-Datei über die Befehlszeile wird der Prozess sofort abgebrochen, der AEM Service startet nicht und folgender Fehler erscheint:

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

Dies liegt daran, dass AEM as a Cloud Service Java SDK 11 erfordert und Sie eine andere Version verwenden, höchstwahrscheinlich Java 8. Um dieses Problem zu beheben, laden Sie [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) herunter und installieren Sie es.

Sobald das Java 11 SDK installiert wurde, vergewissern Sie sich, dass es sich um die aktive Version handelt, indem Sie folgenden Befehl in der Befehlszeile ausführen:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

## Zusätzliche Ressourcen

+ [AEM SDK herunterladen](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker herunterladen](https://www.docker.com/)
+ [Experience Manager-Dispatcher-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=de)
