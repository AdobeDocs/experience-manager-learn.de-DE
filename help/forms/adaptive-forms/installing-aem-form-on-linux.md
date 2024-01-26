---
title: Installieren von AEM Forms unter Linux
description: Erfahren Sie, wie Sie 32-Bit-Bibliotheken für AEM Forms zur Verwendung in Linux-Installationen installieren.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 245
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 100%

---

# Installieren der 32-Bit-Version von gemeinsam genutzten Bibliotheken

Wenn AEM FORMS OSGi oder AEM Forms j2EE unter Linux bereitgestellt wird, müssen Sie sicherstellen, dass 32-Bit-Versionen für einen Satz gemeinsam genutzter Bibliotheken installiert und verfügbar sind. Die Beschreibungen stammen aus den Paketen selbst.

* expat (Stream-orientierte XML-Parser-C-Bibliothek zum Parsen von XML, die von James Clark geschrieben wurde).
* fontconfig (Bibliothek zur Konfiguration und Anpassung von Schriftarten, die entwickelt wurde, um Schriftarten im System zu finden und sie gemäß den von Anwendungen festgelegten Anforderungen auszuwählen).
* freetype (Render-Engine für Schriftarten, die entwickelt wurde, um eine erweiterte Schriftartunterstützung für eine Vielzahl von Plattformen und Umgebungen bereitzustellen). Sie kann Schriftartdateien öffnen und verwalten sowie einzelne Glyphen effizient laden, darauf hinweisen und diese rendern. Es handelt sich weder um einen Schriftarten-Server noch um eine vollständige Text-Rendering-Bibliothek.
* glibc (Kernbibliotheken für das GNU-System und GNU/Linux-Systeme sowie viele andere Systeme, die Linux als Kernel verwenden).
* libcurl (Client-seitige URL-Transferbibliothek).
* libICE (Inter-Client Exchange Library).
* libicu (Bibliothek, die robuste und umfassende Unicode- und Gebietsschema-Unterstützung bietet – International Components for Unicode). Es sind die 64-Bit- und die 32-Bit-Edition dieser Bibliothek erforderlich.
* libSM (X11 Session Management-Bibliothek).
* libuuid (DCE-kompatible Universally Unique Identifier-Bibliothek – zur Generierung eindeutiger Kennungen für Objekte, auf die außerhalb des lokalen Systems zugegriffen werden kann).
* libX11 (Client-seitige X11-Bibliothek).
* libXau (X11-Autorisierungsprotokoll – nützlich zur Beschränkung des Client-Zugriffs auf die Anzeige).
* libxcb (X-Protokoll-C-Sprachenbinding – XCB).
* libXext (Bibliothek für allgemeine Erweiterungen des X11-Protokolls).
* libXinerama (X11-Erweiterung zur Desktop-Erweiterung über mehrere Anzeigen hinweg.) Der Name spielt auf den Begriff „Cinerama“ an, ein Widescreen-Filmformat, bei dem mehrere Projektoren eingesetzt wurden. libXinerama ist die Bibliothek, die über eine Schnittstelle mit der RandR-Erweiterung verbunden ist.
* libXrandr (die Xinerama-Erweiterung ist heutzutage weitgehend veraltet; sie wurde durch die RandR-Erweiterung ersetzt).
* libXrender (Client-Bibliothek der X-Rendering-Erweiterung). 
nss-softokn-freebl (Freebl-Bibliothek für Netzwerksicherheitsdienste).
* zlib (universelle, patentfreie, verlustfreie Datenkomprimierungsbibliothek).

Ab Red Hat Enterprise Linux 6 haben 32-Bit-Editionen einer Bibliothek die Dateinamenerweiterung „.686“, 64-Bit-Editionen hingegen „.x86_64“, z. B. „expat.i686“. Vor RHEL 6 hatten 32-Bit-Editionen die Erweiterung „.i386“. Stellen Sie vor der Installation von 32-Bit-Editionen sicher, dass die neuesten 64-Bit-Editionen installiert sind. Wenn die 64-Bit-Version einer Bibliothek älter ist als die installierte 32-Bit-Version, erhalten Sie einen Fehler wie den folgenden:

0mError: Protected multilib versions: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Multilib version problems found.]

## Erstinstallation

Verwenden Sie unter Red Hat Enterprise Linux den YellowDog Update Modifier (YUM) zur Installation, wie im Folgenden angegeben:

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

Außerdem müssen Sie libcurl.so-, libcrypto.so- und libssl.so-Symlinks erstellen, die auf die neuesten 32-Bit-Versionen der libcurl-, libcrypto- und libssl-Bibliotheken verweisen. Sie finden die Dateien unter: /usr/lib/
-s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
-s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
-s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Aktualisierungen des vorhandenen Systems

Es kann bei Aktualisierungen zu Konflikten zwischen der x86_64- und der i686-Architektur kommen, z. B.:
Error: Transaction check error:
file /lib/ld-2.28.so from install of glibc-2.28-72.el8.i686 conflicts with file from package glibc32-2.28-42.1.el8.x86_64

Deinstallieren Sie bei Anzeige dieses Fehlers zuerst das fehlerhafte Paket, in diesem Fall:
yum remove glibc32-2.28-42.1.el8.x86_64

Schließlich und endlich möchten Sie, dass die x86_64- und i686-Versionen identisch sind, z. B. wie bei der nachfolgenden Ausgabe dieses Befehls:
yum info glibc

Last metadata expiration check: 0:41:33 ago on Sat 18 Jan 2020 11:37:08 AM EST.
Installed Packages
Name : glibc
Version : 2.28
Release : 72.el8
Architecture : i686
Size : 13 M
Source : glibc-2.28-72.el8.src.rpm
Repository : @System
From repo : BaseOS
Summary : The GNU libc libraries
URL : http://www.gnu.org/software/glibc/
License : LGPLv2+ and LGPLv2+ with exceptions and GPLv2+ and GPLv2+ with exceptions and BSD and Inner-Net and ISC and Public Domain and GFDL
Description : The glibc package contains standard libraries which are used by : multiple programs on the system. In order to save disk space and : memory, as well as to make upgrading easier, common system code is : kept in one place and shared between programs. This particular package : contains the most important sets of shared libraries: the standard C : library and the standard math library. Without these two libraries, a : Linux system will not function.

Name : glibc
Version : 2.28
Release : 72.el8
Architecture : x86_64
Size : 15 M
Source : glibc-2.28-72.el8.src.rpm
Repository : @System
From repo : BaseOS
Summary : The GNU libc libraries
URL : http://www.gnu.org/software/glibc/
License : LGPLv2+ and LGPLv2+ with exceptions and GPLv2+ and GPLv2+ with exceptions and BSD and Inner-Net and ISC and Public Domain and GFDL
Description : The glibc package contains standard libraries which are used by : multiple programs on the system. In order to save disk space and : memory, as well as to make upgrading easier, common system code is : kept in one place and shared between programs. This particular package : contains the most important sets of shared libraries: the standard C : library and the standard math library. Without these two libraries, a : Linux system will not function.

## Auswahl nützlicher Yum-Befehle

yum list installed
yum search [Teil_des_Paketnamens]
yum whatprovides [Paketname]
yum install [Paketname]
yum reinstall [Paketname]
yum info [Paketname]
yum deplist [Paketname]
yum remove [Paketname]
yum check-update [Paketname]
yum update [Paketname]
