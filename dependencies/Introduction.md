---
title: Dependencies
description: Dependencies are the way Bottles extend your bottles compatibility.
---

# Dependencies
These are packages which play a crucial role in Bottles, providing one 
or more dependencies, necessary to make the Windows software to work, 
increasing the compatibility level of the bottles.

## What is a dependency?
Like said before, a dependency is a package which is necessary to make
the Windows software to work properly, increasing the compatibility level
of the bottles.

We can compare this to the classic packages used by Linux distributions, let's
make an example: imagine installing a program in your distribution of choice,
this will depend by other packages (dependencies) as without these it will not 
be able to work at its best way or at all.

So in Bottles (generally WINE) it's most the same: when you install a Windows
program, it will depend by other packages (dependencies) to make it work
properly.

## How Bottles handles dependencies?
Bottles has its own independent dependencies manager which takes care of
installation (in multiple ways) and removal.

Our dependencies system is based on a public repository, which is a list of
manifests in YAML format with a specific set of instructions to handle the
package, and an index of all the packages available on it.

When you start Bottles, it will download the dependencies index from the
repository and list them in your bottle's Dependencies view. When you require
a dependency, it will download the specific manifest and execute the
instructions to install it.

## Different installation methods
Dependencies are not all installed in the same way, some can be installed by
simply running the executable, while others will require more complex steps
such as extracting a single file from a Windows Cabinet, registering a new
override, ...

The following example shows how the `dirac` dependency is installed:

```yaml
Name: dirac 
Description: The Dirac directshow filter v1.0.2
Provider: Dirac
License: GPLv2
License_url: https://www.gnu.org/licenses/old-licenses/gpl-2.0.html
Dependencies: []
Steps:
- action: install_exe
  file_name: DiracDirectShowFilter-1.0.2.exe
  url: https://downloads.sourceforge.net/dirac/DiracDirectShowFilter-1.0.2.exe
  rename: DiracDirectShowFilter-1.0.2.exe
  arguments: []
  file_checksum: 6bb4fd577ff881f317a9361d986907c4
```

It is a very simple installation, as you can see, it has only one step, the
`install_exe` action, which will just download the file and run it. Let's
see a more complex example like `dotnet48`:

```yaml
Name: dotnet48
Description: Microsoft .NET Framework 4.8
Provider: Microsoft
License: Microsoft EULA
License_url: https://www.microsoft.com/web/webpi/eula/net_library_eula_enu.htm
Dependencies: 
  - dotnet40
  
Steps:
- action: uninstall
  file_name: Wine Mono

- action: set_windows
  version: win7
  
- action: install_exe
  environment: 
    WINEDLLOVERRIDES: fusion=b
  file_name: ndp48-x86-x64-allos-enu.exe
  url: https://download.visualstudio.microsoft.com/download/pr/7afca223-55d2-470a-8edc-6a1739ae3252/abd170b4b0ec15ad0222a809b761a036/ndp48-x86-x64-allos-enu.exe
  rename: dotnet48.exe
  file_checksum: 4e7a8586d06ce1f8cebac0d4bc379e16
  arguments: /sfxlang:1027 /q /norestart

- action: set_windows
  version: win10

- action: override_dll
  dll: mscoree
  type: native
```

Here we have five steps with different actions:
- `uninstall`: will uninstall the Wine Mono package as this dependency
  will replace it
- `set_windows`: will set the Wine environment to Windows 7, as the
  dependency will fail on different versions of Windows
- `install_exe`: will download the file and run it
- `set_windows`: will set the Wine environment back to Windows 10 (the
  default one in Bottles)
- `override_dll`: will override the `mscoree` DLL to `native` so the program
  will find it

You can see that the `install_exe` action has multiple keys:
- `environment`: will inject an environment variable registering the
  `fusion` DLL override only for the installation process
- `arguments`: these are the arguments to be passed to the executable to make
    it silent and not ask for any input and set the default language

Maybe you noticed that this manifest also fill the `Dependencies` key, this
is because dependencies can also depend on other dependencies, so the
`Dependencies` key is a list of dependencies which should be installed
before this one. 

There are also more complex actions but we will not cover them here, refer
to the [Actions](/dependencies/structure/Actions.md) page for more information.
