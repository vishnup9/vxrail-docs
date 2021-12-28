**VxRail Manager Initialization**

- Table of Contents {:toc}

**Changelog**

|**Version**|**Date**|**Description**|**Author**|
| :-: | :-: | :-: | :-: |
|0.1|12/28/2021|First version|Rohit Singh|
**Introduction**

**Purpose**

The purpose of this document is to describe manual steps that should be performed to initialize VxRail Manager. 

**Scope**

The scope of this document covers the following:

1. VxRail Manager initial configuration

**Imaging/Resetting to factory settings the VxRail nodes**

If it's not already done, all VxRail hosts have to be imaging or resetting to factory settings by using Dell EMC RASR (Rapid Appliance Self Recovery) process.

In the document below, we have detailed steps mentioned to perform the factory reset.



**VxRail Manager initializing procedure**


This procedure describes the steps to upgrade the RASR image on VxRail Dell appliance (node) to the VxRail 7.0.241 release version. This procedure requires the node to be power-cycled. If this RASR image upgrade is run on a currently running cluster, the cluster might be impacted. This procedure contains the following steps:

1. Upgrade the RASR image on the node using ISO. 

2. Install the Dell Upgrade Packages (DUPs) from SD. 

3. Factory Reset from SD.




### **Upgrade the RASR image on the node using ISO**



|Sub-Step|Action|Screenshot|
| :- | :- | :- |
|**1.**|In a Windows client, open iDRAC web UI and launch virtual console.|<p></p><p></p><p></p>|
|**2.**|In the Virtual Console window, click Virtual Media tab and select Connect Virtual Media.|<p></p><p></p><p></p><p></p><p></p>|
|**3.**|<p></p><p>Click Next Boot and select Virtual CD/DVD/ISO</p>|<p></p><p></p><p></p>|
|**4.**|<p>Click Power and select Reset System (warm boot).</p><p>The system will boot to the RASR Update Utility menu.</p>|<p></p><p></p><p></p><p></p>|
|**5.**|<p></p><p>Type R to select RASR Reset, and then type Y to continue.</p>|<p></p><p></p><p></p><p></p><p></p><p></p><p></p>|
|**6.**|<p></p><p>The RASR Reset process begins. Monitor the progress until completion. This portion of the process will take approximately 30 minutes.</p>|<p></p><p></p><p></p><p></p><p></p><p></p><p></p>|
### **Install the Dell Upgrade Packages (DUPs) from SD.**



|Sub-Step|Action|Screenshot|
| :- | :- | :- |
|1|After completing RASR reset through ISO, disconnect virtual media by selecting Virtual Media then Disconnect Virtual Media.||
|2|From the RASR Main menu, Type Q to quit and Y to reboot.||
|3|During boot, press F11 to enter Boot Manager.||
|4|Select One-shot UEFI Boot Menu.||
|5|Select Internal SD: RASRTOOL.||
|6|System will reboot to start RASR.||
|7|At the VxRail RASR Menu, type 99 then press Enter. The VxRail RASR Support Menu will open||
|8|<p>Type I to select Install (DUP(s) and press Enter.</p><p></p><p>The system will be scanned to determine compatible DUPs. See next screen. Firmware version and list might be different per platform.</p>||
|9|Type U to select Upgrade from the Support menu and press Enter.||
|10|<p>Type R to select Rolling DUP(s) Upgrade and press Enter.</p><p></p><p>The DUP upgrades will begin. The system will automatically reboot after each DUP is installed.</p><p></p><p>The DUP upgrades will begin. The system will automatically reboot after each DUP is installed.</p>||


### **Factory Reset from SD**


|Sub-Step|Action|Screenshot|
| :- | :- | :- |
|1.|<p>The system will boot to the RASR Update Utility menu.</p><p>Type F to do Factory Reset and type Y to continue</p><p></p>||
|**2**|Ensure all external storage devices are disconnected correctly to prevent the potential risks of data corruption. If yes, type Disconnected to continue. Press any key to exit Factory Reset.||
|**3**|Factory Reset completes and displays message “Factory install Prototype (FIP) successfully completed”. Press Enter to continue and return to RASR Update Utility menu.||
|**5**|Enter Q to quit and Yes to reboot the system||
|**6**|Once installation is completed completed message will pop post reboot as shown in the image.||


### **VxRail Manager initial configuration**


|Sub-Step|Action|Screenshot|
| :- | :- | :- |
|**1.**|In a Windows client, open iDRAC web UI and launch virtual console.|<p></p><p></p><p></p>|
|**2.**|<p>Run Below commands on master server gre02mgt001</p><p>esxcli network vswitch standard portgroup list</p><p>esxcli network vswitch standard portgroup set –p "Management Network" –v "2800"<br>esxcli network vswitch standard portgroup set –p "VM Network" –v "2800"</p><p>esxcli network vswitch standard portgroup set –p "Private Management Network" –v "2899"<br>esxcli network vswitch standard portgroup set –p "Privae VM Network" –v "2899"</p><p></p><p>vxrail-primary --setup --vxrail-address 172.22.128.14 --vxrail-netmask 255.255.255.0 --vxrail-gateway 172.22.128.1 --no-roll-back --verbose</p><p></p><p>vim-cmd vmsvc/getallvms<br>vim-cmd vmsvc/power.shutdown "1"<br>vim-cmd vmsvc/power.getstate "1"<br>vim-cmd vmsvc/power.on "1"</p>|<p></p>|
|**3.**|<p>For **gre02mgt002, gre02mgt003, gre02mgt004**</p><p>esxcli network vswitch standard portgroup list</p><p>esxcli network vswitch standard portgroup set –p "Management Network" –v "2800"<br>esxcli network vswitch standard portgroup set –p "VM Network" –v "2800"</p><p>esxcli network vswitch standard portgroup set –p "Private Management Network" –v "2899"<br>esxcli network vswitch standard portgroup set –p "Private VM Network" –v "2899"</p><p></p>||

