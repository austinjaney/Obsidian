# R710 Server Build Checklist
#sysadmin #dell #server 

This template is designed to aid the server build out process for Dell PowerEdge Servers.

### BIOS and Firmware Upgrade CD R710
[Bootable ISO for R710 Firmware](https://dell.app.box.com/v/bootableR710)

### If its going to be a Hyper-v Host edit the following in the bios
1. Disable C-states      - Disabled
2. Disable C1E             - Disabled
3. Execute Disabled    - Disabled

### Firmware for Lifecycle Controller and iDRAC
[Lifecycle Controller 1.7.5](https://www.dell.com/support/home/us/en/04/drivers/driversdetails?driverid=0wfgm&oscode=naa&productcode=poweredge-r710)
[Lifecycle Controller 1.7.5 Repair Kit](https://www.dell.com/support/home/us/en/04/drivers/driversdetails?driverid=80xj1&oscode=naa&productcode=poweredge-r710)
[iDrac 6 Firmware Upgrade](https://www.dell.com/support/home/us/en/04/drivers/driversdetails?driverid=kpccc&oscode=naa&productcode=poweredge-r710)

### OMSA
[Open Manage Server Administrator for Windows](https://www.dell.com/support/home/us/en/04/drivers/driversdetails?driverid=gyp4r&oscode=ws8r2&productcode=poweredge-r710)

/Server Information/
Hostname:
OS:
Installed Memory:
Raid Version:
Drives Used:
Completed By:

/When Building a server to be placed in "Inventory”/
- Tape Build Sheet to Server
- Take Server to IT trailer for storage

/When a Server is in repair/
> Tape Build Sheet to Server  
> Make sure to communicate the status to your team.  

- Fresh Thermal Paste (If its a new refurbished unit)
- Install Hardware to the desired system configuration
- Check BIOS Clock Time
- Upgrade Bios and firmware using dells CD
- Run onboard diagnostics
- Install Windows and Updates
- APC PowerChute configuration or other UPS Settings
- Name Ethernet Ports 1-4
- Enable Remote Desktop
- Update Firmware for Lifecycle Controller and iDRAC
- Set Hostname and Join to ncc.lan
- Move Server to the Servers OU
- Run Gpupdate /force
- Label Hardware with Hostname
- Add to OME
- Antivirus Install
- System Center Agent Installed
- Windows Activation

## Misc information about r710’s
PERC H700
https://www.dell.com/downloads/global/products/pvaul/en/perc-technical-guidebook.pdf
