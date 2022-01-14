# Dependencies structure
We already introduced the dependencies structure in the Introduction, here we
will go more in depth and explain the different parts of the dependency.

## Manifest
This file is the way we instruct Bottles to install the dependency, it is very
important that it is written well and that you do not use wrong instructions 
that can dirty Bottles temp folder or, worse, the user's bottles (this it 
should never happen and is why every dependency needs to be reviewed by the 
team before being deployed).

Each manifest has two parts:
- header
- steps

### Header
The header is the first part of the manifest, it contains the metadata of the
dependency and its dependencies if any.

The header must provide the following keys:
- `Name`: the name of the dependency, it must be unique and normally it is the
  same as the name of main download file
- `Description`: despite the name, this is the full name of the dependency
  e.g. for the `dotnet40` package, the description will be `Microsoft .NET Framework 4.0`
- `Provider`: the name of the provider of the dependency (e.g. Microsoft)
- `License`: the license of the dependency, it must be a license name
    (e.g. `Microsoft EULA`)
- `License_url`: the URL of the license of the dependency
- `Dependencies`: a list of dependencies that this dependency depends on

These keys are used by Bottles to show the dependency in the Dependencies view
and handle its requirements before proceeding with the installation. Let's see
an example:

```yaml
Name: d3dx9
Description: Microsoft d3dx9 DLLs from DirectX 9 redistributable
Provider: Microsoft
License: Microsoft EULA
License_url: https://www.microsoft.com/web/webpi/eula/net_library_eula_enu.htm
Dependencies: []
```

In this example, the dependency is named `d3dx9`, its full name is 
`Microsoft d3dx9 DLLs from DirectX 9 redistributable`, it is provided by
`Microsoft` and it is licensed under the `Microsoft EULA`. It does not depend
on any other dependency so the `Depdendencies` list is kept empty.

### Steps
This is where the magic happens, the steps are the instructions to install the
dependency. Each dependency can have multiple steps, and each step can have
only one action. Actions are the type of instruction to perform (like `install_exe` to install an executable, `get_from_cab` to extract a file from a Windows Cabinet, etc).

**Note:** the steps are executed in the order they are listed in the manifest,
so be careful when you add a new one, if your new step depends on the previous
one, you should add it after the previous one.

### Comments
Despite YAML is a simple language and our dependency syntax is very simple and
should be very easy to read, it is still possible to write comments in the
manifest to explain what is going on or why it is needed:

```yaml
..
Steps:
# we need more than one file from cab, so we are going to just extract
# all the files and placing them in the directx_Jun2010_redist whcih
# will be automatically created by Bottles
- action: cab_extract
  file_name: directx_Jun2010_redist.exe
  url: https://download.microsoft.com/download/8/4/A/84A35BF1-DAFE-4AE8-82AF-AD2AE20B6B14/directx_Jun2010_redist.exe
  # urlAlt: http://web.archive.org/web/20210513223919/https://download.microsoft.com/download/8/4/A/84A35BF1-DAFE-4AE8-82AF-AD2AE20B6B14/directx_Jun2010_redist.exe
  file_checksum: 822e4c516600e81dc7fb16d9a77ec6d4
  dest: temp/directx_Jun2010_redist/

