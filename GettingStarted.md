# Getting Started
## Using chocolatey
Now that you have chocolatey on your machine ([[need to install?|Installation]]), you can run several commands.

Take a look at the [[command reference|CommandsReference]]. We are going to be using the [[install command|CommandsInstall]].

Let's install [Notepad++](http://notepad-plus-plus.org/).

1. Open a command line.
1. Type `choco install notepadplusplus` and press Enter.
1. If you have UAC turned on or are not an administrator, chocolatey is going to request administrative permission at some point (at least once during the process). Otherwise it will not be able to finish what it is doing successfully. If you don't have UAC turned on, it will just continue on without stopping to bother you.
1. That's it. Pretty simple but powerful little concept!

### Overriding default install directory or other advanced install concepts

1. Yes we support that through the use of installargs - see [[CommandsInstall#installarguments]]
1. If you wanted to pass native argument to the installer, like the install directory, you would need to know the silent argument passed to that particular installer and then you would specify it on the command line or in the packages.config (upcoming for packages.config).
1. If it was an MSI, then usually you could pass `-ia 'INSTALLDIR=''D:\Program Files'''` (note the double `'` - those get translated to double quotes `"`, posh seems to want to rip them out.
1. For example, Notepad++ uses the [NSIS](http://nsis.sourceforge.net/Main_Page) (NullSoft Scriptable Install System) installer. If we look at the silent options, we see that [/D](http://nsis.sourceforge.net/Docs/Chapter3.html#installerusagecommon) is how we influence the install directory. So we would pass `cinst notepadplusplus.install -ia '/D=E:\SomeDirectory\somebody\npp'` -note that we are looking at the specific package over the virtual (this will be corrected in future releases).

## How does Chocolatey work?
When a package has an exe file, Chocolatey will create a link "shortcut" to the file so that you can run that tool anywhere on the machine.
When a package has a chocolateyInstall.ps1, it will run the script. The instructions in the file can be anything. This is limited only by the .NET framework and PowerShell.
Most of the Chocolatey packages that take advantage of the PowerShell download an application installer and execute it silently.


## Where are Chocolatey packages installed to?

Chocolatey packages are installed to `ChocolateyInstall\lib`, but the software could go to various locations, depending on how the package maintainer created the package.

Some packages are installed under `ChocolateyInstall\lib`, others - especially packages that are based on Windows installers (.msi files) - install to the default path of the original installer (which is most likely within `Program Files`).

There are also packages for which you can set a custom installation path. These packages (like ruby) use the `$env:ChocolateyBinRoot` environment variable. If this variable does not exist, it will be created as `c:\tools` e.g. `C:\tools\ruby193`. To change this behaviour, you can set `$env:ChocolateyBinRoot` to an existing folder, e. g. `C:\somestuff`. Packages that use the environment variable, will then be installed in the given subfolder, f. ex. `C:\somestuff\ruby193`.

## Where does Chocolatey install packages from?
By default it installs packages from chocolatey.org (the community feed). But you can change this by adding default sources and/or using the  `--source` switch when running a command.
