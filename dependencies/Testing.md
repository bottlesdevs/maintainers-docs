---
title: Testing - Dependencies
description: How to test dependencies using a local repository.
---

# Testing
To create and publish a dependency, 2 stages of testing are required:
- [Making it work](#making-it-work)
- [Testing via local manifest](#testing-via-local-manifest)

Even if a dependency seems easy to package and you feel confident doing it 
without testing, please don't. **Tests are essential** to ensure a safe and 
functional environment for the user.

## Making it work
Surely the most obvious phase: before packaging a dependency you need to 
make sure that it is possible to install.

### Basic concepts and requirements
Each dependency must be tested with the official Bottles **Caffe** runner to 
ensure compatibility as this is provided from the very first launch of Bottles.

Use a clean new bottle using the **Custom environment** to avoid other
dependencies to interfere with your tests.

If the dependency is offered by a Service Pack or otherwise a large installer 
that installs multiple resources, try to extract the contents and place the 
necessary files manually. This is a delicate operation, we advise you to read 
on WineDB if there are specific instructions.

### Try to make it work
Here are some tips to start testing dependency:
- save all the procedures you perform, even the smallest changes because you 
  will need them for the dependency manifest
- simply try running the vendor's official installer
- if the installer doesn't work, check in the WineDB if the dependency needs 
  a specific Windows version, other dependencies or tweaks
- if the installer doesn't work, try to extract the resources and place
  them manually in the correct folder(s)
- try a program that uses that dependency to make sure everything works

When everything is working, you can making the dependency manifest. Refer
to the [Structure](/dependencies/structure) for more information.

## Testing via local manifest
The second stage of testing is to test the dependency using your manifest.

First, you need to make a local repository. You can create this in your own 
but we recommend that you clone the git repository to ensure the structure 
and have access to other dependencies in case yours requires them:

```bash
git clone https://github.com/bottlesdevs/dependencies.git
```

Then, place your manifest in the right folder:
- Essentials/
- Fonts/

and update the `index.md` file with your dependency:

```yaml
package_name: # the file name of the manifest, should be the same of Name in the manifest
  Description: My awesome package # the description of the dependency (the full name)
  Category: Essentials # the category of the dependency (should respect the category path where it is placed)
```

To use the local repository, you need to launch Bottles using the 
`LOCAL_DEPENDENCIES` environment variable, pointing to your local repository:

```bash
LOCAL_DEPENDENCIES=/path/to/dependencies/ flatpak run com.usebottles.bottles
```

Then, try to install the dependency in a new bottle using the Dependencies
section.

If that doesn't work, check the terminal where you launched Bottles from and 
search for errors. If they are not present it means that there is some problem 
in the steps of your manifesto and you will have to check the instructions for 
any missing or errors (including typing).
