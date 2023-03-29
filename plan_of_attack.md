
Hardware estimated to arrive 3/30.  Getting ahead of the ball by setting up environment and ensuring standardized parts of the code compile and build.
## Hardware
BGM220 explorer kit
   BGM220PC22HNA module with a Cortex-M33 core with 512Kb flash and 32Kb SRAM.


## Incremental approach to development

Create a project as specified below.  Ensure that:
 - project builds
 - expected files are generated
 - GATT configuration looks correct.
 - Tools for generating BGL, CRC checks and merges all work.
        
Flash initial project when hardware arrives. Check that...
 - BT advertisement and appropriate characteristics are visible in the Gecko iOS app.
 - resetting the board without a payload in the "download area" works fine
 - upload functionality with bad GBL file fails to copy download into the application area.
 - upload functionality with good GBL file copies download into application area and changes are reflected upon reset

The documentation and examples at the SI Labs website are somewhat scattered, and not all of the sources are applicable to the latest GSDK and latest Simplicity IDE.  Thus, it would be a good idea to create a concrete plan of attack that can then be modified as new information or further knowledge comes to light.

### Plan of attack to to create an SOC image(s) that implement(s) the given requirements, at a high level (use this outline to drill down)

 - create a bootloader project and build a gecko bootloader image with "multiple image OTA support"*
 - create an application image that implements the OTA functionality while running**
 - either merge the bootloader with the application image and flash the device in one go, or flash the bootloader and application images separately.
    
*The BGM220PC22HNA modules have the BGAPI UART DFU bootloader v1.10.2 loaded from the factory, so that's probably going to get erased.

** According to its README.md, the SoC- Empty example project can be be configured to include this functionality as a "software component".  Thus, it is possible that this is already implemented partially or fully by loading that example. 

### Questions

 1. How do we handle disconnects?  Are there events we can catch from the BT stack to alert us?  Assuming the uploader app can catch the disconnect, does the user start the process anew and the SOC starts the upload from the beginning?
 2. What happens if the process of copying the BGL into the application area gets interrupted by, say, a reset?  Presumably that means that whatever is in the application area is not usable
