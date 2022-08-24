# monitor-power-savings-EDID_OVERRIDE
turn off monitor power savings in EDID using EDID_OVERRIDE. DPMS (VESA Display Power Management Signaling) "DPMS standby supported" "DPMS suspend supported" "DPMS active-off supported"


## OVERVIEW
EDID (extended display identification data) is what the monitor sends to your computer. the computer stores this information in a registry. the information describes what the monitor is capable of. (refresh rate, timings, color, and there are 3 bits which contain power saving information). this guide is to edit those 3 bits using EDID_OVERRIDE.

## RELEVANT LINKS
- [EDID wiki](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data#EDID_1.4_data_format) (ctrl+f DPMS)
- [windows edid documentation](https://docs.microsoft.com/en-us/windows-hardware/drivers/display/overriding-monitor-edids#updating-an-edid) (contains example of the edid registry location)  
- [toasty cru](https://www.monitortests.com/forum/Thread-Custom-Resolution-Utility-CRU) (edit edid and create an edid override).  
- [other edid editors](https://www.monitortests.com/blog/list-of-edid-editors/) (for example aw edid can check the power savings)  

## INSTRUCTIONS
0. check [aw edid](https://www.analogway.com/americas/products/software-tools/aw-edid-editor/) to see your current power saving options
1. change anything using [toasty cru](https://www.monitortests.com/forum/Thread-Custom-Resolution-Utility-CRU) (edit edid and create an edid override).
2. find the EDID_OVERRIDE folder located in   "Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\DISPLAY\"
3. open the REG_BINARY 0 
4. check the hex value following the line marked 18 (18 in hex is 24 in decimal, this location matches the documentation in wiki)
5. convert your HEX value at that location to binary  
use your calculator in programming mode (win+r, calc, programmer mode (or press alt+3))
6. check the 3 leading bits (for example my VG248qe has EA(hex)=1110 1010(binary) so mine has 111 as the first 3 which has all the power saving features on.
7. change the binary to 0 on all first 3 then convert back to hex (in my case 1110 1010 would need to be changed to 0000 1010(binary)=0A(hex))
8. you will change 2 registry entries, the value at 18 and the last value in the EDID (all hex values are added together including the last one to make sure the sum ends in 00 hex, so you need to change the last value to get the checksum right)
9. change your registry in the EDID location at 18(hex)=24(decimal) value to the new one you created (in my case i will change EA to 0A)
10. change the very last byte in the EDID registry by incrementing it the same amount you decreased the value in 18(hex)=24(decimal)  
in my example the last byte (last 2 digits in line 78(hex)) was BA. i change it to 9A (BA+E0=19A in hex, which is 9A with the relevant checksum last 2 digits) check the wiki page on the 127th byte in EDID for explanation of the checksum (all the bytes must add up ending in 00 so if u change one u have to change the checksum)
11. check [aw edid](https://www.analogway.com/americas/products/software-tools/aw-edid-editor/) to see if you did it correctly, the power savings should be off now
12. click restart64.exe in cru folder.
13. if something goes wrong press f8 (which is recovery mode in the window that pops up after u use restart64)
