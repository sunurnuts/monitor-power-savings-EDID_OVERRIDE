# monitor-power-savings-EDID_OVERRIDE
turn off monitor power savings in EDID using EDID_OVERRIDE.  
DPMS (VESA Display Power Management Signaling)  
"DPMS standby supported" "DPMS suspend supported" "DPMS active-off supported"


## OVERVIEW
EDID (extended display identification data) is what the monitor sends to your computer.  
the computer stores this information in a registry.  
the information describes what the monitor is capable of.   
(refresh rate, timings, color, and there are 3 bits which contain power saving information).  
this guide is to edit those 3 bits using EDID_OVERRIDE.

## RELEVANT LINKS
- [EDID wiki](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data#EDID_1.4_data_format) (ctrl+f DPMS)
- [windows edid documentation](https://docs.microsoft.com/en-us/windows-hardware/drivers/display/overriding-monitor-edids#updating-an-edid) (contains example of the edid registry location)  
- [toasty cru](https://www.monitortests.com/forum/Thread-Custom-Resolution-Utility-CRU) (edit edid and create an edid override).  
- [other edid editors](https://www.monitortests.com/blog/list-of-edid-editors/) (for example aw edid can check the power savings)  

## INSTRUCTIONS
1. change anything using [toasty cru](https://www.monitortests.com/forum/Thread-Custom-Resolution-Utility-CRU) (edit edid and create an edid override). then press restart64.exe if something goes wrong press f8 for recovery mode.
  ![a](https://github.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/blob/main/cru%20changes.png?raw=true)
2. check [aw edid](https://www.analogway.com/americas/products/software-tools/aw-edid-editor/) to see your current power saving options (extract from registry)
  ![one](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/aw%20extract%20from%20registry.png)
  ![two](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/aw%20extract%20from%20registry%202.png)
  ![three](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/aw%20extract%20from%20registry%203.bmp)
3. find the EDID_OVERRIDE folder located in   "Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\DISPLAY\"
  ![e](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/edid%20override%20registry.png)
4. open the REG_BINARY 0 
5. check the hex value following the line marked 18 (18 in hex is 24 in decimal, this 24th BYTE location matches the documentation in [wiki](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data#EDID_1.4_data_format), ctrl+f DPMS in wiki it will show BIT 7,6,5 in BYTE 24. these 3 bits are what you want to change)
  ![f](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/edid%20edit%201.png)
6. convert your HEX value at that location to binary  
use your calculator in programming mode (win+r, calc, programmer mode (or press alt+3))
  ![g](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/calc%201.png)
7. check the 3 leading bits (for example my VG248qe has EA(hex)=1110 1010(binary) so mine has 111 as the first 3 which has all the power saving features on.
8. change the binary to 0 on all first 3 then convert back to hex (in my case 1110 1010 would need to be changed to 0000 1010(binary)=0A(hex))
  ![h](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/calc%202.png)
9. you will change 2 registry entries, the value at 18 and the last value in the EDID (all hex values are added together including the last one to make sure the sum ends in 00 hex, so you need to change the last value to get the checksum right)
10. change your registry in the EDID location at 18(hex)=24(decimal) value to the new one you created (in my case i will change EA to 0A)
  ![i](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/edid%20change%201.png)
11. change the very last byte in the EDID registry by incrementing it the same amount you decreased the value in 18(hex)=24(decimal)(in my case i reduced EA by E0. so i will have to add E0 to the checksum)  

  in my example the last byte (last 2 digits in line 78(hex)) was B9. i change it to 99 (B9+E0=199 in hex, which is 99 with the relevant checksum last 2 digits)
  ![j](https://github.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/blob/main/edid%20edit%202.png)
  ![k](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/edid%20edit%203.png)

  check the wiki page on the 127th byte in EDID for explanation of the checksum (all the bytes must add up ending in 00 so if u change one u have to change the checksum) 

12. click restart64.exe in cru folder. if something goes wrong, like you cannot see your screen, press f8 (which is recovery mode in the window that pops up after u use restart64, you will have to restart whole process)
13. make sure your actual EDID that is supposed to be overriden does not say BAD_EDID
  ![l](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/edid%20checksum.png)
14. check [aw edid](https://www.analogway.com/americas/products/software-tools/aw-edid-editor/) to see if you did it correctly, the power savings should be off now 
  ![m](https://raw.githubusercontent.com/sunurnuts/monitor-power-savings-EDID_OVERRIDE/main/aw%20extract%203.png)
