# TMS570LS3137-HDK + CLion integration

This LED blinky example demonstrates simple LED blinking application intergated with CLion environment using TI CGT ARM
compiler.

## Creating project

This project was created using HALCoGen for TI CGT ARM toolchain, however it is possible to create all project files
manually. To create project with HALCoGen simply follow the [instructions - point 3 + code generation](https://www.ti.com/lit/an/spna121b/spna121b.pdf).
Then use existing ```CMakeLists.txt``` as a template.

## Creating flash configuration

Flash configuration can be created using [Uniflash](https://www.ti.com/tool/UNIFLASH) following the steps below:
* open Uniflash
* specify MCU and debugger (TMS570LS3137 and JLink used here) 
* go to **Standalone Command Line** tab
* press **Generate Package**
* wait for the process to complete and save created package at known location
* extract package, rename it to ```flasher``` and copy whole directory inside project directory

## CLion project settings

For proper building and debugging, both toolchain and custom compiler have to be set in CLion settings:
* open **Preferences**
* navigate to **Build, Execution, Deployment** -> **Toolchains**
* add new toolchain and name it ```TI-ARM```
* in ```C Compiler``` and ```C++ Compiler``` fields specify absolute path to the ```armcl``` binary (it is inside
TI CGT ARM toolchain installation directory)
* apply changes
* expand **Toolchains** and move to **Custom Compiler**
* check **Use custom compiler config** checkbox and provide path to ```TI-ARM-CGT-compiler.yaml``` file
* apply changes, close and reload project

## Flashing and debugging

* select **Edit Configurations...** from dropdown in upper-right corner
* add new **OpenOCD Download & Run** configuration and name it ```Debug```
* select .out file as **Executable**
* select provided ```ti_tmdx570ls31usb.cfg``` file as **Board config file** (valid for JLink debugger)
* check **None** in **Download**
* apply changes
* in **Before launch** section add new action after **Build** - **Run External Tool**
* add new external tool, name it **Flash**
* in **Program** field type path to DSLite executable in ```flasher``` directory, e.g. ```$ProjectFileDir$/flasher/ccs_base/DebugServer/bin/DSLite```
* in **Arguments** field type ```flash -c $ProjectFileDir$/flasher/user_files/configs/tms570ls3137.ccxml -e -f -v $CMakeCurrentProductFile$```
* in **Working directory** field type ```$ProjectFileDir$```
* apply all changes

Happy debugging!