# platform-gd32v-mac-unofficial
The platform package for RISCV processors is available in the offical repositories for PlatformIO. Instructions can be found [here](https://docs.platformio.org/en/latest/platforms/sifive.html). Additional documentation and tutorials can be found on the board vendor pages, e.g. [Longan Nano](https://longan.sipeed.com/en) or [Maixduino](https://maixduino.sipeed.com/en/)
The official repository gd32v from sipeed for PlatformIO currently does not support MacOS (toolchain, flash tool, etc.). When trying to install this platform on a Mac you will see an error like:
    
    PackageManager: Installing toolchain-gd32v @ ~9.2.0
    Error: Could not find a version that satisfies the requirement '~9.2.0' for your system 'darwin_x86_64'

 There is a known issue for this [here](https://github.com/sipeed/platform-gd32v/issues/6)
 
 This project is an unoffical for of the offical Git Repository <https://github.com/sipeed/platform-gd32v>. It provides supporrt for MacOS.
 
# Status
This is an inoffical fork and in an experimental, unsupported state. Do not expect stability or production quality. Use this at your own risk. It is intended to be a temporary workaround until official packages are available).


# Usage
You can install this platform project from the PlatformIO IDE VSCode or from the command line client. For additional information see the PlatformIO [documentation pages](https://docs.platformio.org)
It is not recommended to install this in parallel with official gd32v package (it is possible but there may be conflicts).

## Install with PLatformIO CLI (command line)
    platformio platform install https://github.com/yesitsme007/platform-gd32v-mac-unofficial.git
Uninstall:
    
    platformio platform uninstall gd32v-mac-unofficial
    
## Install with PlatformIO IDE (VSCode)
Go to PIO Home in VSCode. Select platforms on the left pane.
Choose "Advanced installation", enter "https://github.com/yesitsme007/platform-gd32v-mac-unofficial.git" in the text box.
Click on "Install" button.

Uninstall:
Goto platforms, select Installed, browse and select "GigaDevice GD32V Platform with Toolchain for Mac (unofficial fork)
click uninstall button.

### Create new project
You can create a new project based on the existing example projects or using the PlatformIO CLI tool.

    mkdir myproject
    pio init -d myproject -b sipeed-longan-nano --project-option "framework=arduino" --project-option "upload_protocol=dfu"
    
Creation from the IDE fails with an error but the project files get created and can be imported in VSCode. The generated file `platformio.ini` has to be edited. The platform needs to be set to `gd32v-mac-unofficial`. Typically you will use the `dfu` upload protocol if you upload via USB for the Longan Nano. It should look similar to this example:

    [env:sipeed-longan-nano]
    platform = gd32v-mac-unofficial
    board = sipeed-longan-nano
    framework = arduino
    upload_protocol = dfu

###Useful commands:

    pio init -d myproject -b sipeed-longan-nano
    pio init -d myproject -b sipeed-longan-nano --project-option "framework=arduino" --project-option "upload_protocol=dfu"
    platformio run --target clean
    platformio run -e sipeed-longan-nano
    platformio run -e sipeed-longan-nano --target upload
    platformio debug

# License
This is collection of various other open source projects. The licenses of the corresponding projects apply. For additonal parts see [LICENSE](LICENCSE.md) file

1. Please DO NOT USE this at the moment. This is work in progress and not functional. 
Try the [offical repository](https://github.com/sipeed/platform-gd32v)

# Notes
* It seems that PlatformIO does not support extending existing projects with new additional package repositories. Also this project slightly differs from the original one. Therefore it was created as a fork.
* The version numbers in the contained packes match the ones from the official packages. However they do not necessarily match with the really installed packages.
* The toolchain for compilation is different from the offical repository. It uses the version available from the [Sipeed Downlod page](https://www.sifive.com/boards/). I could not find any link to a repository for the 9.2.0 compiler in the offical package or any documentation how to build it.
* Other projects used:
    * dfu-util <https://github.com/riscv-mcu/gd32-dfu-utils> (patched version) 
    * OpenOCD: <https://github.com/riscv/riscv-openocd> 
    * stm32flash: <https://git.code.sf.net/p/stm32flash/code>
* The package repository uses Github releases with attachments to provide the binary archives. See <https://github.com/yesitsme007/platformio-gd32v-mac-unofficial-repo>

# Known Issues
* Compile project for the first without having option `--target upload` causes error:

`Building .pio/build/sipeed-longan-nano/firmware.hex
Adding dfu suffix to firmware.bin
sh: dfu-suffix: command not found
*** [.pio/build/sipeed-longan-nano/firmware.bin] Error 127`

This is an issue in the current master branch of the official project:
Workaround:

`run: platformio run -e sipeed-longan-nano --target upload`

This will install dfu-util utilities, then `pio run` will also succeed

* New project wizard in VSCode terminates with error:

`PIO Core Call Error: "The current working directory /Users/d058463/Documents/PlatformIO/Projects/test4  will be used for
 the project.\n\nThe next files/directories have been created in /Users/d058463/Documents/PlatformIO/Projects/test4 \n
 include - Put project header files here\nlib - Put here project specific (private) libraries\nsrc - 
 Put project source files here\nplatformio.ini - 
 Project Configuration File\n\n\nError: Could not find a version that satisfies the requirement '~9.2.0' for your 
 system 'darwin_x86_64'"`

Workaround: Use command line client to create a new project, e.g.

    mkdir myproject
    pio init -d <myproject> -b sipeed-longan-nano --project-option "framework=arduino" --project-option "upload_protocol=dfu"




