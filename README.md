# Srt Setup

Setup SRT streaming to OBS

SRT is 2-3 times faster than RTMP - haivision.com

## Installation
### On PC
- install latest OBS 
- install latest nodejs LTS
	  https://nodejs.org/es
- obs websockets
	- https://github.com/obsproject/obs-websocket
	- https://github.com/obsproject/obs-websocket/releases
		- install v5.0.1 or greater
		- do not install `compat` versions
	- this will allow us to remotely control OBS so we can start/stop the stream

### On Phone
- install Larix broadcaster
	- [Android](https://play.google.com/store/apps/details?id=com.wmspanel.larix_broadcaster)
	- [iOS](https://apps.apple.com/app/larix-broadcaster/id1042474385)

## Setup - locally

- Find IP address - local
	- which will be referenced as `{local_ip}` from now on

### Larix on Phone

- name: `Local`
- URL: srt://`{local_ip}`:22222
- Mode: `Audio + Video`
- Srt sender mode: `Caller`
- latency (msec): `2000`

### OBS
- Click add source
	- webcam
		- name - IRL Phone
		- Uncheck all boxes
		- Set reconnect delay to 1.5
		- input: srt://`${local_ip}`:22222?connection_timeout=3000&latency=2000000&listen_timeout=5000000&mode=listener&smoother=live&transtype=live&timeout=5000000

After Larix and OBS are setup
- Navigate to main screen on Larix
- Hit grey button to go Live
- Should see phone camera in OBS in a few seconds


## Setup - Remote

### Get Static IP address
- Navigate to noip.com
- Enter a hostname, select `.hopto.org`
	- Hostname can be anything you like
	- Will reference what you entered as Hostname as `{hostname}` from now on
- Click `Sign Up`
- enter a password - do not include spaces or special characters, and keep it under 12 characters of length
- Check the terms of service
- Uncheck the Email Opt-in
- Click `Free Sign Up`
- Verify Email
	- Click Link in email
	- Select Personal
	- Select Remote Access and `Continue`
	- Click `Go to My Account` in the FREE Dynamic DNS card
- ON the dashboard - under create a Hostname
	- Enter Hostname: `{hostname}`
	- Domain: `hopto.org`
	- Record Type: `DNS Host A`
	- IPv4 Address: `{external_ip}`
	- Click `Create Hostname with DDNS Key`
- Navigate to Dashboard - you may be prompted with a Complete your Account Configuration popup
	- Click `Add Now`
	- Enable Two-Factor Authentication

### Port-Forward from your router
- Set up rule to forward all connections
	  incoming port: 22222
	  destination the computers IP: `{local_ip}` port: 22222
	  protocol: BOTH

### Setup Larix
- create a new connection
- Name: Mobile
- URL: srt://`{hostname}`:22222
- Mode: `Audio + Video`
- Srt sender mode: `Caller`
- latency (msec): `2000`

