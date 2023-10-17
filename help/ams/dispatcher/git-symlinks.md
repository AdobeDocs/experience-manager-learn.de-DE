---
title: Ordnungsgemäßes Hinzufügen von Symlinks zu GIT
description: Anleitung dazu, wie und wo Sie Symlinks beim Bearbeiten von Dispatcher-Konfigurationen hinzufügen.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '1252'
ht-degree: 100%

---

# Hinzufügen von Symlinks zu GIT

[Inhaltsverzeichnis](./overview.md)

[&lt;- Zurück: Dispatcher-Konsistenzprüfung](./health-check.md)

In AMS erhalten Sie ein vorausgefülltes GIT-Repository, das den Quell-Code Ihres Dispatchers enthält und bereitsteht, damit Sie mit der Entwicklung und Anpassung beginnen können.

Nachdem Sie die erste `.vhost`-Datei oder Datei `farm.any` der obersten Ebene angelegt haben, müssen Sie eine symbolische Verknüpfung (auch als Symlink bezeichnet) vom Verzeichnis `available_*` zum Verzeichnis `enabled_*` erstellen. Die Verwendung des richtigen Link-Typs ist der Schlüssel für eine erfolgreiche Implementierung über die Cloud Manager-Pipeline. Auf dieser Seite erfahren Sie, wie Ihnen dies gelingt.

## Dispatcher-Archetyp

