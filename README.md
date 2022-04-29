# Setup a Trackmania Forever Server with XAaseco on a remote Linux server

As I am writing this, Trackmania Forums are down so most of content about servers is not available. If anything said here is wrong, please contact me.

I'm making this repo so people can get an (almost) fast to setup trackmania server.

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

### PHP

XAseco uses php. So you need to install it
```
sudo yum update
sudo yum install php php-mysql wget unzip
```


### MySQL
XAseco needs a database to work. Unfortunately, Trackmania Server is quite old so it needs an older version of MySQL.

Therefore, download and install the [old yum MySQL repository](https://dev.mysql.com/downloads/repo/yum/)
```
cd
wget https://dev.mysql.com/get/mysql80-community-release-el7-6.noarch.rpm
sudo rpm -Uvh mysql80-community-release-el7-1.noarch.rpm
```

Then, disable MySql 8.0 and enable MySql 5.7. You should see it enabled
```
sudo yum-config-manager --disable mysql80-community
sudo yum-config-manager --enable mysql57-community
sudo yum repolist all | grep mysql
```

Install MySql
```
sudo yum install mysql-community-server
```

Run it
```
sudo systemctl start mysqld
```

Check if it runs correctly
```
sudo systemctl status mysqld
```

Finally, secure the installation (to prevent future bugs)
```
sudo grep 'temporary password' /var/log/mysqld.log
sudo mysql_secure_installation
```

Once MySql is installed, you have to create the aseco database and a user so the server can connect.

Login to mysql
```
mysql -u root -p
```

Create the database and user (change _MyPass1234!_ with your  password)
```
CREATE DATABASE aseco;
CREATE USER 'tmf'@'localhost' IDENTIFIED BY 'MyPass1234!';
GRANT all ON aseco.* TO 'tmf'@'localhost';
```

Type `exit;` to quit mysql and you're done! 


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


### XAseco configuration
In the `localdatabase.xml` file, change `YOUR_MYSQL_LOGIN` and `YOUR_MYSQL_PASSWORD` with the login and password of the user you created in mysql previously.

In the `dedimania.xml` file, locate the `<masterserver_account>` tag, replace values with your server credentials.
```
<masterserver_account>
		<login>YOUR_SERVER_LOGIN</login>
		<password>YOUR_SERVER_PASSWORD</password>
		<nation>YOUR_SERVER_NATION</nation>
</masterserver_account>
```
`<nation>` is the first three letters of your country (ex: FRA for France)

Note: you can the community code instead the password. Go [here](http://official.trackmania.com/tmf-communitycode/) and use you server credentials.

### Port forwarding

- 2350: Server port `TCP/UDP`
- 4350: Server p2p port `TCP`
- 5000: Server xmlrpc port `TCP`

## Scripts
- Start: `./start.sh`
- Start without XAseco: `./startWithoutXAseco.sh`
- Start XAseco: `./startXAseco.sh` (to use when server is started without Xaseco)
- Stop: `./stop.sh`

## Troubleshooting
In case you started the server but isn't showing in server list, check log files in the `Logs` folder.

In case XAseco doesn't start, check the logs in the `xaseco` folder, either `aseco.log` or `logfile.txt`. Errors here happens mostly because of wrong installation, and a fix can certainly be found easily on the internet as it's not directly related to XAseco.

## Useful links
- [Trackmania Server (2011-02-21) download link](http://files2.trackmaniaforever.com/TrackmaniaServer_2011-02-21.zip)
- [XAseco 116 download link](https://www.gamers.org/tmn/xaseco_116.zip)
- [Trackmania Community Code page](http://official.trackmania.com/tmf-communitycode/)
- [TMF Server Quickstart by Frans P. de Vries at gamers.org](https://www.gamers.org/tmf/quickstart.html)
- [How to set up a Dedicated Trackmania Server with XASECO & Records Eyepiece by Rhys Jones](https://medium.com/@Jonese1234/how-to-set-up-a-dedicated-trackmania-server-with-xaseco-records-eyepiece-d8a44dbf528e)
