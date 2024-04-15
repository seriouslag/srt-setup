# Srt Setup

Setup SRT streaming to OBS

SRT is 2-3 times faster than RTMP - haivision.com

## Installation
### On PC
- install latest OBS 
- install the latest NodeJS LTS version (is this needed, I do not believe so)
    - [download](https://nodejs.org/)

### On Phone
- install Larix broadcaster
	- [Android](https://play.google.com/store/apps/details?id=com.wmspanel.larix_broadcaster)
	- [iOS](https://apps.apple.com/app/larix-broadcaster/id1042474385)
- install StreamCtrl
        - [Android](https://play.google.com/store/apps/details?id=dev.t4ils.obs_remote&pli=1)
        - iOS - TODO

## Setup - locally

Locally means on your home network connection

- Find IPV4 IP address - local
    - which will be referenced as `{local_ip}` from now on
    - on mac:
        - open terminal and enter: `ifconfig | grep inet | grep broadcast`
    - on windows:
        - open powershell or cmd and enter: `ipconfig`
            - It will be listed on the line of `IPv4 Address`

### Larix on a Phone

Open Larix - go to settings - Add a connection:
- name: `Local`
- URL: srt://`{local_ip}`:19492
- Mode: `Audio + Video`
- Srt sender mode: `Caller`
- latency (msec): `2000`

### OBS
- Click add source
	- Media Source
	- name - IRL Phone
	- Uncheck all boxes
   	- Set reconnect delay to 1 S
	- input: srt://`${local_ip}`:19492?connection_timeout=3000&latency=2000000&listen_timeout=5000000&mode=listener&smoother=live&transtype=live&timeout=5000000
- Setup Websockets
	- Open Tools (drop down) <  WebSocket Server Settings
	- Check `Enable Web Server`
	- Check `Enable System Tray Alerts`
	- Set Server Port to `44555`
	- Press `Generate Password`
	- Click Show Connect Info

#### After Larix and OBS are setup
- Navigate to the main screen on Larix
- Hit the grey button to go Live
- Should see phone camera in OBS in a few seconds

#### Remote control OBS
- In OBS, navigate to the Websocket Server Settings
    - Tools < Websocket Server Settings
    - Click `Show Connect Info`
- Open StreamCtrl on phone
- Click floating `+` Button
- Select OBS from the popup menu
- Click OK on popup
- Scan QR Code

## Setup - Remote Connection

A remote connection is a connection outside of your home network (off of wifi)

### Get a Static IP address
- Navigate to noip.com
- Enter a hostname, select `.hopto.org`
	- Hostname can be anything you like
	- Will reference what you entered as Hostname as `{hostname}` from now on
- Click `Sign Up`
- enter a password - do not include spaces or special characters, and keep it under 12 characters in length
- Check the terms of service
- Uncheck the Email Opt-in
- Click `Free Sign Up`
- Verify Email
	- Click the link in the email
	- Select Personal
	- Select Remote Access and `Continue`
	- Click `Go to My Account` in the FREE Dynamic DNS card
- On the dashboard - under create a Hostname
	- Enter Hostname: `{hostname}`
	- Domain: `hopto.org`
	- Record Type: `DNS Host A`
	- IPv4 Address: `{external_ip}`
	- Click `Create Hostname with DDNS Key`
- Navigate to Dashboard - you may be prompted with a Complete your Account Configuration popup
	- Click `Add Now`
	- Enable Two-Factor Authentication
- Setup DDNS Client
    - This automatically updates your non-static IP address to noip.com so your new hostname will route to your IP.
    - [Client](https://my.noip.com/dynamic-dns/duc)
    - Open DUCSetup_v4_1_1.exe and install the DUC.
    - Enter your No-IP username and password.
    - Select the hostname(s) or group(s) you would like to update.
 	- Click Save

### Port-Forward from your router

Port forwarding is the process of allowing connections from outside your home network to insides your home network.
We will need to allow specific connections to routed to your PC that is running OBS.

Each router has different configurations for setting up port-forwarding.
Your best bet is to Google, "Port forward on {insert your router brand and modal}"

#### SRT connection
  - Set up a rule to forward all connections
      - incoming port: 22222
      - destination the computers IP: `{local_ip}` port: 19492
      - protocol: BOTH

#### OBS Websocket
  - Set up a rule to forward all connections
      - incoming port: 44555
      - destination the computers IP: `{local_ip}` port: 44555
      - protocol: BOTH

#### Remote control OBS
- In OBS on your computer, navigate to the Websocket Server Settings
    - Tools < Websocket Server Settings
    - Click `Show Connect Info`
- Open StreamCtrl on the phone
- Click floating `+` Button
- Select OBS from the popup menu
- Click OK on the popup
- Scan QR Code
- Edit the new connection
- Name the new connection `Remote`
- Set the IP Address/Host to `{hostname}`.hopto.org
- Click the save button (floating checkmark)

### Setup Larix

Open Larix on your phone, navigate to settings, Add a connection

- create a new connection
- Name: Mobile
- URL: srt://`{hostname}`:19492
- Mode: `Audio + Video`
- Srt sender mode: `Caller`
- latency (msec): `2000`


###

After following you should be able to stream to your home computer's OBS and remotely control OBS over cellular data.
