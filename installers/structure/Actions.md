---
title: Actions - Installers
description: These are the actions supported in installers steps.
---

# Actions
Each step has an action type, Bottles uses this to figure out what operation to 
perform. Each of these determines a specific set of parameters.

Unlike dependencies, installers have fewer actions. More will certainly be 
added in the future. The installers should be kept as light and clear as 
possible, in case it is necessary to install external resources beyond the 
program for which the installer is born, it is convenient to create a 
dependency and declare it in the dependency list of the installer.

## Actions table
The following table lists all the actions that Bottles supports for
installers steps.

| Action | Description |
| ------ | ----------- |
| `install_exe` or `install_msi` | Install an executable |
| `update_config` | Update a configuration file (yaml, json, ini) |
| `run_script` | Run a shell script |

## `install_exe` or `install_msi`
This action is used to install an executable.

### Parameters

| Key | Description |
| --- | ----------- |
| `file_name` | The name of the file to be installed |
| `url` | The URL or path of the file to be downloaded |
| `rename` | The new name of the file if it must be renamed |
| `file_checksum` | The checksum of the file (MD5) |
| `arguments` | The arguments to be passed to the executable |
| `environment` | The environment variables to be injected |

### Example
```yaml
Steps:
- action: install_exe
  file_name: UplayInstaller.exe
  url: https://ubistatic3-a.akamaihd.net/orbit/launcher_installer/UplayInstaller.exe
  file_checksum: 1c2d115033a6a5f422c2074a44ad23b9
  arguments: /S
```

## `update_config`
This action is used to update a configuration file. It will also create it
if it does not exist. A common use case for this action is pre-configure
some configuration files, e.g. for the Uplay launcher to disable the glitched
overlay setting.

### Parameters

| Key | Description |
| --- | ----------- |
| `path` | The path of the file to be updated |
| `type` | The type of the file to be updated (yaml, json, ini) |
| `upd_keys` | The dictionary of keys to be updated |
| `del_keys` | The list of keys to be deleted |

> Note: usually configuration files are placed in the user diriectory, so
> to reach them, use the `userdir/` placeholder, Bottles will automatically
> replace it with the user directory.

### Example
```yaml
- action: update_config
  path: userdir/Local Settings/Application Data/Ubisoft Game Launcher/settings.yml
  type: yaml
  upd_keys: 
    overlay:
      enabled: false
      forceunhookgame: false
      warning_enabled: false
    user:
      closebehavior: CloseBehavior_Close
```