Der [AEM-Archetyp](https://github.com/adobe/aem-project-archetype) ist in der Regel der Ausgangspunkt für AEM-Entwicklungspersonen, um ihr Projekt zu starten.

Im Folgenden finden Sie ein Beispiel für den Bereich des Quell-Codes, in dem die verwendeten Symlinks angezeigt werden:

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

Beispielsweise enthält das Verzeichnis `/etc/httpd/conf.d/available_vhosts/` die potenziellen `.vhost`-Staging-Dateien, die wir in unserer laufenden Konfiguration verwenden können.

Die aktivierten `.vhost`-Dateien werden als `symlinks` mit relativem Pfad innerhalb des Verzeichnisses `/etc/httpd/conf.d/enabled_vhosts/` angezeigt.

## Erstellen eines Symlinks

Wir verwenden symbolische Verknüpfungen (Symlinks) zur Datei, damit der Apache-Webserver die Zieldatei als dieselbe Datei behandelt. Wir möchten die Datei nicht in beide Verzeichnisse duplizieren. Stattdessen soll nur ein Link von einem Verzeichnis (eine symbolische Verknüpfung) zum anderen vorhanden sein.

Denken Sie daran, dass Ihre bereitgestellten Konfigurationen auf einen Linux-Host ausgerichtet sind. Das Erstellen eines Symlinks, der nicht mit dem Zielsystem kompatibel ist, führt zu Fehlern und unerwünschten Ergebnissen.

Wenn Ihre Workstation kein Linux-Computer ist, werden Sie sich wahrscheinlich fragen, welche Befehle Sie verwenden sollten, um diese Links oder Verknüpfungen ordnungsgemäß zu erstellen, damit eine GIT-Übertragung ermöglicht wird.

> `TIP:` Es ist wichtig, relative Links zu verwenden, damit diese Links weiterhin funktionieren, wenn Sie eine lokale Kopie des Apache-Webservers installiert haben und eine andere Installationsbasis vorhanden gewesen war. Wenn Sie einen absoluten Pfad verwenden, muss Ihre Workstation oder ein anderes System über genau dieselbe Verzeichnisstruktur verfügen.

### OSX/Linux

Symlinks sind nativ für diese Betriebssysteme und hier finden Sie einige Beispiele, wie Sie diese Links erstellen. Öffnen Sie Ihre bevorzugte Terminal-Anwendung und verwenden Sie die folgenden Beispielbefehle, um den Link zu erstellen:

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

Im Folgenden finden Sie ein Beispiel für einen vollständigen Befehl als Referenz:

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

Im Folgenden finden Sie ein Beispiel für den Link, wenn Sie die Datei mit dem Befehl `ls` auflisten lassen:

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` MS Windows (genauer: NTFS) unterstützt übrigens symbolische Links schon seit Windows Vista.

![Abbildung der Windows-Eingabeaufforderung mit der Hilfeausgabe des Befehls „mklink“](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` Um mit dem Befehl „mklink“ Symlinks zu erstellen, sind Administratorrechte erforderlich, damit er ordnungsgemäß ausgeführt werden kann. Selbst mit einem Administratorkonto müssen Sie die Eingabeaufforderung als Admin ausführen, es sei denn, der Entwicklermodus ist aktiviert.
> <br/>Unzureichende Berechtigungen:
> ![Abbildung einer Windows-Eingabeaufforderung, in der der Befehl aufgrund unzureichender Berechtigungen fehlschlägt](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>Geeignete Berechtigungen:
> ![Abbildung einer als Admin ausgeführten Windows-Eingabeaufforderung](./assets/git-symlinks/windows-mklink-properpriv.png)

Im Folgenden finden Sie den Befehl zum Erstellen des Links:

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


Im Folgenden finden Sie ein Beispiel für einen vollständigen Befehl als Referenz:

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### Entwicklermodus (Windows 10)

Im [Entwicklermodus](https://docs.microsoft.com/de-de/windows/apps/get-started/enable-your-device-for-development) können Sie mit Windows 10 problemlos Apps testen, die Sie entwickeln, die Ubuntu Bash-Shell-Umgebung verwenden, eine Vielzahl von Entwicklereinstellungen ändern und vieles mehr.

Microsoft scheint weiterhin Funktionen zum Entwicklermodus hinzuzufügen oder einige dieser Funktionen standardmäßig zu aktivieren, sobald sie eine breitere Anwendung erreichen und als stabil betrachtet werden (z. B. benötigt die Ubuntu Bash-Shell-Umgebung bei Creators Update keinen Entwicklermodus mehr).

Was bedeutet das für Symlinks? Wenn der Entwicklermodus AKTIVIERT ist, brauchen Sie keine Eingabeaufforderung mit erhöhten Rechten auszuführen, um Symlinks erstellen zu können. Sobald der Entwicklermodus aktiviert ist, können daher alle Benutzenden Symlinks erstellen.

> Nach der Aktivierung des Entwicklermodus sollten Benutzende sich ab- und wieder anmelden, damit die Änderungen wirksam werden.

Jetzt können Sie sehen, dass der Befehl funktioniert, auch wenn Sie ihn nicht als Admin ausführen

![Bild der Windows-Eingabeaufforderung, von normalen Benutzenden mit aktiviertem Entwicklermodus ausgeführt](./assets/git-symlinks/windows-mklink-devmode.png)

#### Alternativer/programmgesteuerter Ansatz

Es gibt eine spezielle Richtlinie, die es bestimmten Benutzenden ermöglichen kann, symbolische Links zu erstellen → [Erstellen von symbolischen Links (Windows 10) – Windows-Sicherheit | Microsoft-Dokumente](https://docs.microsoft.com/de-de/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO:
- Dies kann von Kundinnen und Kunden genutzt werden, um die Erstellung symbolischer Links programmgesteuert allen Entwicklungspersonen in ihrer Organisation (d. h. Active Directory) zu ermöglichen, ohne dass der Entwicklermodus manuell auf jedem Gerät aktiviert werden muss.
- Darüber hinaus sollte diese Richtlinie auch in früheren Versionen von MS Windows verfügbar sein, die keinen Entwicklermodus bieten.

KONTRA:
- Diese Richtlinie scheint keine Auswirkungen auf Benutzende zu haben, die der Admin-Gruppe angehören. Admins müssen weiterhin die Eingabeaufforderung mit erhöhten Berechtigungen ausführen. Seltsam.

> Die Änderungen an der lokalen/Gruppenrichtlinie können nur wirksam werden, wenn die bzw. der Benutzende sich ab- und wieder anmeldet.

Führen Sie `gpedit.msc` aus und fügen Sie je nach Bedarf Benutzende hinzu bzw. ändern Sie diese. Admins sind standardmäßig vorhanden

![Fenster „Gruppenrichtlinien-Editor“, das die Berechtigung zum Anpassen anzeigt](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### Aktivieren von Symlinks in GIT

Git verarbeitet Symlinks gemäß der Option „core.symlinks“

Quelle: [Git – Dokumentation zu git-config](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*Wenn „core.symlinks“ auf „false“ gesetzt ist, werden symbolische Links als kleine einfache Dateien ausgecheckt, die den Link-Text enthalten. `git-update-index[1]` und `git-add[1]` ändern den aufgezeichneten Typ nicht in eine reguläre Datei. Nützlich für Dateisysteme wie FAT, die keine symbolischen Links unterstützen.
Der Standardwert ist „true“, mit Ausnahme von `git-clone[1]` oder `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.`. In den meisten Fällen geht Git davon aus, dass Windows für Symlinks nicht geeignet ist, und setzt dies auf „false“.*

Das Verhalten von Git unter Windows wird hier gut erklärt: Symbolische Links ・ Git-for-Windows/Git Wiki ・ GitHub

> `Info`: Die Annahmen in der oben verlinkten Dokumentation scheinen für eine mögliche AEM Developer-Einrichtung unter Windows zu passen, insbesondere mit NTFS und der Tatsache, dass nur Datei-Symlinks und Verzeichnis-Symlinks vorliegen

Hier die gute Nachricht: [Git für Windows, Version 2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) – das Installationsprogramm verfügt über eine [explizite Option zur Aktivierung der Unterstützung von symbolischen Links](https://github.com/git-for-windows/git/issues/921).

> `Warning`: Die Option „core.symlink“ kann während des Klonens des Repositorys zur Laufzeit bereitgestellt oder anderweitig als globale Konfiguration gespeichert werden.

![Anzeigen des GIT-Installationsprogramms mit Unterstützung für Symlinks](./assets/git-symlinks/windows-git-install-symlink.png)

Git für Windows speichert globale Voreinstellungen in `"C:\Program Files\Git\etc\gitconfig"`. Diese Einstellungen werden von anderen Git-Desktop-Client-Anwendungen möglicherweise nicht berücksichtigt.
Dies ist der Haken dabei: nicht alle Entwicklungspersonen verwenden den nativen Git-Client (d. h. Git Cmd, Git Bash), und einige der Git-Desktop-Anwendungen (z. B. GitHub Desktop, Atlassian Sourcetree) können unterschiedliche Einstellungen/Standardeinstellungen für die Verwendung des Systems oder eines eingebetteten Git haben.

Hier ein Beispiel für den Inhalt der Datei `gitconfig`

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Git-Befehlszeilentipps

Es kann Situationen geben, in denen Sie neue symbolische Links erstellen müssen (z. B. das Hinzufügen eines neuen Hosts oder einer neuen Farm).

Wir haben in der obigen Dokumentation gesehen, dass Windows einen Befehl „mklink“ zum Erstellen symbolischer Links anbietet.

Wenn Sie in einer Git Bash-Umgebung arbeiten, können Sie stattdessen den standardmäßigen Bash-Befehl `ln -s` verwenden, aber es muss eine spezielle Anweisung wie die folgende vorangestellt werden:

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### Zusammenfassung

Damit Git unter einem Microsoft Windows-Betriebssystem Symlinks korrekt handhabt (zumindest bezüglich der aktuellen AEM Dispatcher-Konfigurationsgrundlinie), benötigen Sie:

| Element | Mindestversion/-konfiguration | Empfohlene Version/Konfiguration |
|------|---------------------------------|-------------------------------------|
| Betriebssystem | Windows Vista oder höher | Windows 10 Creator-Update oder höher |
| Dateisystem | NTFS | NTFS |
| Funktionalität zum Arbeiten mit Symlinks für Windows-Benutzende | `"Create symbolic links"` Gruppen-/lokale Richtlinie `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10-Entwicklermodus aktiviert |
| Git | Native Client-Version 1.5.3 | Native Client-Version 2.10.2 oder höher |
| Git-Konfiguration | `--core.symlinks=true`-Option beim Git-Klonen über die Befehlszeile | Globale Git-Konfiguration<br/>`[core]`<br/> symlinks = true <br/> Nativer Git-Client-Konfigurationspfad: `C:\Program Files\Git\etc\gitconfig` <br/>Standardspeicherort für Git Desktop-Clients: `%HOMEPATH%\.gitconfig` |

> `Note:` Wenn Sie bereits über ein lokales Repository verfügen, müssen Sie von der Quelle aus neu klonen. Sie können an einem neuen Speicherort klonen und Ihre nicht freigegebenen/unbearbeiteten lokalen Änderungen manuell im neu geklonten Repository zusammenführen.
