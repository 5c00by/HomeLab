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

- Quotom-Q575g6-s05 Mini Pc (Intel 7th generation i7 processor 16 gig of Ram/Kingston SUV500MS/ 240Gb SSD/ Kingston 120 SSD/ DualBand Wireless AC7260 pci Wifi/Bluetooth Adapter with Two Dual Band 7dbi 2.4Ghz/5ghz antennas/ Additional Twilio Backup Sim is also installed onboard ) - Main use Firewall via PFSense

- ASUS M5A97 R2.0 AM3+ AMD 970 SATA 6Gb/s USB 3.0 ATX AMD Motherboard (AmdFx 8350 processor 16gb Ram(expanding to 32 later)/ Samsung 512gb SSD, WD Black 2TB hard drive, 2.5" WD Blue 1Tb Drive/ AMD RX560 4gb Graphics Card)  

- Asus AMD Zacate E350-m1-Pro- (Amd A5 processor, 10gb Ram, 120 gb SSD. Will be used as Open MediaVault/Nextcloud instance for backups when more storage is added. Planned expansion to 24 tb SnapRAID/UnionFS build with at least 2-3 parity drives if Raid Controller addon allows

- Gigabyte F2A88XM-DH3 (Amd A-10 7890k Processor, 18gb Ram ,AMD R7250 4gbVram Gpu, Rx560 2Gb VRAM GPU 24 TB JBOD Current use as a Proxmox Test Lab

- Dell PowerEdge R710 3.5" (48 gig RAM, 46 4tb, WD IRonwolf Nas Drives, Modified riser install for Nvidia GT630 4gb GPU) _ Primary use Main Proxmox Server 

- Dell Compellent SC220 24Bay 2.5", No drives yet, Will use to migrate VM's at later date for better usage with SAS storage medium and RAID Z1

  - 1 LG Nexus 5x. Device is Running MaruOs (Currently Okinawa Build) and had been modified to run microg and Kali Linux Nethunter on top of the Rom. Device can be used for controlling the home but main use is for another project and pentesting. With MaruOs this allows for phone to be used in "convergence" to allow for a full Debian Linux desktop when connected to a supported phone dock or casted from phone to a compatible display. 

- HP Proliant Ml330 Generation 3 Server (4gigs) 

- 2 Wyse Terminals (Model Cx0 C30LE.Ce 128f/512r.DVI US)

- 2 Hp Compaq ddx2200 Microtower pc (2gb Ram, Pentium4 processor, 1 kingston 120gb SSD, 1 WD Blue 320gb HDD) Mainly Thin Clients

- TP-Link TL-SG116E UnManaged Switch

- Huawei e3372 USB Modem (Twilio Sim Attached to it.)

- Raspberry Pi 4 8gb - Kali Linux Test Environment

- Raspberry Pi 4 4gb - Docker/Smart Home system

# Setup 


Smarthome:

-  HomeAssistant Supervised Install
-  120gb size
Features: 
- Redundant backup automated to upload to two separate cloud accounts at varying times during the day, Cloud account holds 30 days worth of twice daily backups. The System itself holds 2 days of backups each for a total of 4 local daily snapshots saved to the system and 60 saved within cloud account.
- System is backed protected against power loss in multiple steps. First step is main system is on a 19500Mwa backup battery designed to hold power for up to 6 hours   attached to a battery backup rated to keep the device and an interface running for up to 10 hours along with a usb modem to ensure in case of a power loss that critical notifications from the alarm system still reach intended persons. This is done VIA SMS (Through Twilio Sim and Api Calls if power is inconsistent) and the device is hardwired to home power to charge automatically. Dedicated camera and reed switch sensor monitor for any tampering with where devices are stored. 
- Third Backup being developed for Offsite physical storage
- System consists of Vm image in combination with multiple sensors around the house for information. Much of the information is not available outside of local network and consists of several motion, light, infrared, pressure sensors along with several cameras made from local sourced materials and open source software to manage the system. The idea is to rely on as little commercial devices as possible or at least limit as much information as possible the data that is being sent from the devices. Several devices have been modified either physically or by firmware to accomplish this. 
- Local Wireguard Tunnel and backup Connection to system via Tor Hidden Service token allow for remote secure connection to home if web interface in unavailable because default remote connection is down. 
- In Case Network access is comepletely down System can still send and respond to actionable notifications from Dedicated SIM 

Uses:
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
  - Self Hosted Matrix Server and IRC Chatroom 
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
  - SMS Notifications using Twilio sent through Signal Messenger

 
PFsense router:

 - PFsense
 - 240 gb in Size
   - Current minipc Quotom-Q575g6-s05 comes with up to 6 gigabit speed ethernet ports along with ability to add wifi via PCI card and cellular data capability. Idea is to use the extra ports as a dedicated Encrypted router and run ethernet ports to additional Server Rack within home once completed. This would allow for a local "private network" for certain devices while leaving the remaining devices in the home available on the "clear" network. 
  - System is backed protected against power loss in multiple steps. First step is main system is on a 19500Mwa backup battery designed to hold power for up to 6 hours.

  - PFSENSE Firewall/ Server Rack Connection 
  
 
Homeserver:
OS:
- Proxmox VE
  - Current Size: ( as of 10/08/2020)   24tb JBOD Config in Raid 1 ( Mainly because drives are old and not currently used heavily)
- Features: 
  - Currently a JBOD of 3.5" Drives, 12 total equalling roughly 24tb with a possibility to add at least 2 more tb. 
  -
  
   Current VM's Installed:
   
 Android X86:
 
  - Android 9.0
  - 32Gb in size
 Uses:
   - Android Os Rom testing and App testing with ADB Sideload
  
 Windows: 

  - Windows 7 R2
  - 100gb in size
 Uses:
   - Testing Exploits and in the rare case Windows was needed for anything. 

Magic Mirror Server:
  
  - Ubuntu Server VM
  - VM 32 gb in size
    - Server is for Instances of Magic Mirror builds (https://magicmirror.builders/) to use with information display panels built throughout the house. The Raspberry pi Zero W's will act as a client to stream the mirrors and also running room assistant and Motion Eye clients for presence detection. The pi's will be flashed with Dietpi (https://dietpi.com/dietpi-software.html) to conserve space and easier usage to deploy docker to install roomassistant, Chromium in kiosk mode for the display and also motioneye through the addon service and save space on SD card for install compared to Raspbian. 
    
Music Server:

- Debian 10 Vm within Proxmox Running AzuraCast and Mixxx
- VM container is 120 gigs in size
- Planned expansion to add another storage for backed up music files and passing through for Live Music sets

  - Dedicated Streaming Server for internet radio and performance streaming.
  - Planned possible update would be to migrate this to a larger VM on a better server and add DJ software to. Would use a thin Client to connect Turntables to in order to stream without needing a dedicated laptop at home can allocate additional storage for music library to keep everything together and act as a trinary backup (Secondary is Cloud Backup at present.) 

Pihole/PiVPN Server:

- Rasbian Jessie Vm within Proxmox
- VM is 16 gigs in Size
   - Ad blocker and VM client using Pihole and Mullvad VPN service. 
   - Instance is meant to be able to send connection over to mobile devices via WireGuard Setup. 
   - Wireguard connection is set to connect to Blokada Instance in mobile devices for On the Go adblock/VPN 
 
   
 NextCloud LXC Turnkey Container
 
   - 8gb LXC container
   - Uses dedicated mount storage within Proxmox Server
   - Information is rsynced to offsite storage 
   - Maria DB as database
 
 Docker VM
 
  - Ubuntu 20.04 VM
  - 200 gb in Size
  - Current Running:
     . Heimdall
     . Signal Server
     . Magic Mirror
     . Nodered
     
     
  
 Windows 10 Vm
   - 200 gb Vm
   - Main use for running any needed windows programs and migration of data from old NTFS formatted drives

 Plex vm
   - Ubuntu Server 20.04
   - Currently Passing 3 tb Drive through to VM, Will move over to different storage later


 
 # Todo List
 
 - Build Dedicated Email Server
 - Build Dedicated Gaming Server ( SteamOS with support for RetroArch and custom Games to add on as well)
 - Custom built Smart Displays using recycled parts from decomissioned computers ( Can salvage ram, cameras, displays, microphones, and batteries) 
 - Add VM for CraftBeerPi (http://web.craftbeerpi.com/installation/), possibly BrewPi (https://www.brewpi.com/), and MashBerry (http://sebastian-duell.de/en/mashberry/index.html)
 - Build Information displays using the raspbery pi0w's. Software used will be Dietpi and running Chromium in Kiosk mode to point at a MagicMirror Server instance. Displays will be sourced from salvaged laptop parts (will add screen model no# later) and homemade frames. Will use 12v-5v stepdown converter with two outputs to power both the pi and display. Will need to source Control boards from ebay and add USB dongle for addons ( Motion sensor, Camera, Ect to regulate if display is awake or not. Possible Mic for satellite Ai Assistant support once opensource program is located to replace using SNIPS.AI) 
 - Config remaining 2 Rasberry Pi 3b devices for HTPC Streaming Client. Considering using Devices as a Steamlink Client to Remote SteamOS instance ( May need to make or clone VM for multiple instances but may need to research restrictions on logins for this for local play cases). Devices should be able to stream from VM fine as long as hardwired to network ( Home network includes multiple Cat-6 runs) Will need to test for input lag between wireless controls and IR input (Wireless is preferable to using cabled controllers)
 - Update pi-Zero W's to run Motioneye (https://github.com/ccrisan/motioneye/wiki)  and Room Assistant (https://github.com/mKeRix/room-assistant) 
 - Attempt to see if possible to add either a Riot client (https://about.riot.im/features)  or Linphone client (https://www.linphone.org/) to pi to make Smart mirror capable of intercom like features and mesaging. May need to switch from a mirror to using an upcycled replacement touchscreen monitor build for this (possible option MT230DW03 V.0 replacement monitor https://www.ebay.com/itm/Dell-Inspiron-One-2320-23-Desktop-LCD-Touch-Screen-Glossy-MT230DW03-V-0/184154777593?_trkparms=aid%3D111001%26algo%3DREC.SEED%26ao%3D1%26asc%3D225074%26meid%3D55994aa369034417947f07bf3da358ac%26pid%3D100675%26rk%3D1%26rkt%3D15%26mehot%3Dnone%26sd%3D184154777593%26itm%3D184154777593%26pmt%3D0%26noa%3D1%26pg%3D2380057&_trksid=p2380057.c100675.m4236&_trkparms=pageci%3Aa8b6d093-5f10-11ea-8454-74dbd180f09e%7Cparentrq%3Aabfee74f1700aa66c9b3943cffea8ca0%7Ciid%3A1)
 
