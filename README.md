# Setup a Trackmania Forever Server on a remote Linux server

As I am writing this, Trackmania Forums are down so most of content about servers is not available. If anything said here is wrong, please contact me.

I'm making this repo so people can get an (almost) fast to setup trackmania server.

If you want to install a server with XAseco, follow [this tutorial](https://gitlab.com/tony.basso33/tmf-server-linux/-/tree/tmf-with-xaseco) instead.

## Requirements

- A VPS
- PuTTY to access VPS

## Installation

Using PuTTY, connect to your VPS.

### Download 
```
cd /home
git clone git@gitlab.com:tony.basso33/tmf-server-linux.git tmf
```

## Configuration
### Create your Dedicated Trackmania Server

You need a Dedicated Server account, that you can create on [this page](https://player.trackmania.com/ ). 

Navigate to the Dedicated servers tab on the left and create it.

### Configure Trackmania Server 

Edit Trackmania Server config file to set our credentials
```
cd /home/tmf
sudo nano /GameData/Config/dedicated_cfg.txt
```
Inside the `<masterserver_account>` tag, replace values with your server crendentials.
```
<masterserver_account>
		<login>YOUR_LOGIN</login>
		<password>YOUR_PASSWORD</password>
		<validation_key>YOUR_LAST_3_CHARACTERS_OF_VALIDATION_KEY</validation_key>
</masterserver_account>
```

### Packmask
It is used to dertermine which environments server will allow. It has these differents values:
- `stadium` **(required to play with TMNF)**
- `united` (so every environments are allowed)
- `original` (Speed, Rally, Alpine)
- `sunrise` (Island, Bay, Coast)
- `speed`, `rally`, `alpine`, `island`, `bay`, `coast` (to play on a single environment)
```
cd /home/tmf
sudo nano /GameData/Config/dedicated_cfg.txt
```

Inside the `<packmask>` tag, replace `stadium` with any value listed above.
```
<packmask>stadium</packmask>
```

### Match configuration
It is used to set global in game server configuration (time, gamemode, maps...)

Easiest way to configure match settings is to make a configuration directly from the game.

Go to Trackmania main menu > Party Mode > Local > Create > (set what you want) > Launch > (choose your maps) > Save Settings

A file will be created in your game MatchSettings (`Documents/TrackMania/Tracks/MatchSettings`)


⚠️ Don't forget to add selected maps to `GameData/Tracks` of server. Path of map must be the same as in your game folder.

### Port forwarding

- 2350: Server port `TCP/UDP`
- 4350: Server p2p port `TCP`
- 5000: Server xmlrpc port `TCP`

## Scripts
- Start: `./start.sh`
- Stop: `./stop.sh`

## Troubleshooting
In case you started the server but isn't showing in server list, check log files in the `Logs` folder.

## Useful links
- [Trackmania Server (2011-02-21) download link](http://files2.trackmaniaforever.com/TrackmaniaServer_2011-02-21.zip)
- [TMF Server Quickstart by Frans P. de Vries at gamers.org](https://www.gamers.org/tmf/quickstart.html)
- [How to set up a Dedicated Trackmania Server with XASECO & Records Eyepiece by Rhys Jones](https://medium.com/@Jonese1234/how-to-set-up-a-dedicated-trackmania-server-with-xaseco-records-eyepiece-d8a44dbf528e)
