=====================================================================

How to update I-NUCLEO-LRWAN Preloaded AT command Software

=====================================================================

!!! Important Note !!!

since the shield board with the preloaded firmware has Flash read protection, it is necessary to backup whole flash/eeprom before any flash programming, please read recover guide in the document below before any flash operations:  

https://github.com/USIWPModule/USI_I-NUCLEO-LRWAN1/blob/main/preloaded_firmware/WM-SG-SM-42%20recover%20guide.pdf




#1 download sm42_fw_update_tool from the link below, and upzip it on the Windows PC
   https://github.com/USIWPModule/USI_I-NUCLEO-LRWAN1/blob/main/preloaded_firmware/sm42_fw_update_tool_v1.1.zip
   
#2 download the the preloaded firmware (.hex) which you want to programming to the device from the links below:
   
   * download link for version 2.8:
   
      https://github.com/USIWPModule/USI_I-NUCLEO-LRWAN1/blob/main/preloaded_firmware/wm-sg-sm-42_firmware_v2.8.hex
   
   * download link for version 2.9:
   
      https://github.com/USIWPModule/USI_I-NUCLEO-LRWAN1/blob/main/preloaded_firmware/wm-sg-sm-42_firmware_v2.9.hex
      
#3 download the update instructions from the link below, and follow it to reprogramming the preloaded firmware in the device.

   https://github.com/USIWPModule/USI_I-NUCLEO-LRWAN1/blob/main/preloaded_firmware/WM-SG-SM-42%20Update%20Preloaded%20AT%20Command%20FW%20Application%20Note%20rev.%201.2.pdf

