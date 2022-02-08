---
title: Structure - Installers
description: Structure of an installer manifest in Bottles.
---

# Installers structure
Each installer is a set of instructions and these are saved in manifests 
(YAML files) which are interpreted by Bottles. For this it is important that 
the structure of these is correct, otherwise Bottles will not be able to 
process them.

## Manifest
Like [Dependencies](/dependencies/structure) each manifest has two parts:
- header
- steps

### Header
The header is the first part of the manifest, it contains the metadata of the
installer and its dependencies if any.

The header must provide the following keys:
- `Name`: the name of the installer, it must be unique and normally it is the
  same as the name of main download file
- `Description`: this is a small description of the installer (e.g. for
  epicgamestore it is "The official Blizzard Battle.net launcher")
- `Grade`: the grade of compatibility of the installer (Bronze, Silver, Gold, 
  Platinum) this should be set to Undefined and updated by the maintainer
  according to the Final grade defined in the review (we will see it later)
- `Arch`: the architecture of the bottle (win32, win64)
- `Dependencies`: a list of dependencies that this installer depends on
- `Parameters`: a list of parameters to be set in the bottle configuration
- `Executable`: the entry to be added to the bottle Programs list, can be
  used to configure arguments and other things

These keys are used by Bottles to show the installer in the Installers view
and handle its requirements before proceeding with the installation. Let's see
an example:

```yaml
Name: battlenet
Description: The official Blizzard Battle.net launcher.
Grade: Gold
Arch: win64

Dependencies:
- msls31
- riched20
- arial32
- vcredist2015
```

In this example, the installer is named `battlenet`, the grade is `Gold`, the
needed bottle arch is `win64` and it depends on the following dependencies:
- `msls31`
- `riched20`
- `arial32`
- `vcredist2015`

### Steps
Like dependencies, each installer has a set of steps that must be executed
in order to install the software. Each step define an action type and a set
of parameters.

**Note:** the steps are executed in the order they are listed in the manifest,
so be careful when you add a new one, if your new step depends on the previous
one, you should add it after the previous one.

### Comments
Despite YAML is a simple language and our installer syntax is very simple and
should be very easy to read, it is still possible to write comments in the
manifest to explain what is going on or why it is needed:

```yaml
..
Steps:
- action: install_msi
  file_name: Battle.net-Setup-enUS.exe
  url: http://dist.blizzard.com/downloads/bna-installers/322d5bb9ae0318de3d4cde7641c96425/retail.1/Battle.net-Setup-enUS.exe
  file_checksum: e413dfc296c3701416e3fe5af45aedbf
```
