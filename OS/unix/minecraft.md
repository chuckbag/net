# Minecraft

## Install Minecraft:
Install Java-JDK
```bash
yum install java-1.6.0-openjdk
```

Make a directory for Minecraft:
```bash
mkdir /opt/minecraft
```

Download minecraft and put it in the directory. Then make it an executable.  (or [download the server file here](https://minecraft.net/download))
```bash
wget http://minecraft.net/download/minecraft_server.jar /opt/minecraft/.
chmod +x /opt/minecraft/minecraft_server.jar
```

Out of the box, minecraft cannot be run as a deamon.  so make sure you have
screen installed so you can "background" the app.
```bash
yum install screen
```

Start screen with the following command:
```bash
screen
```

Create a startup script by entering the following:
```bash
echo java -Xmx1024M -Xms1024M -jar minecraft_server.jar nogui | startMinecraft.bash
chmod +x startMinecraft.bash
```

## Run Minecraft:

### Start service:

Start the minecraft server with the following command:
```bash
./startMinecraft.bash
```

Use the different [screen commands](screen.md) to close the modify the current session without closing the app. 

### Stop service:
To shutdown the server off do the following:

First save the session with the /save-all command:
```
/save-all
2012-08-14 17:28:21 [INFO] Saving...
2012-08-14 17:28:21 [INFO] Saved the world
```

Then shut down the server with the /stop command:
```
/stop 
2012-08-14 17:31:15 [INFO] Stopping the server
2012-08-14 17:31:15 [INFO] Stopping server
2012-08-14 17:31:15 [INFO] Saving players
2012-08-14 17:31:15 [INFO] Saving worlds
2012-08-14 17:31:15 [INFO] Saving chunks for level 'world'/xd@37722456
2012-08-14 17:31:15 [INFO] Saving chunks for level 'world'/xc@26afa68a
2012-08-14 17:31:15 [INFO] Saving chunks for level 'world'/xe@55dec1dd
[centos-test minecraft]#
```

## Modify the Server:
The `server.properties` file allows you to modify how the server will work. 

See all [details on this file from the minecraft wiki](http://www.minecraftwiki.net/wiki/Server.properties).

With this install, modify the file with the following command:
```bash
vim /opt/minecraft/server.properties
```


A couple of examples of changes you could make would be the following:
```
# ---------------------------------------------------------------
# nice message of the day
motd=chucks vm minecraft servermotd=chucks vm minecraft server
# allow non-licensed users to connect to my private server
online-mode=false
# setup the server to (0= Survival, 1= Creative)
gamemode=1
```


