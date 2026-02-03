<div align="center">

# 🎮 Palworld Dedicated Server with Docker

**Complete guide to create and manage your own Palworld dedicated server for free**

[![Docker](https://img.shields.io/badge/Docker-Latest-2496ED?logo=docker)](https://www.docker.com/)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-Latest-2496ED?logo=docker)](https://docs.docker.com/compose/)
[![Palworld](https://img.shields.io/badge/Palworld-Server-orange)](https://www.palworld.gg/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-Compatible-E95420?logo=ubuntu)](https://ubuntu.com/)

[![Create Your Own FREE Palworld Dedicated Server](https://github.com/TechBeme/Palworld/assets/101749351/970e4455-bc9e-4406-be1a-a43183c178d4)](https://youtu.be/ZXk4wE1rcXM)

**Languages:** [🇧🇷 Português](README.md) • [🇪🇸 Español](README.es.md)

</div>

---

## 📋 Table of Contents
- [📖 Introduction](#-introduction)
- [🚀 Server Creation Steps](#-server-creation-steps)
  - [Choosing a Hosting Service](#1-choosing-a-hosting-service)
  - [Accessing and Configuring the Virtual Machine](#2-accessing-and-configuring-the-virtual-machine)
  - [Palworld Server Configuration](#3-palworld-server-configuration)
  - [Accessing the Server](#4-accessing-the-server)
- [🔄 How to Update the Palworld Server](#-how-to-update-the-palworld-server)
- [🐳 How to Update the Docker Image](#-how-to-update-the-docker-image)
- [⚙️ How to Change Palworld Server Settings](#%EF%B8%8F-how-to-change-palworld-server-settings)
  - [List of Palworld Dedicated Server Configuration Parameters](#1-list-of-palworld-dedicated-server-configuration-parameters)
  - [List of Palworld Dedicated Server Admin Commands](#2-list-of-palworld-dedicated-server-admin-commands)
- [💾 How to Automatically Backup and Restore Palworld Server Saves](#-how-to-automatically-backup-and-restore-palworld-server-saves)

---

## 📖 Introduction
Creating a dedicated server for Palworld allows you to play with friends anytime, without depending on your PC or personal internet. This guide covers creating a server using cloud services like Oracle Cloud, Google Cloud, AWS, and Hostinger.

> [!WARNING]
> Currently, dedicated servers are exclusively available for Steam users. Unfortunately, Xbox Gamepass or Xbox Console players cannot connect to dedicated servers. For these players, the only available option is to join private invite-only sessions that support a maximum of 4 players.

## 🚀 Server Creation Steps
### 1. Choosing a Hosting Service

| VPS | CPUs | Ram | Price |
|-----|------|-----|-------|
| [Oracle Cloud](https://www.oracle.com/cloud/free) | 4 | 24 GB | Free |
| [Google Cloud](https://cloud.google.com) | X | X | $300 free credit for 3 months |
| [AWS](https://aws.amazon.com/free) | X | X | Free for 1 year |
| [Hostinger](https://hostinger.com?REFERRALCODE=1RFSV68) | 2 | 8 GB | $34.99 |

### 2. Accessing and Configuring the Virtual Machine
- **Create an Account on the chosen service.**
- **Create a New Virtual Machine (VM):**
   - **Location:** Choose the closest for **lower latency** (example: São Paulo, Brazil).
   - **Settings:** In this guide I will use the **Ubuntu** Operating System.
- **Access the VM via SSH.**
- **Get Root Access and Update the Machine:**
   ```
   sudo su
   ```
   ```
   sudo apt update
   sudo apt upgrade
   ```
- **Install Docker:**
   - Follow the instructions from Docker's [official website](https://docs.docker.com/engine/install/) for installation.
      - [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
      - [Debian](https://docs.docker.com/engine/install/debian/)
      - [Fedora](https://docs.docker.com/engine/install/fedora/)
      - [CentOS](https://docs.docker.com/engine/install/centos/)
      - [Raspberry Pi OS (32-bit)](https://docs.docker.com/engine/install/raspberry-pi-os/)
      - [RHEL (s390x)](https://docs.docker.com/engine/install/rhel/)
      - [SLES](https://docs.docker.com/engine/install/sles/)
      - [Binaries](https://docs.docker.com/engine/install/binaries/)
- **Verify the installation**:
  ```
  docker --version
  ```
   - **If you get the error "dial unix /var/run/docker.sock: connect: permission denied" it means you are not the root user.**
      - Simply add the sudo command before any docker command:
         ```
         sudo docker --version
         ```
      - or switch to the root user and execute the commands normally:
         ```
         sudo su
         docker --version
         ```

### 3. Palworld Server Configuration
- **Create a file called [docker-compose.yml](/docker-compose.yml).**
   ```
   sudo nano docker-compose.yml
   ```
- **Insert your preferred server settings.**
   - Example configuration:
   ```yml
    services:
       palworld:
          image: thijsvanloef/palworld-server-docker:latest
          restart: unless-stopped
          container_name: palworld-server
          ports:
            - 8211:8211/udp
            - 27015:27015/udp
          environment:
             - PUID=1000
             - PGID=1000
             - PORT=8211
             - PLAYERS=16 # Number of server slots
             - MULTITHREADING=true
             - RCON_ENABLED=true
             - RCON_PORT=25575
             - TZ=UTC-3
             - COMMUNITY=true  # Shows your server in the community listing.
             - SERVER_NAME=Server name
             - SERVER_DESCRIPTION=Server description
             #- SERVER_PASSWORD= # Server password. (note: remove the hashtag before "- SERVER_PASSWORD" to use a password)
             - ADMIN_PASSWORD=admin # Administrator password.
             # If you want to edit the server settings, add the commands below: 
             - DEATH_PENALTY=Item #Death penalty: None: No death penalty; Item: Drops items except equipment; ItemAndEquipment: Drops all items; All: Drops all PALs and all items.
             - ENABLE_INVADER_ENEMY=True # Enable/disable invasions
             - PAL_EGG_DEFAULT_HATCHING_TIME=1 # Time in hours to hatch eggs.
             - DISCORD_WEBHOOK_URL= # Insert your discord webhook, something like: https://discord.com/api/webhooks/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
             - DISCORD_PRE_UPDATE_BOOT_MESSAGE=Expanding... The pal is increasing its power!
             - DISCORD_POST_UPDATE_BOOT_MESSAGE=Update complete! The pal is ready, stronger than ever.
             - DISCORD_PRE_START_MESSAGE=The pal woke up! Ready for the hunt, strong and steady.
             - DISCORD_PRE_SHUTDOWN_MESSAGE=Shutting down... The pal is exhausted, needs some time.
             - DISCORD_POST_SHUTDOWN_MESSAGE=Hold on, the pal is falling... it's down!
          volumes:
             - ./servidor:/palworld/
   ```
   - Set the port, number of players, password, and other settings.
   - Access the complete documentation [here](https://github.com/thijsvanloef/palworld-server-docker).
- **Save the Configuration File.**
- **Create the server:**
   ```
   sudo docker compose up
   ```
   - Done, now just wait for the server to start completely.

### 4. Accessing the Server
- **Open the port in the Firewall:**
   - Create a UDP rule for the game with the port you defined (Default: 8211).
- **Get the VM's External IP.**
- **Open Palworld and Connect to the Server:**
   - Go to multiplayer and use IP:PORT to connect.

---

## 🔄 How to Update the Palworld Server
[![How to UPDATE Your Palworld Dedicated Server to the New Version](https://github.com/TechBeme/Palworld/assets/101749351/78e6a562-8420-418b-9a2b-5afa823fba92)](https://youtu.be/6tdbGYvDKOU)

To update the server to the latest version of Palworld, simply restart the server.
Make sure you are the root user and in the same directory as the [docker-compose.yml](/docker-compose.yml) file.
* Restart the server:
```
sudo docker compose restart
```

---

## 🐳 How to Update the Docker Image
### For Windows Users:
  - **Access Docker's Graphical Interface:**
    - Navigate to 'Containers' and delete the current container by clicking the trash icon.
    - Then, go to 'Images', locate the Palworld server image, click the three dots and select 'Pull' to update the image.

### For Linux or Command Line Users:
> **Useful commands:**
>   - View the list of containers:
>   ```
>   docker container list
>   ```
>   - Find the server directory:
>   ```
>   docker container inspect palworld-server -f '{{ json .Mounts }}'
>   ```
  1. Open the terminal in the Docker Compose folder.
  2. Stop and remove the server container:
        ```
        docker stop palworld-server
        docker rm palworld-server
        ```
  3. Update the server image:
        ```
        docker pull thijsvanloef/palworld-server-docker:latest
        ```
  4. Make sure your [docker-compose.yml](/docker-compose.yml) file is updated with the latest settings.
  5. Recreate the container with the command:
        ```
        docker compose up -d
        ```

---

## ⚙️ How to Change Palworld Server Settings
[![How to Change Your Palworld Dedicated Server Settings](https://github.com/TechBeme/Palworld/assets/101749351/243722b4-e1d2-425d-9735-55ac86222cfd)](https://youtu.be/PsjYGGFpaqo)

This guide explains how to change various settings on your Palworld dedicated server, for example, disable item drop on death, reduce egg incubation time, and speed up crafting.

> [!WARNING]
> IT IS NO LONGER NECESSARY TO CHANGE THE ```PalWorldSettings.ini``` FILE
>
> Now it is possible to change settings directly in your [docker-compose.yml](/docker-compose.yml)
> 
> HOWEVER, YOU MUST FIRST UPDATE YOUR DOCKER IMAGE ACCORDING TO THIS GUIDE: [How to Update the Docker Image](#-how-to-update-the-docker-image)


### 1. List of Palworld Dedicated Server Configuration Parameters
These parameters MUST BE CHANGED IN [docker-compose.yml](/docker-compose.yml) (NOT IN ```PalWorldSettings.ini```). These settings allow you to customize the server's gaming experience.

> [!WARNING]
> These are all the commands that exist in the game files, but not all are working.
> The game hasn't implemented them yet, it's not a server problem!

| parameters                                | Description                                                        | Default Value                                                                                | Allowed Value                          |
|-------------------------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------|----------------------------------------|
| DIFFICULTY                                | Game difficulty                                                    | None                                                                                         | `None`,`Normal`,`Difficult`            |
| DAYTIME_SPEEDRATE                         | Day speed - Lower numbers mean shorter days                        | 1.000000                                                                                     | Decimal Number                         |
| NIGHTTIME_SPEEDRATE                       | Night speed - Lower numbers mean shorter nights                    | 1.000000                                                                                     | Decimal Number                         |
| EXP_RATE                                  | EXP gain rate                                                      | 1.000000                                                                                     | Decimal Number                         |
| PAL_CAPTURE_RATE                          | Pal capture rate                                                   | 1.000000                                                                                     | Decimal Number                         |
| PAL_SPAWN_NUM_RATE                        | Pal spawn rate                                                     | 1.000000                                                                                     | Decimal Number                         |
| PAL_DAMAGE_RATE_ATTACK                    | Pal damage multiplier                                              | 1.000000                                                                                     | Decimal Number                         |
| PAL_DAMAGE_RATE_DEFENSE                   | Damage multiplier to Pals                                          | 1.000000                                                                                     | Decimal Number                         |
| PLAYER_DAMAGE_RATE_ATTACK                 | Player damage multiplier                                           | 1.000000                                                                                     | Decimal Number                         |
| PLAYER_DAMAGE_RATE_DEFENSE                | Damage multiplier to player                                        | 1.000000                                                                                     | Decimal Number                         |
| PLAYER_STOMACH_DECREASE_RATE              | Player hunger depletion rate                                       | 1.000000                                                                                     | Decimal Number                         |
| PLAYER_STAMINA_DECREASE_RATE              | Player stamina reduction rate                                      | 1.000000                                                                                     | Decimal Number                         |
| PLAYER_AUTO_HP_REGEN_RATE                 | Player automatic HP regeneration rate                              | 1.000000                                                                                     | Decimal Number                         |
| PLAYER_AUTO_HP_REGEN_RATE_IN_SLEEP        | Player HP regeneration rate during sleep                           | 1.000000                                                                                     | Decimal Number                         |
| PAL_STOMACH_DECREASE_RATE                 | Pal hunger depletion rate                                          | 1.000000                                                                                     | Decimal Number                         |
| PAL_STAMINA_DECREASE_RATE                 | Pal stamina reduction rate                                         | 1.000000                                                                                     | Decimal Number                         |
| PAL_AUTO_HP_REGEN_RATE                    | Pal automatic HP regeneration rate                                 | 1.000000                                                                                     | Decimal Number                         |
| PAL_AUTO_HP_REGEN_RATE_IN_SLEEP           | Pal health regeneration rate during sleep (in Palbox)              | 1.000000                                                                                     | Decimal Number                         |
| BUILD_OBJECT_DAMAGE_RATE                  | Structure damage multiplier                                        | 1.000000                                                                                     | Decimal Number                         |
| BUILD_OBJECT_DETERIORATION_DAMAGE_RATE    | Structure deterioration rate                                       | 1.000000                                                                                     | Decimal Number                         |
| COLLECTION_DROP_RATE                      | Collectible items multiplier                                       | 1.000000                                                                                     | Decimal Number                         |
| COLLECTION_OBJECT_HP_RATE                 | Collectible objects HP multiplier                                  | 1.000000                                                                                     | Decimal Number                         |
| COLLECTION_OBJECT_RESPAWN_SPEED_RATE      | Collectible objects respawn interval - Lower number, faster regen  | 1.000000                                                                                     | Decimal Number                         |
| ENEMY_DROP_ITEM_RATE                      | Enemy dropped items multiplier                                     | 1.000000                                                                                     | Decimal Number                         |
| DEATH_PENALTY                             | Death penalty</br>None: No death penalty</br>Item: Drops items except equipment</br>ItemAndEquipment: Drops all items</br>All: Drops all PALs and all items. | All        | `None`,`Item`,`ItemAndEquipment`,`All` |
| ENABLE_PLAYER_TO_PLAYER_DAMAGE            | Allows players to damage other players                             | False                                                                                        | True or False                          |
| ENABLE_FRIENDLY_FIRE                      | Allows friendly fire                                               | False                                                                                        | True or False                          |
| ENABLE_INVADER_ENEMY                      | Enables invaders                                                   | True                                                                                         | True or False                          |
| ACTIVE_UNKO                               | Enables UNKO (?)                                                   | False                                                                                        | True or False                          |
| ENABLE_AIM_ASSIST_PAD                     | Enables aim assist for controller                                  | True                                                                                         | True or False                          |
| ENABLE_AIM_ASSIST_KEYBOARD                | Enables aim assist for keyboard                                    | False                                                                                        | True or False                          |
| DROP_ITEM_MAX_NUM                         | Maximum number of dropped items in the world                       | 3000                                                                                         | Integer Number                         |
| DROP_ITEM_MAX_NUM_UNKO                    | Maximum number of UNKO drops in the world                          | 100                                                                                          | Integer Number                         |
| BASE_CAMP_MAX_NUM                         | Maximum number of base camps                                       | 128                                                                                          | Integer Number                         |
| BASE_CAMP_WORKER_MAX_NUM                  | Maximum number of workers                                          | 15                                                                                           | Integer Number                         |
| DROP_ITEM_ALIVE_MAX_HOURS                 | Time until items disappear in hours                                | 1.000000                                                                                     | Decimal Number                         |
| AUTO_RESET_GUILD_NO_ONLINE_PLAYERS        | Automatically resets guild when no players are online              | False                                                                                        | True or False                          |
| AUTO_RESET_GUILD_TIME_NO_ONLINE_PLAYERS   | Time to automatically reset guild when no players are online       | 72.000000                                                                                    | Decimal Number                         |
| GUILD_PLAYER_MAX_NUM                      | Maximum number of players in Guild                                 | 20                                                                                           | Integer Number                         |
| PAL_EGG_DEFAULT_HATCHING_TIME             | Time (h) to hatch massive egg                                      | 72.000000                                                                                    | Decimal Number                         |
| WORK_SPEED_RATE                           | Work speed multiplier                                              | 1.000000                                                                                     | Decimal Number                         |
| IS_MULTIPLAY                              | Enables multiplayer                                                | False                                                                                        | True or False                          |
| IS_PVP                                    | Enables PVP                                                        | False                                                                                        | True or False                          |
| CAN_PICKUP_OTHER_GUILD_DEATH_PENALTY_DROP | Allows players from other guilds to pick up death penalty items    | False                                                                                        | True or False                          |
| ENABLE_NON_LOGIN_PENALTY                  | Enables no login penalty                                           | True                                                                                         | True or False                          |
| ENABLE_FAST_TRAVEL                        | Enables fast travel                                                | True                                                                                         | True or False                          |
| IS_START_LOCATION_SELECT_BY_MAP           | Enables starting location selection                                | True                                                                                         | True or False                          |
| EXIST_PLAYER_AFTER_LOGOUT                 | Toggle to delete players when they logout                          | False                                                                                        | True or False                          |
| ENABLE_DEFENSE_OTHER_GUILD_PLAYER         | Allows defense against other guild players                         | False                                                                                        | True or False                          |
| COOP_PLAYER_MAX_NUM                       | Maximum number of players in a guild                               | 4                                                                                            | Integer Number                         |
| REGION                                    | Region                                                             |                                                                                              | String                                 |
| USEAUTH                                   | Uses authentication                                                | True                                                                                         | True or False                          |
| BAN_LIST_URL                              | Which ban list to use                                              | [https://api.palworldgame.com/api/banlist.txt](https://api.palworldgame.com/api/banlist.txt) | string                                 |

### 2. List of Palworld Dedicated Server Admin Commands
These Palworld server admin commands are used within the game, through chat, allowing the admin to manage and control various aspects of the server in real time.

| Command                                   | Description                                                                                                                                                                                                                           |
|-------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /AdminPassword {Password}                 | Grants admin permission.                                                                                                                                                                                                              |
| /Shutdown {Seconds} {MessageText}         | Shuts down the server. You can set a time in seconds for the shutdown and an optional message to notify players on the server.                                                                                                       |
| /DoExit                                   | Shuts down the server immediately. Use only in case of technical problems, as some data may be lost.                                                                                                                                 |
| /Broadcast {MessageText}                  | Sends a message to all players on the server.                                                                                                                                                                                        |
| /KickPlayer {PlayerUID or SteamID}        | Kicks a player from the server.                                                                                                                                                                                                      |
| /BanPlayer {PlayerUID or SteamID}         | Bans a player from the server. The player will not be able to reconnect to the server until unbanned.                                                                                                                                |
| /TeleportToPlayer {PlayerUID or SteamID}  | Teleports you to a specific player.                                                                                                                                                                                                  |
| /TeleportToMe {PlayerUID or SteamID}      | Teleports a specific player to you.                                                                                                                                                                                                  |
| /ShowPlayers                              | Shows the list of all connected players.                                                                                                                                                                                             |
| /Info                                     | Shows server information.                                                                                                                                                                                                            |
| /Save                                     | Manually saves game data, such as player and creature progression.                                                                                                                                                                   |

---

## 💾 How to Automatically Backup and Restore Palworld Server Saves
This guide teaches how to perform automatic and manual backups of your Palworld dedicated server, as well as how to restore old saves.

> [!IMPORTANT]
> These commands will only work if you update your Docker image to the latest version: [How to Update the Docker Image](#-how-to-update-the-docker-image)

### 1. Create a Backup
  - To create a backup of the game save at the current moment, use the command:
    ```
    docker exec palworld-server backup
    ```
    The server will perform a save before the backup if RCON is enabled and will create a backup in ```/palworld/backups/```.
    
### 2. Restore from a Backup
  - To restore from a backup, use the command:
    ```
    docker exec -it palworld-server restore
    ```
    Now just select which backup you want to restore.

### 3. Manually Restore a Backup
  - Stop the server:
    ```
    docker compose down
    ```
  - Locate the backup you want to restore in ```/servidor/backups/``` and unzip the file.
  - Delete the folder with the current server settings: ```servidor/Pal/Saved/SaveGames/0/<old_hash>```.
  - Copy the contents of the backup folder ```Saved/SaveGames/0/<new_hash>``` to ```servidor/Pal/Saved/SaveGames/0/<new_hash>```.
  - Replace the ```DedicatedServerName``` inside ```servidor/Pal/Saved/Config/LinuxServer/GameUserSettings.ini``` with the new folder name.
    ```
    DedicatedServerName=<new_hash>  # Replace with your folder name.
    ```
  - Restart the server:
    ```
    docker compose up -d
    ```

> - To transfer the backup to your PC or another server, use this command in the backup folder:
>   ```
>   curl --upload-file <backup name> https://transfer.sh/<backup name>
>   ```
> - Copy the provided link and download the file on your PC or on the other server.

---

<div align="center">

**Developed by [Rafael Vieira](https://github.com/TechBeme)**

[![GitHub](https://img.shields.io/badge/GitHub-TechBeme-181717?logo=github)](https://github.com/TechBeme)
[![Fiverr](https://img.shields.io/badge/Fiverr-Tech__Be-1DBF73?logo=fiverr)](https://www.fiverr.com/tech_be)
[![Upwork](https://img.shields.io/badge/Upwork-Profile-14a800?logo=upwork)](https://www.upwork.com/freelancers/~01f0abcf70bbd95376)
[![Email](https://img.shields.io/badge/Email-contact@techbe.me-EA4335?logo=gmail)](mailto:contact@techbe.me)

</div>
