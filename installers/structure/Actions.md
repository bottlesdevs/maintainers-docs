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
| `run_winecommand` | Run a command using the WineCommand interface |

## `install_exe` or `install_msi`
This action is used to install an executable.

### Parameters

| Key | Description |
| --- | ----------- |
| `file_name` | The name of the file to be installed |
| `url` | The URL or path of the file to be downloaded (e.g. set it to `"local"` to ask the user for a local file) |
| `rename` | The new name of the file if it must be renamed |
| `file_checksum` | The checksum of the file (MD5) |
| `arguments` | The arguments to be passed to the executable |
| `environment` | The environment variables to be injected |

> All local resources are collected by Bottles as first step and then
> used by the installer when needed.

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

## `run_script`
This action is used to define and run a shell script. It is useful to
execute operations when an action is not provided by Bottles.

> Note: this action should be used with caution, as it can be dangerous
> to run arbitrary commands.

### Parameters

| Key | Description |
| --- | ----------- |
| `script` | The script to be executed |

To avoid damage to a minimum, this action comes with some restrictions but 
also with some placeholders to facilitate the creation of scripts.

- accessing the `bottle.yml` will result in an error, use the `Parameters`
  key instead
- to get the bottle name use the `!bottle_name` placeholder
- to get the bottle path use the `!bottle_path` placeholder
- to get the bottle drive_c path use the `!bottle_drive` placeholder

### Example
```yaml
- action: run_script
  script: |
    echo "Bottle is: !bottle_name"
    echo "Bottle path:"
    ls !bottle_path
    echo "Bottle drive_c:"
    ls !bottle_drive
```


## `run_winecommand`
*Since 2022.4.28*

This action is used to define a list of commands to be executed inside the
prefix using the WineCommand interface.

> Note: this action should be used with caution, as it can be dangerous
> to run arbitrary commands.

### Parameters

| Key | Description |
| --- | ----------- |
| `commands` | The list of commands to be executed |

Optionally, the following parameters can be used to configure the WineCommand
per each command:

| Key | Description |
| --- | ----------- |
| `minimal` | If set to true, the command will be executed in a minimal environment |
| `arguments` | The arguments to be passed to the command |

### Example
```yaml
- action: wine_winecommand
  commands:
    - command: start steam://install/39140
      arguments: /S
      minimal: true
    - command: wineboot -u
      minimal: true
```
