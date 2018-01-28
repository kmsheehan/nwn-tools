# NWN-Tools

The nwn-tools here have been adapted from the existing tools with updates
to support Neverwinter Nights Enhanced Edition (NWN EE).  Currently this
project has two tools:

* nwnnss - NWN Script compiler / de-compiler / extractor
* nwnmdl - NWN Model compiler / de-compiler / extractor

These tools are based on the nwn-tools project https://github.com/niv/nwn-tools

## Changes

1. Fixed compilation issues to ensure the tools build universally on Windows,
MacOS, and Linux.
2. Replaced the command line argument processor (previously hand written)
with docopt.cpp to simplify argument processing and standardize the arguments
across all platforms.
3. Added support for NWN EE including new command line arguments as needed.
The default for the tools is NWN EE.
4. Renamed nwnnsscomp to nwnnss.  The nwnnsscomp tool was more than a compiler
as it can decompile, extract, and perform other functions on scripts.
5. Renamed nwnnssmdl to nwnnss.  The nwnnssmdl tool was more than a compiler
as it can decompile, extract, and perform other functions on models.

## Usage

Prebuilt binaries are available in the Neverwinter Vault for all platforms:

* [NWN Enhanced Edition Script Compiler (nwnnss)](https://neverwintervault.org/project/nwn1/other/tool/nwn-enhanced-edition-script-compiler-nwnnss)

### Installation

* Unpack the binaries from the zip file and place them somewhere in your path.
* Check the following requirements
    * Windows - This has been tested on Windows 7 and Windows 10.  Please report
    issues.
    * MacOs - This has been tested on MacOs 10.10 and 10.13.  Please report any issues.
    * Linux (may need support to run 32 bit binaries and libstdc++ 4.9 or newer:
        * Debian / Ubuntu:
          ```
          dpkg --add-architecture i386
          apt-get update
          apt-get install libstdc++6:i386
          ```
        * Fedora (latest):
          ```
          yum install libstdc++.i686
          ```
        * OpenSuse:
          ```
          zypper install compat-32bit
          zypper install libstdc++6-32bit
          ```
        * CentOS / Red Hat: TBD

### Using nwnnss

#### Usage message
```
nwnnss
    Usage:
      nwnnss (compile | t1 | t2 | t3 | t4) [cgloqrs] [-x <nwnversion>] [-u <nwnuserdir>] [-p <nwndir> | -k <incdir>] [<file>...]
      nwnnss (decompile | extract) [qr][-x <nwnversion>] [-p <nwndir> | -k <incdir>] [-u <nwnuserdir>] <file>...
      nwnnss (-h | --help)
      nwnnss --version
    Options:
        -c              Enable CPP support
        -g              Produce ndb file
        -l              List constant, struct, and function symbols
        -o              Enable optimizer
        -q              Silence most messages
        -r              Report basic status even when quiet
        -s              Print symbol count
        -p nwndir       NWN install Dir or set NWNDIR in env
        -u nwnuserdir   NWN User Dir or set NWNUSERDIR in env
        -k incdir       Unpacked NWN script location
        -x nwnversion   Set NWN version for compiler [default: 1.74]
        -h --help       Show this screen
        --version       Show version
```
#### Compile

* Options -g, -l, -o, -q, -r, -s control various compiler settings as defined above
* Option -x controls the version of the compiler.  Currently this primarily configures
where the compiler looks for the game files.  The default when not present is 1.74.
Passing anything less than 1.74 will force the compiler to look for NWN 1.69 if paths
to the NWN installation are not provided.
* Option -p nwndir - This is REQUIRED for NWN EE since there is no default path for the
installation.  It is optional on Windows when NWN 1.69 is used.
    * Note: On Windows command line arguments that have spaces are problematic
    especially in powershell.  Use the Command Prompt and quote the path or
     use the Windows 'short' path for the "Beamdog Library" - "Beamdo~1"
* Option -u nwnuserdir - This is used only with NWN EE since the user content
is a separate path from the installation.
      * Note: On Windows command line arguments that have spaces are problematic
      especially in powershell.  Use the Command Prompt and quote the path or
       use the Windows 'short' path for the "Neverwinter Nights" - "Neverw~1"
* Option -k incdir - As an alternative to provding the paths to the NWN installation
a path to an foldr can be provided where all of the game scripts are extracted from
the .bif files are located.
    * Note: the include dir can either be a flat directory with all the files
extracted in the correct order from oldest to newest (so later scripts overwrite
earlier scripts) or it may contain a subdirectory for each set of scripts.
        * For 1.69 These must be named base, xp1, xp1patch, xp2, xp2patch, xp3
        * For 1.74 These must be named base_scripts
* Option -c was added to support the Community Patch Project (CPP).  Currently the
CPP is only supported for 1.69 (not NWN EE) and requires extracting the game scripts
as specified using 'Option -k' above.  The CPP scripts should be placed in a folder
under the incdir named xpcpp.

##### Examples
* Compile a folder of scripts for NWN EE without extracting game files
  ```
  nwnnss compile -p /Applications/Beamdog/00829 -u "/Users/username/Documents/Neverwinter Nights/" *.nss
  ```
* Compile a folder of scripts for NWN EE using extracted game files in a flat folder
  ```
  nwnnss compile -k ../../tmp/bif/base_scripts.bif *.nss
  ```

#### Decompile (TODO) - Needs documentation

#### Extract (TODO) - Needs documentation

### Using nwnmdl (TODO) - Not tested and command line parser not updated

## Building

* Clone the repository at https://github.com/kmsheehan/nwn-tools using git
* Windows using VisualStudio 17
    * Install WinFlexBison3
        * Option 1: Down load and install manually from https://sourceforge.net/projects/winflexbison/
        * Option 2: Chocolately (Package manager for Windows) https://chocolatey.org
          ```
          choco install winflexbison3
          ```
    * Open the folder where the git repository was cloned File->Open->Folder
    * Build all with CMake->Build All
    * Binaries will be located in your <home folder>\CMakeBuilds\<guid>\build
* Linux (Debian / Ubuntu)
    * Install the 32 bit c++ compiler to support C++11
        * Install the 32 bit versions of the following:
          ```
          apt-get install g++-multilib build-essential cmake gperf git gcc-6 g++-6 bison gdb
          ```
    * Execute cmake
      ```
      cmake .
      ```
    * Execute make
      ```
      make all
      ```
*
