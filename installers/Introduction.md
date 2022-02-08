---
title: Installers
description: Installers are the way to automate the installation of the softwares.
---

# Installers
Introduced in Bottles 2022.2.14, installers are set of rules which Bottles uses
to automate the installation of the softwares. They define the required
dependencies, tweaks and other things to install a software. We call them
Actions and they are defined in the [Actions](/installers/Actions) section.

## How Bottles handles installers?
Bottles is not just another wineprefix manager, it comes with a set of
integrated managers (e.g. the dependency manager, the WINE backend, runtime ..) 
that all together manage the operations of your bottles. The installers 
integrate with all of these managers, functioning as an orchestrator and 
giving orders to each one to reproduce an installation (installing 
dependencies, updating the bottle configuration, launching executables, editing
configuration files, etc.).

Even if this system has already been seen in other software, in Bottles 
everything is managed internally and no external tools are used. This 
choice is given by the need to have an extremely integrated and easily 
traceable system, as well as ensuring a better user experience and avoiding 
the "puzzle" sensation that sometimes fails.

## When should installers be used?
Installers automate the installation process but that doesn't mean you should 
always use them. These require maintenance as programs receive updates and 
resources can change location. For this reason it is convenient to use this 
system only for programs that require interventions to be installed.

Let's take some examples:
- Epic Games Launcher  
  it requires some dependencies to works, so it is a good idea to make an
  installer for it.
- Notepad++
  it doesn't require any dependencies or tweaks, so an installer is not
  necessary.

This does not mean that you cannot create installers for programs that work 
out of the box but that you need to consider whether you will have the time to 
keep them up to date and therefore worth it.