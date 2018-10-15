# stm32-development-in-linux

If you need/want to have STM32 development environment in Linux...

## Writer

* SJ Kim
* bus710@gmail.com

## Environment

* Mint Linux 18.3 64bit \(or equivalent distro\)
* STM32F446RE board (or equivalent board)
* J-Link debugger

---

## Index

* Get Basic Tools
* Get ARM-GCC Compiler
* Get JLink Package
* Get OpenJDK
* Get STM32CubeMX
* Generate an Example
* Compile the Example
* Get Eclipse (GNU-MCU-Eclipse and Vrapper Packages)
* Import the Example to Eclipse
* Set Build Option and Parser Configuration
* Set Debug Configuration in Eclipse
* Tips for Eclipse  
* Tips for Windows Users  
* Conclusion

---

### Get Basic Tools

```
$ sudo apt update
$ sudo apt install build-essential
$ sudo apt install git
```

### Get ARM-GCC Compiler

```
$ sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
$ sudo apt update
$ sudo apt install gcc-arm-embedded
$ arm-none-eabi-gcc -v
```

Tips:
- https://launchpad.net/gcc-arm-embedded
- https://developer.arm.com/open-source/gnu-toolchain/gnu-rm

### Get JLink Package

Download **J-Link Software and Documentation pack for Linux, DEB Installer, 64-bit** from [https://www.segger.com/downloads/jlink/\#J-LinkSoftwareAndDocumentationPack](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack)

Currently the version is 6.30a

```
$ sudo dpkg -i JLink_Linux_V630_x86_64.deb
```

To test it, connect your eval board and JLink to your PC and...

```
$ JLinkExe -device STM32F446RE -speed 1000 -if swd

// If JLinkExe starts without issue...
  
$ connect

// See the result and exit.

> exit
```

Tip: After installing, reboot may require.

## Get OpenJDK

STM32CubeMX and Eclipse require 32bit JRE.    

```
$ sudo apt install openjdk-11-jre:i386
``` 

## Get STM32CubeMX

STM32CubeMX is a util that can be used for biler-plating based on hardeware.  
[https://www.st.com/en/development-tools/stm32cubemx.html](https://www.st.com/en/development-tools/stm32cubemx.html)  
  
ST doesn't provide an open link for the util but we need to request.  
After the request, ST will send an email to us.  
  
After download, extract the zip file and run its installer as below.
```
$ sudo ./SetupSTM32CubeMX-4.27.0.linux
```
  
Tips
- After installing JRE, please use a new terminal for STM32CubeMX installation.

## Generate an Example

To have a basic code set, we have to run STM32CubeMX.  
I designed a minimal setup with the util and the design includes:  
- STM32F446RE
- 8MHz xtal
- A pin for LED
- A pair of UART
- Full speed USB and its stack.
- FreeRTOS

The Test_USB_CDC4.ioc file in this repo is the configuration for the project.  
To use the configuration:
- Run STM32CubeMX (The binary can be found from **/usr/local/STMicroelectronics/STM32Cube**)
- Load the configuration
- Go to **Project => Settings** and check it is Makefile based. 
- Go to **Project => Generate Code** then the code can be generated.


## Compile the Example

Now we can just compile the example generated by STM32CubeMX.

```
$ cd $PROJECT-ROOT (where the Makefile is)
$ make
```

## Get Eclipse (GNU-MCU-Eclipse and vrapper Packages)

To install minimum dev environment:

* Get Eclipse CDT Oxygen (as of 2018, eclipse-cpp-2018-09-linux-gtk-x86_64.tar.gz)
* Extract it and move it to somewhere you want.
* Run Eclipse in the directory.
* Once it is done, go help => Market Place.
* Search and install **GNU-MCU-Eclipse** and **Vrapper**

To set the tool chain and j-link's path:

* Run Eclipse
* Go to Window => Preferences => MCU => Global ARM Toolchains Paths
* Set the path to **/usr/bin**
* Go to Window => Preferences => MCU => Global SERGGER J-Link Paths
* Set the executable name to **JLinkGDBServerCLExe**
* Set the path to **/opt/SEGGER/JLink**

## Import the Example to Eclipse

To import the example project we made:

* File => New => Project 
* In the Wizard's first screen, C/C++ => Makefile project with existing file
* In the Wizard's second screen, browse the project location and select ARM GCC.

## Set Build Option and Parser Configuration

Build Option should be changed:

* Go to Project => Properties => C/C++ Build => Builder Settings => Build Command
* Edit it as the image \(make VERBOSE=1\)

![](/assets/20180201a.png)

Parser Option should be changed:

* Go to Project => Properties => C/C++ General => Preprocessor Include Pathes, Macros etc.
* Go to Providers tap => Click CDT GCC Build Output Parser Item
* Change Compiler command pattern

![](/assets/20180201b.png)

![](/assets/20180201c.png)

Compiler Option should be changed:

* Go to Project => Properties => C/C++ General => Preprocessor Include Pathes, Macros etc.
* Go to Providers tap => Click CDT ARM Cross GCC Build-in Compiler Settings
* Change Command to get compiler specs

![](/assets/20180201d.png)

![](/assets/20180201e.png) 

Everything is done!

Go to Project => Clean. That will clean and build the project.

---

## Set Debug Configuration in Eclipse

Finally it is time to try debugging.

* Go to Run => Debug Configurations
* Right click **GDB SEGGER J-Link Debugging** and **New**
* Click the second tap "Debugger"
* Actual executable for the server should be **"JLinkGDBServerCLExe"**
* Device name should be **"STM32F446RE"** (The screenshot below shows one of my old configuration)
* Executable for the client should be **"arm-none-eabi-gdb"**
* Click Apply and Debug

![](/assets/20180201f.png)

## Tips for Eclipse  

Sometimes C/C++ Perspective doesn't show Debug Launch button on its tap.
* Go to Window => Perspective => Customize Perspective => Tool Bar Visibility
* Activate Debug Item in the list.

  
If Eclipse's syntax check is too annoying for you, go to:
- Project Property >> C/C++ General >> Code Analasys
- And disable the options.
  
If Eclipse's formatter annoys,
- Go to Window >> Preference >> C/C++ >> Editor >> Save actions
- Disable Format source code.  
  
If you don't want to build every time you save,
- Go to Project
- Disable Build Automatically

## Tips for Windows Users
  
Almost same things can be done to have the same environment except:
- System Environment and the **Path** should have ARM-GCC's location (C:\Program Files (x86)\GNU Tools ARM Embedded\VERSION_CAN_DIFF\bin)
- System Environment and the **Path** should have JLink's location (C:\Program Files (x86)\SEGGER\JLink_VERSION_CAN_DIFF)
- System Environment and the **Path** should have MSYS2's location (C:\msys64\usr\bin) 
- System Environment and the **MSYS2_PATH_TYPE** should have its value as **inherit**  

Also in the MSYS2 terminal, enter these to get make
```
$ pacman -Syuu 
$ pacman -S tar git tree vim base-devel
```

----

## Conclusion

So far we followed this short manual to set up nRF development environment in Linux.

In the manual,

* Downloaded the compiler, IDE, and SDK
* Learned how to make your own project by scratching
* Compiled and debugged the project.

As software changes everyday, the setup can be changed as well. However the basic step might be not very different.

