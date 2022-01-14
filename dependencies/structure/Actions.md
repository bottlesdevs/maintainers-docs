# Actions
These are the one step instruction type (which we have seen before), Bottles
supports several and will certainly be extended in the future. However, these
alone are not enough and require other parameters, sometimes common and other
times specific to the action. We will cover them all in this documentation.

## Actions table
The following table lists all the actions that Bottles supports for
dependencies steps.

| Action | Description |
| ------ | ----------- |
| `install_exe` | Install an executable |
| `install_msi` | Install an MSI |
| `download_archive` | Download an archive |
| `archive_extract` | Extract an archive |
| `cab_extract` | Extract a Windows Cabinet |
| `get_from_cab` | Extract a file from a Windows Cabinet |
| `override_dll` | Override a DLL |
| `uninstall` | Call an uninstaller by its name |
| `set_windows` | Set the current Windows version |
| `set_register_key` | Set a Windows registry key |
| `copy_file` or `copy_dll` | Copy a file from one place to another |
| `install_fonts` | Copy one or more fonts to the bottle windows/Fonts path |
| `register_font` | Register a font in the Windows registry |
| `replace_font` | Replace a font replacement in the Windows registry |

> This page is not complete.