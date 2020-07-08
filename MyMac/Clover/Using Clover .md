# Using Clover 							 						



https://www.tonymacx86.com/threads/how-to-install-os-x-yosemite-using-clover.144426/





\17. Hit **Customize** and install Clover to the same USB with the following **Custom Install** settings:

[![clover-1.png](Using%20Clover%20.assets/79143-a3e18d156eae359df45da59a1ba63a79.jpg)](https://www.tonymacx86.com/attachments/clover-1-png.107993/)[![clover-2.png](Using%20Clover%20.assets/79141-78a1424146f26565e7ad478fc1fcc3e4.jpg)](https://www.tonymacx86.com/attachments/clover-2-png.107991/)[![clover-4.png](Using%20Clover%20.assets/79139-2f486be8de2ffcf247f7b16230b9f5bb.jpg)](https://www.tonymacx86.com/attachments/clover-4-png.107989/)[![clover-3.png](Using%20Clover%20.assets/79140-f894c4413ff69ebde80fd0e99d644871.jpg)](https://www.tonymacx86.com/attachments/clover-3-png.107990/)[![clover-5.png](Using%20Clover%20.assets/79138-665ca9ba956f73df0dd877a30791c48c.jpg)](https://www.tonymacx86.com/attachments/clover-5-png.107988/)

**NOTE:** There are a few different ways to setup Clover on the USB. Using Legacy Mode  is the simplest and most reliable for 5, 6, 7, 8, and 9 Series Gigabyte  motherboards with standard default BIOS or UEFI settings. For all other  UEFI-based systems such as ASUS 7, 8, and 9 Series motherboards, use  UEFI Boot Mode to install to the EFI partition of the USB for UEFI  booting only.

 A few more changes need to be made to the default Clover installation:


​

\20. Navigate to **/EFI/CLOVER/** and replace default **config.plist** with attached **config.plist\*
** 21. Navigate to **/EFI/CLOVER/kexts/** and create a folder called **10.10**
 \22. Navigate to **/EFI/CLOVER/kexts/10.10/** and add **[FakeSMC.kext](http://www.tonymacx86.com/downloads.php?do=cat&id=11)
** 23. Navigate to **/EFI/CLOVER/kexts/10.10/** and add **[your ethernet kext](http://www.tonymacx86.com/downloads.php?do=cat&id=11)**
 \24. Navigate to **/EFI/CLOVER/kexts/10.10/** and add **[NullCPUPowerManagement.kext](http://www.tonymacx86.com/downloads.php?do=cat&id=11)**
 \25. Navigate to **/EFI/CLOVER/drivers64UEFI/** remove **VBoxHfs-64.efi** and add **[HFSPlus.efi](https://github.com/JrCs/CloverGrowerPro/blob/master/Files/HFSPlus/X64/HFSPlus.efi?raw=true)**
 \26. (Optional) Navigate to **/EFI/CLOVER/ACPI/patched/** and add **DSDT.aml** and **SSDT.aml 
** 
 *See attached **config.plist** for a working minimal configuration. 

**NOTE:** There are a few different ways to setup Clover on the USB. Using Legacy Mode  is the simplest and most reliable for 5, 6, 7, 8, and 9 Series Gigabyte  motherboards with standard default BIOS or UEFI settings. For all other  UEFI-based systems such as ASUS 7, 8, and 9 Series motherboards, use  UEFI Boot Mode to install to the EFI partition of the USB for UEFI  booting only.

 A few more changes need to be made to the default Clover installation:


​

\20. Navigate to **/EFI/CLOVER/** and replace default **config.plist** with attached **config.plist\*
** 21. Navigate to **/EFI/CLOVER/kexts/** and create a folder called **10.10**
 \22. Navigate to **/EFI/CLOVER/kexts/10.10/** and add **[FakeSMC.kext](http://www.tonymacx86.com/downloads.php?do=cat&id=11)
** 23. Navigate to **/EFI/CLOVER/kexts/10.10/** and add **[your ethernet kext](http://www.tonymacx86.com/downloads.php?do=cat&id=11)**
 \24. Navigate to **/EFI/CLOVER/kexts/10.10/** and add **[NullCPUPowerManagement.kext](http://www.tonymacx86.com/downloads.php?do=cat&id=11)**
 \25. Navigate to **/EFI/CLOVER/drivers64UEFI/** remove **VBoxHfs-64.efi** and add **[HFSPlus.efi](https://github.com/JrCs/CloverGrowerPro/blob/master/Files/HFSPlus/X64/HFSPlus.efi?raw=true)**
 \26. (Optional) Navigate to **/EFI/CLOVER/ACPI/patched/** and add **DSDT.aml** and **SSDT.aml 
** 
 *See attached **config.plist** for a working minimal configuration. 