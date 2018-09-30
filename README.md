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
* Get JRE (Oracle's)
* Get STM32CubeMX
* Generate an Example
* Compile the Example
* Get Eclipse (GNU-MCU-Eclipse and vrapper Packages)
* Import the Example to Eclipse
* Set Build Option and Parser Configuration
* Set Debug Configuration in Eclipse
* Set Debug Launcher in C/C++ Perspective
* Conclusion

---

### Get Basic Tools

```
$ sudo apt update
$ sudo apt install build-essential
$ sudo apt install eclipse
$ sudo apt install git
```

### Get ARM-GCC Compiler

```
$ sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
$ sudo apt update
$ sudo apt install gcc-arm-embedded
$ arm-none-eabi-gcc -v
```

### Get JLink Package

Download **J-Link Software and Documentation pack for Linux, DEB Installer, 64-bit** from [https://www.segger.com/downloads/jlink/\#J-LinkSoftwareAndDocumentationPack](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack)

Currently the version is 6.30a

```
$ sudo dpkg -i JLink_Linux_V630_x86_64.deb
```

To test it, connect your eval board or JLink to your PC and...

```
$ JLinkExe -device STM32F446RE -speed 1000 -if swd

// See the result and exit.

> exit
```

## Get JRE

## Get STM32CubeMX

## Generate an Example

## Compile the Example

Now we can just compile an example.

```
$ cd $PROJECT-ROOT
$ make
```

## Get Eclipse (GNU-MCU-Eclipse and vrapper Packages)

To install minimum dev environment:

* Get Eclipse Neon 3 \(Not Oxygen or Luna\).
* Extract it and run Eclipse\_Inst
* Once it is done, go help &gt; Market Place.
* Search and install MCU and vrapper

To set the tool chain and j-link's path:

* Run Eclipse
* Go to Window &gt; Preferences &gt; MCU &gt; Global ARM Toolchains Paths
* Set the path to **/usr/bin**
* Go to Window &gt; Preferences &gt; MCU &gt; Global SERGGER J-Link Paths
* Set the executable name to **JLinkGDBServerCLExe**
* Set the path to **/opt/SEGGER/JLink**

## Import the Example to Eclipse

To import the example project we made:

* File &gt; New &gt; Project 
* In the Wizard's first screen, C/C++ &gt; Makefile project with existing file
* In the Wizard's second screen, browse the project location and select ARM GCC.

## Set Build Option and Parser Configuration

Build Option should be changed:

* Go to Project &gt; Properties &gt; C/C++ Build &gt; Builder Settings &gt; Build Command
* Edit it as the image \(make VERBOSE=1\)

![](/assets/20180201a.png)

Parser Option should be changed:

* Go to Project &gt; Properties &gt; C/C++ General &gt; Preprocessor Include Pathes, Macros etc.
* Go to Providers tap &gt; Click CDT GCC Build Output Parser Item
* Change Compiler command pattern

![](/assets/20180201b.png)

![](/assets/20180201c.png)

Compiler Option should be changed:

* Go to Project &gt; Properties &gt; C/C++ General &gt; Preprocessor Include Pathes, Macros etc.
* Go to Providers tap &gt; Click CDT ARM Cross GCC Build-in Compiler Settings
* Change Command to get compiler specs

![](/assets/20180201d.png)

![](/assets/20180201e.png) 

Everything is done!

Go to Project &gt; Clean. That will clean and build the project.

---

## Set Debug Configuration in Eclipse

Finally it is time to try debugging.

* Go to Run &gt; Debug Configurations
* Right click **GDB SEGGER J-Link Debugging** and **New**
* Click the second tap "Debugger"
* Actual executable for the server should be **"JLinkGDBServerCLExe"**
* Device name should be **"STM32F446RE"** (The screenshot below shows one of my old configuration)
* Executable for the client should be **"arm-none-eabi-gdb"**
* Click Apply and Debug

![](/assets/20180201f.png)

## Set Debug Launcher in C/C++ Perspective

Sometimes C/C++ Perspective doesn't show Debug Launch button on its tap.

* Go to Window &gt; Perspective &gt; Customize Perspective &gt; Tool Bar Visibility
* Activate Debug Item in the list.

----

## Conclusion

So far we followed this short manual to set up nRF development environment in Linux.

In the manual,

* Downloaded the compiler, IDE, and SDK
* Learned how to make your own project by scratching
* Compiled and debugged the project.

As software changes everyday, the setup can be changed as well. However the basic step might be not very different.

