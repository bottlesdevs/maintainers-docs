---
title: Testing - Installers
description: How to test installers using a local repository.
---

# Testing
To test the installers we can proceed in the same way as for the [dependencies](/dependencies/Testing),
using the `LOCAL_INSTALLERS` environment variable pointing to the local 
repository.

Even if an installer seems easy to package and you feel confident doing it 
without testing, please don't. **Tests are essential** to ensure a safe and 
functional environment for the user.

As the process is the same as for the dependencies, we will only describe the
differences:

- the installers repository url is: https://github.com/bottlesdevs/programs.git
- the environment variable is: `LOCAL_INSTALLERS`

All the other steps are the same, so please refer to the [Testing](/dependencies/Testing)
documentation.
