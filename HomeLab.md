# HomeLab
Description of my HomeLab/ Smart Home setup. 

# Contents
- Overview
- Gear
- Setup
- Servers
- To do list

# Overview
My Homelab consists of multiple repurposed copmuters (for now) and some upcycled parts salvaged from electronics around the home ( Mostly monitors and reflashed older cellphones. Each device and VM associated with the server uses will all be codenamed according to the following scheme for ease of distinction

In Addition I'm also including my Smarthome Setup in the listing since some of the functions and uses are actually tied in with the Lab uses as well and I feel it's just different enough to warrant mention. 

# Gear

- Quotom-Q575g6-s05 Mini Pc (Intel 7th generation i7 processor 16 gig of Ram/Kingston SUV500MS/ 240Gb SSD/ Kingston 120 SSD/ DualBand Wireless AC7260 pci Wifi/Bluetooth Adapter with Two Dual Band 7dbi 2.4Ghz/5ghz antennas/ Additional Twilio Backup Sim is also installed onboard but not currently active) "Site A"

- ASUS M5A97 R2.0 AM3+ AMD 970 SATA 6Gb/s USB 3.0 ATX AMD Motherboard (AmdFx 8350 processor 16gb Ram(expanding to 32 later)/ Samsung 512gb SSD, WD Black 2TB hard drive, 2.5" WD Blue 1Tb Drive( Planned migration to 6 WD IronWolf Green 4tb drives)/ AMD RX560 4gb Graphics Card/ AMD RX560 2gb Graphics Card 9 non-crossfire) "Site C" ( will be used as another Proxmox Node with Both Graphics Cards being in use for up to 2 seperate VM's for thin clients) 

- Asus AMD Zacate E350-m1-Pro- (Amd A5 processor, 10gb Ram, 6 WD 3tb Green Drives, AMD R7 4gb Graphics Card) "Site B" Will be used as a proxmox node but will mainly be used for Cloud Storage with NextCloud 

- Gigabyte F2A88XM-DH3 (Amd A-10 7890k Processor, 8gb Ram (expanding to 32gb later, Kingston 120gb SSD, WD Blue 1tb HDD,) Current use as a SteamOS remote pc and Emulation machine. 

- 10" lcd Touch panel hardwired to local alarm system for control

- 10 cameras. 5 cameras are modified from original firmware (WyzeCam mk2 and Wyzecam Pan)  to use RTSP and local SD storage for monitoring. 3 are made from upcycled devices from local laptops and single board computer cameras. Cameras piggy back off of IR light from visible cameras for nightvision. Remaining two cameras are Nest cameras. Cameras also use MotionEye Os as a means of using cameras as a motion detection device

- Control devices: 5 Google Nexus devices.

 - 3 2012 google Nexus 5 devices running modified OS's ( UBPorts). 
   Any google software was removed from the devices and only run UBPORTS Ubuntu Touch Mobile OS. 

  - 1 LG Nexus 5x. Device is Running MaruOs (Currently Okinawa Build) and had been modified to run microg and Kali Linux Nethunter on top of the Rom. Device can be used for controlling the home but main use is for another project and pentesting. With MaruOs this allows for phone to be used in "convergence" to allow for a full Debian Linux desktop when connected to a supported phone dock or casted from phone to a compatible display. 

  - 1 2012 LG Nexus 7 LTE Tablet. Tablet has been modified to run LineageOS and Microg. Once again No Google accounts are associated with the device and only limited applications are used to support the system. Also Doubles usage as local remote control.

- In home Alarm System with Self Monitoring using Alarm Decoder AD2USB appliance 

- 3 Motion sensors all hardwired to the house mains along with battery backup

- OBI202 voip box. Used as POTS line. Rewired Phone Line from ouside the house. replaces CAT3 line used for primary phone line to CAT5e and replaced all ports in house. Phone line is using Twilio For emergency monitoring and Google Voice for VOIP service.

- Aeotec Z-stick Gen5 For Zwave device control 

- HP Proliant Ml330 Generation 3 Server (4gigs) 

- 2 Wyse Terminals (Model Cx0 C30LE.Ce 128f/512r.DVI US)

- 10 raspberry pi 0w's ( 5 with GPIO pins, 5 Without )

- 2 Raspberry Pi 3b 

- 2 Raspberry pi 3b+

- 3 Routers Flashed with DDWRT 

- 2 Hp Compaq ddx2200 Microtower pc (2gb Ram, Pentium4 processor, 1 kingston 120gb SSD, 1 WD Blue 320gb HDD) 

- 3 Netgear GS608 8 port gigabit switch

- TP-Link TL-SG116E UnManaged Switch

# Setup 
Home Server( Proxmox Site A)

- Proxmox VE
- Size: (as of 02/20/2020) 256gb + 120gb Secondary drive 

  - System is backed protected against power loss in multiple steps. First step is main system is on a 19500Mwa backup battery designed to hold power for up to 6 hours.

  - Main VM for home services (Smarthome control, Music Streaming Server, DNS filtering, Alarm System) 
     
     Current VM's insalled:
     
Smarthome (Kerberos):

- Ubuntu Vm within Proxmox Running Docker and HomeAssistant Core 
- VM container is 50 gigs in size
Features: 
- Redundant backup automated to upload to two separate cloud accounts at varying times during the day, Cloud account holds 30 days worth of twice daily backups. The System itself holds 2 days of backups each for a total of 4 local daily snapshots saved to the system and 60 saved within cloud account.
- System is backed protected against power loss in multiple steps. First step is main system is on a 19500Mwa backup battery designed to hold power for up to 6 hours (See Proxmox VE entry.) Second step is a dedicated raspberry pi 3b+ ( Soon to be upgraded to a pi4 4 gig model)  attached to a battery backup rated to keep the device and an interface running for up to 10 hours along with a usb modem to ensure in case of a power loss that critical notifications from the alarm system still reach intended persons. This is done VIA SMS and the device is hardwired to home power to charge automatically. Dedicated camera and reed switch sensor monitor for any tampering with where devices are stored. 
- Third Backup being developed for Offsite physical storage
- System consists of Vm image in combination with multiple sensors around the house for information. Much of the information is not available outside of local network and consists of several motion, light, infrared, pressure sensors along with several cameras made from local sourced materials and open source software to manage the system. The idea is to rely on as little commercial devices as possible or at least limit as much information as possible the data that is being sent from the devices. Several devices have been modified either physically or by firmware to accomplish this. 
- Local Wireguard Tunnel and backup Connection to system via Tor Hidden Service token allow for remote secure connection to home if web interface in unavailable because default remote connection is down. 
- In Case Network access is comepletely down System can still send and respond to actionable notifications from Dedicated SIM 
 -Uses:
  -  Smart Autmation Control of every light in the house
  -  System also controls Security and Safety Systems within the house including Locks, Smoke/CO Alarm, Home Alarm systems, Thermostat
  - Monitors occupancy within the house via a combination of Motion Sensors, Occupancy Sensors and monitoring electronic devices by MAC ID   and Wifi Connection from NMAP and local bluetooth radios. 
  - Monitoring Network Traffic and home ISP connection speed 
  - Monitoring SIP home telephony connection and calls from OBIHAI device
  - Temperature monitoring
  - Vitals monitoring ( Via Google Fit API)
  - Calendar
  - Cryptocurrency Monitoring ( Crypto Prices from Bitcoin, Etherium, Litecoin, and dogecoin via CoinMarketCap)
  - Weather forecast with Full local Radar conditions and Allergy Alerts, 
  - Nasa Launch Schedule and ISS (international Space station) tracking 
  - Selfhosted Bitwarden instance
  - Self Hosted Matrix Node and IRC Chatroom 
  - RTSP camera Monitoring Via MotionEye Os integration 
  - Local PiHole Instance 
  - Emergency Notifications Via Email to SMS and dedicated Twilio account. 
  - Non essential Notifications sent by push notification on local control devices.
  - Environmental Sensors ( Air Quality)
  - Air Traffic Monitoring ( Via opensky and Checking Local Airport for delays)
  - Local Street Traffic Via Wayze local map 
  - Local Bitwarden Password Management instance 
  - Power Management and monitoring for home
  - NMAP scan for devices within home
  - Bluetooth scaning for device changes in home 


Music Server (Siren):

- Debian 10 Vm within Proxmox Running AzuraCast
- VM container is 16 gigs in size

  - Dedicated Streaming Server for internet radio and performance streaming.
  - Planned possible update would be to migrate this to a larger VM on a better server and add DJ software to. Would use a thin Client to connect Turntables to in order to stream without needing a dedicated laptop at home can allocate additional storage for music library to keep everything together and act as a trinary backup (Secondary is Cloud Backup at present.) 

Pihole/PiVPN Server (Hemidall):

- Rasbian Jessie Vm within Proxmox
- VM is 16 gigs in Size
   - Specified Blocklist and VPN (mullvad)
   - Ad blocker and VM client using Pihole and Mullvad VPN service. 
   - Instance is meant to be able to send connection over to mobile devices via WireGuard Setup. 
   - Wireguard connection is set to connect to Blokada Instance in mobile devices for On the Go adblock/VPN 
 
 Signal Server (Hermes)

 - Debian 10 VM within Proxmox
 - VM is 20 gigs in Size
  - Stand alone Signal Messaging Server for the house using the dedicated number from Twilio. Main use is for home notifications from the smarthome system .
 
 PFsense router (Charon):

 - PFsense
 - VM 2gb in Size
  - Current minipc Quotom-Q575g6-s05 comes with up to 6 gigabit speed ethernet ports along with ability to add wifi via PCI card and cellular data capability. Idea is to use the extra ports as a dedicated Encrypted router and run ethernet ports to additional Server Rack within home once completed. This would allow for a local "private network" for certain devices while leaving the remaining devices in the home available on the "clear" network. 
 
Homeserver ( Site B):
OS:
- Proxmox VE
  - Current Size: ( as of 2/20/2020)   2tb JBOD Config in Raid 1 ( Mainly because drives are old and not currently used heavily)
- Features: 
  - Currently a JBOD of 2.5" Drives, 6 total equalling roughly 2tb with a possibility to add at least 2 more tb. 
  - Site B at present is mainly a testing environment Will be upgraded to full cloud storage once acquired remaining needed hardware to support it. 
  
   Current VM's Installed:
   
 Android X86:
 
  - Android 8.0
  - 32Gb in size
 Uses:
   - Android Os Rom testing and App testing with ADB Sideload
  
 Windows: 

  - Windows 7 R2
  - 100gb in size
 Uses:
   - Testing Exploits and in the rare case Windows was needed for anything. 

 # Servers (plans for expansion)
 - Quotom-Q575g6-s05 Mini Pc (Intel 7th generation i7 processor 16 gig of Ram/Kingston SUV500MS/ 240Gb SSD/ Kingston 120 SSD/ DualBand Wireless AC7260 pci Wifi/Bluetooth Adapter with Two Dual Band 7dbi 2.4Ghz/5ghz antennas/ Additional Twilio Backup Sim is also installed onboard but not currently active) "Site A" 
 
 - ASUS M5A97 R2.0 AM3+ AMD 970 SATA 6Gb/s USB 3.0 ATX AMD Motherboard (AmdFx 8350 processor 16gb Ram(expanding to 32 later)/ Samsung 512gb SSD, WD Black 2TB hard drive, 2.5" WD Blue 1Tb Drive( Planned migration to 6 WD IronWolf Green 4tb drives)/ AMD RX560 4gb Graphics Card/ AMD RX560 2gb Graphics Card 9 non-crossfire) "Site B" ( will be used as another Proxmox Node with Both Graphics Cards being in use for up to 2 seperate VM's for thin clients and HTPC usage With Plex) 
 
 - Asus AMD Zacate E350-m1-Pro- (Amd A5 processor, 10gb Ram, 6 WD 3tb Green Drives, AMD R7 4gb Graphics Card) "Site C" Will be used as a proxmox node but will mainly be used for Cloud Storage with NextCloud 
 
 - HP Proliant Mlgg3 Server (4gb of ram, Intel Xeon Processor 3.06GHz) (possible use as a dedicated mailserver due to age) 
 
 - Gigabyte F2A88XM-DH3 (Amd A-10 7890k Processor, 8gb Ram (expanding to 32gb later, Kingston 120gb SSD, WD Blue 1tb HDD,) Current use as a SteamOS remote pc and Emulation machine. Will Be used for Dedicated Home Theater Pc. 
 
 # Todo List
 
 - Build Dedicated Email Server
 - Build Dedicated Gaming Server ( SteamOS with support for RetroArch and custom Games to add on as well)
 - Custom built Smart Displays using recycled parts from decomissioned computers ( Can salvage ram, cameras, displays, microphones, and batteries) 
 - Add VM for CraftBeerPi (http://web.craftbeerpi.com/installation/), possibly BrewPi (https://www.brewpi.com/), and MashBerry (http://sebastian-duell.de/en/mashberry/index.html)
 - Build Information displays using the raspbery pi0w's. Software used will be Dietpi and running Chromium in Kiosk mode to point at a MagicMirror Server instance. Displays will be sourced from salvaged laptop parts (will add screen model no# later) and homemade frames. Will use 12v-5v stepdown converter with two outputs to power both the pi and display. Will need to source Control boards from ebay and add USB dongle for addons ( Motion sensor, Camera, Ect to regulate if display is awake or not. Possible Mic for satellite Ai Assistant support once opensource program is located to replace using SNIPS.AI) 
 - Config remaining 2 Rasberry Pi 3b devices for HTPC Streaming Client. Considering using Devices as a Steamlink Client to Remote SteamOS instance ( May need to make or clone VM for multiple instances but may need to research restrictions on logins for this for local play cases). Devices should be able to stream from VM fine as long as hardwired to network ( Home network includes multiple Cat-6 runs) Will need to test for input lag between wireless controls and IR input (Wireless is preferable to using cabled controllers)
 
