# HomeLab
Description of my HomeLab/ Smart Home setup. 

# Contents
. Overview
. Gear
. Setup
. Servers
. To do list

# Overview
My Homelab consists of multiple repurposed copmuters (for now) and some upcycled parts salvaged from electronics around the home ( Mostly monitors and reflashed older cellphones. Each device and VM associated with the server uses will all be codenamed according to the following scheme for ease of distinction

In Addition I'm also including my Smarthome Setup in the listing since some of the functions and uses are actually tied in with the Lab uses as well and I feel it's just different enough to warrant mention. 

# Gear

- Quotom-Q575g6-s05 Mini Pc (Intel 7th generation i7 processor 16 gig of Ram/Kingston SUV500MS/ 240Gb SSD/ Kingston 120 SSD/ DualBand Wireless AC7260 pci Wifi/Bluetooth Adapter with Two Dual Band 7dbi 2.4Ghz/5ghz antennas/ Additional Twilio Backup Sim is also installed onboard but not currently active) "Site A"

- ASUS M5A97 R2.0 AM3+ AMD 970 SATA 6Gb/s USB 3.0 ATX AMD Motherboard (AmdFx 8350 processor 16gb Ram(expanding to 32 later)/ Samsung 512gb SSD, WD Black 2TB hard drive, 2.5" WD Blue 1Tb Drive( Planned migration to 6 WD IronWolf Green 4tb drives)/ AMD RX560 4gb Graphics Card/ AMD RX560 2gb Graphics Card 9 non-crossfire) "Site B" ( will be used as another Proxmox Node with Both Graphics Cards being in use for up to 2 seperate VM's for thin clients and HTPC usage With Plex) 

-Asus AMD Zacate E350-m1-Pro- (Amd A5 processor, 10gb Ram, 6 WD 3tb Green Drives, AMD R7 4gb Graphics Card) "Site C" Will be used as a proxmox node but will mainly be used for Cloud Storage with NextCloud 

- 10" lcd Touch panel hardwired to local alarm system for control

- 10 cameras. 5 cameras are modified from original firmware (WyzeCam mk2 and Wyzecam Pan)  to use RTSP and local Sim storage for monitoring. 3 are made from upcycled devices from local laptops and single board computer cameras. Cameras piggy back off of IR light from visible cameras for nightvision. Remaining two cameras are Nest cameras. Cameras also use MotionEye Os as a means of using cameras as a motion detection device

- Control devices: 5 Google Nexus devices.

 3 2012 google Nexus 5 devices running modified OS's ( UBPorts). 
   Any google software was removed from the devices and only run UBPORTS Ubuntu Touch Mobile OS. 

1 LG Nexus 5x. Device is Running MaruOs (Currently Okinawa Build) and had been modified to run microg and Kali Linux Nethunter on top of the Rom. Device can be used for controlling the home but main use is for another project and pentesting. 

1 2012 LG Nexus 7 LTE Tablet. Tablet has been modified to run LineageOS and Microg. Once again No Google accounts are associated with the device and only limited applications are used to support the system. Also Doubles usage as local remote control.

- In home Alarm System with Self Monitoring using Alarm Decoder AD2USB appliance 

- 3 Motion sensors all hardwired to the house mains along with battery backup

- OBI202 voip box. Used as POTS line. Rewired Phone Line from ouside the house. replaces CAT3 line used for primary phone line to CAT5e and replaced all ports in house. Phone line is using Twilio For emergency monitoring and Google Voice for VOIP service.

- Aeotec Z-stick Gen5 For Zwave device control 

# Setup 
Home Server( Proxmox Site A)
OS:
Proxmox VE
Features:
- System is backed protected against power loss in multiple steps. First step is main system is on a 96000mwa backup battery designed to hold power for up to 6 hours.
Uses:
- Main VM for home services (Smarthome control, Music Streaming Server, DNS filtering, Alarm System) 
Smarthome (Kerberos):
OS:
- Ubuntu Vm within Proxmox Running Docker and HomeAssistant Core 
- VM container is 50 gigs in size
Features: 
- Redundant backup automated to upload to two separate cloud accounts at varying times during the day, Cloud account holds 30 days worth of twice daily backups. The System itself holds 2 days of backups each for a total of 4 local daily snapshots saved to the system and 60 saved within cloud account.
- System is backed protected against power loss in multiple steps. First step is main system is on a 96000mwa backup battery designed to hold power for up to 6 hours (See Proxmox VE entry.) Second step is a dedicated raspberry pi 3b+ ( Soon to be upgraded to a pi4 4 gig model)  attached to a battery backup rated to keep the device and an interface running for up to 10 hours along with a usb modem to ensure in case of a power loss that critical notifications from the alarm system still reach intended persons. This is done VIA SMS and the device is hardwired to home power to charge automatically. Dedicated camera and reed switch sensor monitor for any tampering with where devices are stored. 
- Third Backup being developed for Offsite physical storage
- System consists of Vm image in combination with multiple sensors around the house for information. Much of the information is not available outside of local network and consists of several motion, light, infrared, pressure sensors along with several cameras made from local sourced materials and open source software to manage the system. The idea is to rely on as little commercial devices as possible or at least limit as much information as possible the data that is being sent from the devices. Several devices have been modified either physically or by firmware to accomplish this. 
- Local Wireguard Tunnel and backup Connection to system via Tor Hidden Service token allow for remote secure connection to home if web interface in unavailable because default remote connection is down. 
- In Case Network access is comepletely down System can still send and respond to actionable notifications from Dedicated SIM 
Uses: 
-  Smart Autmation Control of every light in the house
-  System also controls Security and Safety Systems within the house including Locks, Smoke/CO Alarm, Home Alarm systems, Thermostat
- Monitors occupancy within the house via a combination of Motion Sensors, Occupancy Sensors and monitoring electronic devices by MAC ID and Wifi Connection from NMAP and local bluetooth radios. 
- Monitoring Network Traffic and home ISP connection speed 
- Monitoring SIP home telephony connection and calls from OBIHAI device
- Temperature monitoring
- Vitals monitoring ( Via Google Fit API)
- Calendar
- Cryptocurrency Monitoring ( Crypto Prices from Bitcoin, Etherium, Litecoin, and dogecoin via CoinMarketCap)
- Weather forecast with Full local Radar conditions and Allergy Alerts, 
- Nasa Launch Schedule and ISS (international Space station) tracking 
- Selfhosted Bitwarden instance
- Self Hosted Matrix Node and IRC Chatroom via "The Lounge" integration
- RTSP camera Monitoring Via MotionEye Os integration 
- Local PiHole Instance 
- Emergency Notifications Via Email to SMS and dedicated Twilio account. 
- Non essential Notifications sent by push notification on local control devices.
- Environmental Sensors ( Air Quality)
- Air Traffic Monitoring ( Via opensky and Checking Local Airport for delays)
- Local Street Traffic Via Wayze local map 
- Local Bitwarden Password Management instance 
- Power Management and monitoring 

Music Server (Siren):
OS:
- Debian 10 Vm within Proxmox Running AzuraCast
- VM container is 16 gigs in size
Features: 

Uses: 
- Dedicated Streaming Server for internet radio and performance streaming.
- Planned possible update would be to migrate this to a larger VM on a better server and add DJ software to. Would use a thin Client to connect Turntables to in order to stream without needing a dedicated laptop at home can allocate additional storage for music library to keep everything together and act as a trinary backup (Secondary is Cloud Backup at present.) 

Pihole/PiVPN Server (Hemidall):
OS:
- Rasbian Jessie Vm within Proxmox
- VM is 16 gigs in Size
Features:
 - Specified Blocklist and VPN (mullvad)
 Uses:
 - Ad blocker and VM client using Pihole and Mullvad VPN service. 
 - Instance is meant to be able to send connection over to mobile devices via WireGuard Setup. 
 - Wireguard connection is set to connect to Blokada Instance in mobile devices for On the Go adblock/VPN 
 
 Signal Server
 OS:
 - Debian 10 VM within Proxmox
 - VM is 20 gigs in Size
Uses: Stand alone Signal Messaging Server for the house using the dedicated number from Twilio. Main use is for home notifications from the smarthome system .
 
 # Servers 
 - Quotom-Q575g6-s05 Mini Pc (Intel 7th generation i7 processor 16 gig of Ram/Kingston SUV500MS/ 240Gb SSD/ Kingston 120 SSD/ DualBand Wireless AC7260 pci Wifi/Bluetooth Adapter with Two Dual Band 7dbi 2.4Ghz/5ghz antennas/ Additional Twilio Backup Sim is also installed onboard but not currently active) "Site A" 
 
 - ASUS M5A97 R2.0 AM3+ AMD 970 SATA 6Gb/s USB 3.0 ATX AMD Motherboard (AmdFx 8350 processor 16gb Ram(expanding to 32 later)/ Samsung 512gb SSD, WD Black 2TB hard drive, 2.5" WD Blue 1Tb Drive( Planned migration to 6 WD IronWolf Green 4tb drives)/ AMD RX560 4gb Graphics Card/ AMD RX560 2gb Graphics Card 9 non-crossfire) "Site B" ( will be used as another Proxmox Node with Both Graphics Cards being in use for up to 2 seperate VM's for thin clients and HTPC usage With Plex) 
 
 -Asus AMD Zacate E350-m1-Pro- (Amd A5 processor, 10gb Ram, 6 WD 3tb Green Drives, AMD R7 4gb Graphics Card) "Site C" Will be used as a proxmox node but will mainly be used for Cloud Storage with NextCloud 
 
 
 
 # Todo List
 
 
