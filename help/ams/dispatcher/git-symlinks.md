---
title: Ordnungsgemäßes Hinzufügen von Symlinks zu GIT
description: Anleitung zum Hinzufügen von Symlinks beim Arbeiten mit Dispatcher-Konfigurationen.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 1%

---


# Hinzufügen von Symlinks zu GIT

[Inhalt](./overview.md)

[&lt;- Zurück: Dispatcher-Konsistenzprüfung](./health-check.md)

In AMS erhalten Sie ein vorausgefülltes GIT-Repository, das den Quellcode Ihres Dispatchers enthält und bereit ist, mit der Entwicklung und Anpassung zu beginnen.

Nachdem Sie die erste `.vhost` Datei oder oberste Ebene `farm.any` -Datei erstellen, müssen Sie eine symbolische Verknüpfung aus der `available_*` Verzeichnis in `enabled_*` Verzeichnis. Die Verwendung des richtigen Linktyps ist der Schlüssel für eine erfolgreiche Implementierung über die Cloud Manager-Pipeline. Auf dieser Seite erfahren Sie, wie Sie das machen.

## Dispatcher-Archetyp

Der AEM-Entwickler startet sein Projekt in der Regel von der [AEM Archetyp](https://github.com/adobe/aem-project-archetype)

Im Folgenden finden Sie ein Beispiel für den Bereich des Quellcodes, in dem die verwendeten Symlinks angezeigt werden:

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

Beispiel: `/etc/httpd/conf.d/available_vhosts/` enthält das Staging-Potenzial `.vhost` -Dateien, die wir in unserer laufenden Konfiguration verwenden können.

Die aktivierten `.vhost` -Dateien werden als relativer Pfad angezeigt `symlinks` innerhalb der `/etc/httpd/conf.d/enabled_vhosts/` Verzeichnis.

## Symlink erstellen

Wir verwenden symbolische Links zur Datei, damit der Apache Webserver die Zieldatei als dieselbe Datei behandelt.  Wir möchten die Datei nicht in beide Verzeichnisse duplizieren.  Stattdessen nur eine Verknüpfung von einem Verzeichnis (symbolische Verknüpfung) zum anderen.

Erkennen Sie, dass Ihre bereitgestellten Konfigurationen auf einen Linux-Host ausgerichtet sind.  Das Erstellen eines Symlinks, der nicht mit dem Zielsystem kompatibel ist, führt zu Fehlern und unerwünschten Ergebnissen.

Wenn Ihre Workstation kein Linux-Computer ist, werden Sie sich wahrscheinlich fragen, welche Befehle Sie verwenden sollten, um diese Links ordnungsgemäß zu erstellen, damit sie sie in GIT übertragen können.

> `TIP:` Es ist wichtig, relative Links zu verwenden, da die Links weiterhin funktionieren, wenn Sie eine lokale Kopie des Apache Webservers installiert haben und eine andere Installationsbasis haben.  Wenn Sie einen absoluten Pfad verwenden, muss Ihre Workstation oder andere Systeme mit derselben exakten Verzeichnisstruktur übereinstimmen.

### OSX/Linux

Symlinks sind nativ für diese Betriebssysteme und hier finden Sie einige Beispiele, wie Sie diese Links erstellen.  Öffnen Sie Ihre bevorzugte Terminal-Anwendung und verwenden Sie die folgenden Beispielbefehle, um den Link zu erstellen:

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

Hier finden Sie ein Beispiel für einen ausgefüllten Befehl zur Referenz:

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

Im Folgenden finden Sie ein Beispiel für den Link, wenn Sie die Datei mit der `ls` command:

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` MS Windows (besser, NTFS) unterstützt seit dem... Windows Vista!

![Abbildung der Windows-Eingabeaufforderung mit der Hilfedokumentation des mklink-Befehls](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` Für den Befehl mklink zum Erstellen von Symlinks sind Administratorrechte erforderlich, damit er ordnungsgemäß ausgeführt werden kann. Selbst als Administratorkonto müssen Sie die Eingabeaufforderung &quot;Als Administrator&quot;ausführen, es sei denn, der Entwicklermodus ist aktiviert.
> <br/>Unordnungsgemäße Berechtigungen:
> ![Abbildung der Windows-Eingabeaufforderung mit dem Befehl schlägt aufgrund von Berechtigungen fehl](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>Ordnungsgemäße Berechtigungen:
> ![Bild der Windows-Eingabeaufforderung als Administrator ausgeführt](./assets/git-symlinks/windows-mklink-properpriv.png)

Im Folgenden finden Sie die Befehle zum Erstellen des Links:

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


Hier finden Sie ein Beispiel für einen ausgefüllten Befehl zur Referenz:

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### Entwicklermodus ( Windows 10 )

Bei [Entwicklermodus](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), können Sie mit Windows 10 problemlos Apps testen, die Sie entwickeln, die Ubuntu Bash-Shell-Umgebung verwenden, eine Vielzahl von Entwicklereinstellungen ändern und andere Dinge tun.

Microsoft scheint weiterhin Funktionen zum Entwicklermodus hinzuzufügen oder einige dieser Funktionen standardmäßig zu aktivieren, sobald sie eine breitere Anwendung erreichen und als stabil betrachtet werden (z. B. bei Creators Update benötigt die Ubuntu Bash Shell-Umgebung keinen Entwicklermodus mehr).

Was ist mit Symlinks? Wenn der Entwicklermodus AKTIVIERT ist, müssen Sie keine Eingabeaufforderung mit erhöhten Berechtigungen ausführen, um Symlinks erstellen zu können. Sobald der Entwicklermodus aktiviert ist, können alle Benutzer daher Symlinks erstellen.

> Nach der Aktivierung des Entwicklermodus sollten Benutzer sich anmelden/anmelden, damit die Änderungen wirksam werden.

Jetzt können Sie sehen, dass der Befehl funktioniert, ohne als Administrator ausgeführt zu werden

![Bild der Windows-Eingabeaufforderung, die als normaler Benutzer mit aktiviertem Entwicklermodus ausgeführt wird](./assets/git-symlinks/windows-mklink-devmode.png)

#### Alternativer/programmatischer Ansatz

Es gibt eine bestimmte Richtlinie, die es bestimmten Benutzern ermöglichen kann, symbolische Links zu erstellen → [Erstellen von symbolischen Links (Windows 10) - Windows-Sicherheit | Dokumente für Microsoft](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PROs:
- Dies kann von Kunden genutzt werden, um die Erstellung symbolischer Links programmgesteuert allen Entwicklern in ihrer Organisation (d. h. Active Directory) zu ermöglichen, ohne den Entwicklermodus manuell auf jedem Gerät aktivieren zu müssen.
- Darüber hinaus sollte diese Richtlinie in früheren Versionen von MS Windows verfügbar sein, die keinen Entwicklermodus bieten.

CONs:
- Diese Richtlinie scheint keine Auswirkungen auf Benutzer zu haben, die der Gruppe Administratoren angehören. Administratoren müssen weiterhin die Eingabeaufforderung mit erhöhten Berechtigungen ausführen. Seltsam.

> Die Änderungen an der lokalen/Gruppenrichtlinie können nur wirksam werden, wenn der Benutzer sich anmelden/anmelden muss.

Ausführen `gpedit.msc`, fügen Sie nach Bedarf Benutzer hinzu/ändern Sie diese. Administratoren sind standardmäßig vorhanden

![Fenster &quot;Gruppenrichtlinien-Editor&quot;mit Berechtigung zum Anpassen](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### Aktivieren von Symlinks in GIT

Git verarbeitet Symlinks gemäß der Option core.symlinks

Quelle: [Git - Dokumentation zu git-config](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*Wenn &quot;core.symlinks&quot;auf &quot;false&quot;gesetzt ist, werden symbolische Links als kleine einfache Dateien ausgecheckt, die den Linktext enthalten. `git-update-index[1]` und `git-add[1]` ändert den aufgezeichneten Typ nicht in eine reguläre Datei. Nützlich für Dateisysteme wie FAT, die keine symbolischen Links unterstützen.
Der Standardwert ist &quot;true&quot;, mit Ausnahme von `git-clone[1]` oder `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` In den meisten Fällen geht Git davon aus, dass Windows für Symlinks nicht geeignet ist, und setzt dies auf &quot;false&quot;.*

Das Verhalten von Git unter Windows wird hier ausführlich erläutert: Symbolische Links ・ Git-for-Windows/Git Wiki ・ GitHub

> `Info`: Die in der oben stehenden Dokumentation aufgelisteten Annahmen scheinen mit einer möglichen AEM-Einrichtung des Entwicklers unter Windows, insbesondere NTFS und der Tatsache, dass wir nur Dateisymlinks im Vergleich zu Verzeichnissymlinks haben, in Ordnung zu sein

Hier ist die gute Nachricht, denn [Git für Windows-Version 2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) Das Installationsprogramm verfügt über eine [explizite Option zur Aktivierung der Unterstützung von symbolischen Links.](https://github.com/git-for-windows/git/issues/921)

> `Warning`: Die Option core.symlink kann während des Klonens des Repositorys zur Laufzeit bereitgestellt werden oder anderweitig als globale Konfiguration gespeichert werden.

![Anzeigen des GIT-Installationsprogramms mit Unterstützung für Symlinks](./assets/git-symlinks/windows-git-install-symlink.png)

Git für Windows speichert globale Voreinstellungen in `"C:\Program Files\Git\etc\gitconfig"` . Diese Einstellungen werden von anderen Git-Desktop-Client-Apps möglicherweise nicht berücksichtigt.
Im Folgenden finden Sie den Haken: Nicht alle Entwickler verwenden den nativen Git-Client (d. h. Git Cmd, Git Bash) und einige der Git Desktop-Apps (z. B. GitHub Desktop, Atlassian Sourcecenter) können unterschiedliche Einstellungen/Standardeinstellungen für die Verwendung von System oder einem eingebetteten Git haben.

Hier finden Sie ein Beispiel für das, was sich im `gitconfig` file

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

Es kann Situationen geben, in denen Sie neue symbolische Verknüpfungen erstellen müssen (z. B. das Hinzufügen eines neuen Hosts oder einer neuen Farm).

Wir haben in der obigen Dokumentation gesehen, dass Windows einen &quot;mklink&quot;-Befehl zum Erstellen symbolischer Links anbietet.

Wenn Sie in einer Git Bash-Umgebung arbeiten, können Sie stattdessen den standardmäßigen Bash-Befehl verwenden `ln -s` aber es muss durch eine spezielle Anweisung wie die folgende vorangestellt werden:

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### Zusammenfassung

Um die korrekte Git-Handhabung von Symlinks (zumindest für den Umfang der aktuellen AEM Dispatcher-Konfigurationsgrundlinie) in einem Microsoft Windows-Betriebssystem zu erhalten, benötigen Sie:

| Element | Mindestversion/Konfiguration | Empfohlene Version/Konfiguration |
|------|---------------------------------|-------------------------------------|
| Betriebssystem | Windows Vista oder höher | Windows 10 Creator-Update oder höher |
| Dateisystem | NTFS | NTFS |
| Funktion zum Verarbeiten von Symlinks für Windows-Benutzer | `"Create symbolic links"` Gruppe/lokale Richtlinie `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Entwicklermodus für Windows 10 aktiviert |
| GIT | Native Client-Version 1.5.3 | Native Client-Version 2.10.2 oder höher |
| Git-Konfiguration | `--core.symlinks=true` Option beim Ausführen eines Git-Klons über die Befehlszeile | Globale Git-Konfiguration<br/>`[core]`<br/>    symlinks = true <br/> Nativer Git-Client-Konfigurationspfad: `C:\Program Files\Git\etc\gitconfig` <br/>Standardspeicherort für Git Desktop-Clients: `%HOMEPATH%\.gitconfig` |

> `Note:` Wenn Sie bereits über ein lokales Repository verfügen, müssen Sie von der Quelle aus neu klonen. Sie können an einem neuen Speicherort klonen und Ihre nicht zugewiesenen/nicht gestaffelten lokalen Änderungen manuell in das neu geklonte Repository zusammenführen.