<div align="center">

# 🎮 Servidor Dedicado de Palworld con Docker

**Guía completa para crear y administrar tu propio servidor dedicado de Palworld gratis**

[![Docker](https://img.shields.io/badge/Docker-Latest-2496ED?logo=docker)](https://www.docker.com/)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-Latest-2496ED?logo=docker)](https://docs.docker.com/compose/)
[![Palworld](https://img.shields.io/badge/Palworld-Server-orange)](https://www.palworld.gg/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-Compatible-E95420?logo=ubuntu)](https://ubuntu.com/)

[![Crea tu Propio Servidor Dedicado de Palworld GRATIS](https://github.com/TechBeme/Palworld/assets/101749351/970e4455-bc9e-4406-be1a-a43183c178d4)](https://youtu.be/ZXk4wE1rcXM)

**Idiomas:** [🇧🇷 Português](README.md) • [🇺🇸 English](README.en.md)

</div>

---

## 📋 Tabla de Contenidos
- [📖 Introducción](#-introducción)
- [🚀 Pasos para Crear el Servidor](#-pasos-para-crear-el-servidor)
  - [Elección del Servicio de Alojamiento](#1-elección-del-servicio-de-alojamiento)
  - [Acceso y Configuración de la Máquina Virtual](#2-acceso-y-configuración-de-la-máquina-virtual)
  - [Configuración del Servidor Palworld](#3-configuración-del-servidor-palworld)
  - [Acceso al Servidor](#4-acceso-al-servidor)
- [🔄 Cómo Actualizar el Servidor de Palworld](#-cómo-actualizar-el-servidor-de-palworld)
- [🐳 Cómo Actualizar la Imagen de Docker](#-cómo-actualizar-la-imagen-de-docker)
- [⚙️ Cómo Cambiar la Configuración del Servidor de Palworld](#%EF%B8%8F-cómo-cambiar-la-configuración-del-servidor-de-palworld)
  - [Lista de Parámetros de Configuración del Servidor Dedicado de Palworld](#1-lista-de-parámetros-de-configuración-del-servidor-dedicado-de-palworld)
  - [Lista de Comandos de Administrador del Servidor Dedicado de Palworld](#2-lista-de-comandos-de-administrador-del-servidor-dedicado-de-palworld)
- [💾 Cómo Hacer Copias de Seguridad Automáticas y Restaurar Guardados del Servidor de Palworld](#-cómo-hacer-copias-de-seguridad-automáticas-y-restaurar-guardados-del-servidor-de-palworld)

---

## 📖 Introducción
Crear un servidor dedicado para Palworld te permite jugar con amigos en cualquier momento, sin depender de tu PC o internet personal. Esta guía cubre la creación de un servidor utilizando servicios en la nube como Oracle Cloud, Google Cloud, AWS y Hostinger.

> [!WARNING]
> Actualmente, los servidores dedicados están disponibles exclusivamente para usuarios de Steam. Desafortunadamente, los jugadores de Xbox Gamepass o Xbox Console no pueden conectarse a servidores dedicados. Para estos jugadores, la única opción disponible es unirse a sesiones privadas por invitación que admiten un máximo de 4 jugadores.

## 🚀 Pasos para Crear el Servidor
### 1. Elección del Servicio de Alojamiento

| VPS | CPUs | Ram | Precio |
|-----|------|-----|--------|
| [Oracle Cloud](https://www.oracle.com/cloud/free) | 4 | 24 GB | Gratis |
| [Google Cloud](https://cloud.google.com) | X | X | $300 de crédito gratis por 3 meses |
| [AWS](https://aws.amazon.com/free) | X | X | Gratis por 1 año |
| [Hostinger](https://hostinger.com?REFERRALCODE=1RFSV68) | 2 | 8 GB | $34.99 |

### 2. Acceso y Configuración de la Máquina Virtual
- **Crea una Cuenta en el servicio elegido.**
- **Crea una Nueva Máquina Virtual (VM):**
   - **Ubicación:** Elige la más cercana para **menor latencia** (ejemplo: São Paulo, Brasil).
   - **Configuración:** En esta guía usaré el Sistema Operativo **Ubuntu**.
- **Accede a la VM vía SSH.**
- **Obtén Acceso Root y Actualiza la Máquina:**
   ```
   sudo su
   ```
   ```
   sudo apt update
   sudo apt upgrade
   ```
- **Instala Docker:**
   - Sigue las instrucciones del [sitio oficial](https://docs.docker.com/engine/install/) de Docker para la instalación.
      - [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
      - [Debian](https://docs.docker.com/engine/install/debian/)
      - [Fedora](https://docs.docker.com/engine/install/fedora/)
      - [CentOS](https://docs.docker.com/engine/install/centos/)
      - [Raspberry Pi OS (32-bit)](https://docs.docker.com/engine/install/raspberry-pi-os/)
      - [RHEL (s390x)](https://docs.docker.com/engine/install/rhel/)
      - [SLES](https://docs.docker.com/engine/install/sles/)
      - [Binaries](https://docs.docker.com/engine/install/binaries/)
- **Verifica la instalación**:
  ```
  docker --version
  ```
   - **Si aparece el error "dial unix /var/run/docker.sock: connect: permission denied" significa que no estás como usuario root.**
      - Simplemente añade el comando sudo antes de cualquier comando docker:
         ```
         sudo docker --version
         ```
      - o cambia al usuario root y ejecuta los comandos normalmente:
         ```
         sudo su
         docker --version
         ```

### 3. Configuración del Servidor Palworld
- **Crea un archivo llamado [docker-compose.yml](/docker-compose.yml).**
   ```
   sudo nano docker-compose.yml
   ```
- **Inserta la configuración del servidor de tu preferencia.**
   - Ejemplo de configuración:
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
             - PLAYERS=16 # Número de espacios del servidor
             - MULTITHREADING=true
             - RCON_ENABLED=true
             - RCON_PORT=25575
             - TZ=UTC-3
             - COMMUNITY=true  # Muestra tu servidor en la lista de la comunidad.
             - SERVER_NAME=Nombre del servidor
             - SERVER_DESCRIPTION=Descripción del servidor
             #- SERVER_PASSWORD= # Contraseña del servidor. (nota: elimina el Hashtag antes de "- SERVER_PASSWORD" para usar contraseña)
             - ADMIN_PASSWORD=admin # Contraseña del administrador.
             # Si quieres editar la configuración del servidor, añade los comandos abajo: 
             - DEATH_PENALTY=Item #Penalización por muerte: None: Sin penalización por muerte; Item: Suelta objetos excepto equipamiento; ItemAndEquipment: Suelta todos los objetos; All: Suelta todos los PALs y todos los objetos.
             - ENABLE_INVADER_ENEMY=True # Activar/desactivar invasiones
             - PAL_EGG_DEFAULT_HATCHING_TIME=1 # Tiempo en horas para incubar los huevos.
             - DISCORD_WEBHOOK_URL= # Inserta tu webhook de discord, algo como: https://discord.com/api/webhooks/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
             - DISCORD_PRE_UPDATE_BOOT_MESSAGE=En fase de expansión... ¡El pal está aumentando su poder!
             - DISCORD_POST_UPDATE_BOOT_MESSAGE=¡Actualización completa! El pal está listo, más fuerte que nunca.
             - DISCORD_PRE_START_MESSAGE=¡El pal despertó! Listo para la caza, firme y fuerte.
             - DISCORD_PRE_SHUTDOWN_MESSAGE=Apagando... El pal está exhausto, necesita tiempo.
             - DISCORD_POST_SHUTDOWN_MESSAGE=¡Aguanta que el pal se está cayendo... se fue!
          volumes:
             - ./servidor:/palworld/
   ```
   - Define el puerto, número de jugadores, contraseña y otras configuraciones.
   - Accede a la documentación completa [aquí](https://github.com/thijsvanloef/palworld-server-docker).
- **Guarda el Archivo de Configuración.**
- **Crea el servidor:**
   ```
   sudo docker compose up
   ```
   - Listo, ahora solo espera a que el servidor inicie completamente.

### 4. Acceso al Servidor
- **Abre el puerto en el Firewall:**
   - Crea una regla UDP para el juego con el puerto que definiste (Predeterminado: 8211).
- **Obtén la IP Externa de la VM.**
- **Abre Palworld y Conéctate al Servidor:**
   - Ve a multijugador y usa IP:PUERTO para conectarte.

---

## 🔄 Cómo Actualizar el Servidor de Palworld
[![Cómo ACTUALIZAR tu Servidor Dedicado de Palworld a la Nueva Versión](https://github.com/TechBeme/Palworld/assets/101749351/78e6a562-8420-418b-9a2b-5afa823fba92)](https://youtu.be/6tdbGYvDKOU)

Para actualizar el servidor a la versión más reciente de Palworld, simplemente reinicia el servidor.
Verifica que estés como usuario root y en el mismo directorio que el archivo [docker-compose.yml](/docker-compose.yml).
* Reiniciar el servidor:
```
sudo docker compose restart
```

---

## 🐳 Cómo Actualizar la Imagen de Docker
### Para Usuarios de Windows:
  - **Accede a la Interfaz Gráfica de Docker:**
    - Navega hasta 'Containers' y elimina el contenedor actual haciendo clic en el ícono de la papelera.
    - Luego, ve a 'Images', localiza la imagen del servidor Palworld, haz clic en los tres puntos y selecciona 'Pull' para actualizar la imagen.

### Para Usuarios de Linux o Línea de Comandos:
> **Comandos útiles:**
>   - Ver la lista de contenedores:
>   ```
>   docker container list
>   ```
>   - Encontrar el directorio del servidor:
>   ```
>   docker container inspect palworld-server -f '{{ json .Mounts }}'
>   ```
  1. Abre la terminal en la carpeta de Docker Compose.
  2. Detén y elimina el contenedor del servidor:
        ```
        docker stop palworld-server
        docker rm palworld-server
        ```
  3. Actualiza la imagen del servidor:
        ```
        docker pull thijsvanloef/palworld-server-docker:latest
        ```
  4. Asegúrate de que tu archivo [docker-compose.yml](/docker-compose.yml) esté actualizado con la configuración más reciente.
  5. Recrea el contenedor con el comando:
        ```
        docker compose up -d
        ```

---

## ⚙️ Cómo Cambiar la Configuración del Servidor de Palworld
[![Cómo Cambiar la Configuración de tu Servidor Dedicado de Palworld](https://github.com/TechBeme/Palworld/assets/101749351/243722b4-e1d2-425d-9735-55ac86222cfd)](https://youtu.be/PsjYGGFpaqo)

Esta guía explica cómo cambiar varias configuraciones en tu servidor dedicado de Palworld, por ejemplo, desactivar la pérdida de objetos al morir, reducir el tiempo de incubación de huevos y acelerar el crafteo.

> [!WARNING]
> YA NO ES NECESARIO MODIFICAR EL ARCHIVO ```PalWorldSettings.ini```
>
> Ahora es posible cambiar la configuración directamente en tu [docker-compose.yml](/docker-compose.yml)
> 
> SIN EMBARGO, PRIMERO DEBES ACTUALIZAR TU IMAGEN DE DOCKER SEGÚN ESTA GUÍA: [Cómo Actualizar la Imagen de Docker](#-cómo-actualizar-la-imagen-de-docker)


### 1. Lista de Parámetros de Configuración del Servidor Dedicado de Palworld
Estos parámetros DEBEN SER CAMBIADOS EN [docker-compose.yml](/docker-compose.yml) (NO EN ```PalWorldSettings.ini```). Estas configuraciones permiten personalizar la experiencia de juego del servidor.

> [!WARNING]
> Estos son todos los comandos que existen en los archivos del juego, pero no todos están funcionando.
> ¡El juego aún no los ha implementado, no es un problema del servidor!

| parámetros                                | Descripción                                                        | Valor Predeterminado                                                                         | Valor Permitido                        |
|-------------------------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------|----------------------------------------|
| DIFFICULTY                                | Dificultad del juego                                               | None                                                                                         | `None`,`Normal`,`Difficult`            |
| DAYTIME_SPEEDRATE                         | Velocidad del día - Números menores significan días más cortos     | 1.000000                                                                                     | Número Decimal                         |
| NIGHTTIME_SPEEDRATE                       | Velocidad de la noche - Números menores significan noches más cortas| 1.000000                                                                                    | Número Decimal                         |
| EXP_RATE                                  | Tasa de ganancia de EXP                                            | 1.000000                                                                                     | Número Decimal                         |
| PAL_CAPTURE_RATE                          | Tasa de captura de Pal                                             | 1.000000                                                                                     | Número Decimal                         |
| PAL_SPAWN_NUM_RATE                        | Tasa de aparición de Pal                                           | 1.000000                                                                                     | Número Decimal                         |
| PAL_DAMAGE_RATE_ATTACK                    | Multiplicador de daño de Pals                                      | 1.000000                                                                                     | Número Decimal                         |
| PAL_DAMAGE_RATE_DEFENSE                   | Multiplicador de daño a Pals                                       | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_DAMAGE_RATE_ATTACK                 | Multiplicador de daño del jugador                                  | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_DAMAGE_RATE_DEFENSE                | Multiplicador de daño al jugador                                   | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_STOMACH_DECREASE_RATE              | Tasa de agotamiento del hambre del jugador                         | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_STAMINA_DECREASE_RATE              | Tasa de reducción de resistencia del jugador                       | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_AUTO_HP_REGEN_RATE                 | Tasa de regeneración automática de HP del jugador                  | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_AUTO_HP_REGEN_RATE_IN_SLEEP        | Tasa de regeneración de HP del jugador durante el sueño            | 1.000000                                                                                     | Número Decimal                         |
| PAL_STOMACH_DECREASE_RATE                 | Tasa de agotamiento del hambre de Pal                              | 1.000000                                                                                     | Número Decimal                         |
| PAL_STAMINA_DECREASE_RATE                 | Tasa de reducción de resistencia de Pal                            | 1.000000                                                                                     | Número Decimal                         |
| PAL_AUTO_HP_REGEN_RATE                    | Tasa de regeneración automática de HP de Pal                       | 1.000000                                                                                     | Número Decimal                         |
| PAL_AUTO_HP_REGEN_RATE_IN_SLEEP           | Tasa de regeneración de salud de Pal durante el sueño (en Palbox)  | 1.000000                                                                                     | Número Decimal                         |
| BUILD_OBJECT_DAMAGE_RATE                  | Multiplicador de daño a estructuras                                | 1.000000                                                                                     | Número Decimal                         |
| BUILD_OBJECT_DETERIORATION_DAMAGE_RATE    | Tasa de deterioro de estructuras                                   | 1.000000                                                                                     | Número Decimal                         |
| COLLECTION_DROP_RATE                      | Multiplicador de objetos coleccionables                            | 1.000000                                                                                     | Número Decimal                         |
| COLLECTION_OBJECT_HP_RATE                 | Multiplicador de HP de objetos coleccionables                      | 1.000000                                                                                     | Número Decimal                         |
| COLLECTION_OBJECT_RESPAWN_SPEED_RATE      | Intervalo de reaparición de objetos coleccionables - Cuanto menor el número, más rápida la regeneración | 1.000000                              | Número Decimal                         |
| ENEMY_DROP_ITEM_RATE                      | Multiplicador de objetos soltados por enemigos                     | 1.000000                                                                                     | Número Decimal                         |
| DEATH_PENALTY                             | Penalización por muerte</br>None: Sin penalización por muerte</br>Item: Suelta objetos excepto equipamiento</br>ItemAndEquipment: Suelta todos los objetos</br>All: Suelta todos los PALs y todos los objetos. | All        | `None`,`Item`,`ItemAndEquipment`,`All` |
| ENABLE_PLAYER_TO_PLAYER_DAMAGE            | Permite que los jugadores causen daño a otros jugadores            | False                                                                                        | Verdadero o Falso                      |
| ENABLE_FRIENDLY_FIRE                      | Permite fuego amigo                                                | False                                                                                        | Verdadero o Falso                      |
| ENABLE_INVADER_ENEMY                      | Habilita invasores                                                 | True                                                                                         | Verdadero o Falso                      |
| ACTIVE_UNKO                               | Habilita UNKO (?)                                                  | False                                                                                        | Verdadero o Falso                      |
| ENABLE_AIM_ASSIST_PAD                     | Habilita asistencia de puntería para control                       | True                                                                                         | Verdadero o Falso                      |
| ENABLE_AIM_ASSIST_KEYBOARD                | Habilita asistencia de puntería para teclado                       | False                                                                                        | Verdadero o Falso                      |
| DROP_ITEM_MAX_NUM                         | Número máximo de objetos soltados en el mundo                      | 3000                                                                                         | Número Entero                          |
| DROP_ITEM_MAX_NUM_UNKO                    | Número máximo de solturas de UNKO en el mundo                      | 100                                                                                          | Número Entero                          |
| BASE_CAMP_MAX_NUM                         | Número máximo de campamentos base                                  | 128                                                                                          | Número Entero                          |
| BASE_CAMP_WORKER_MAX_NUM                  | Número máximo de trabajadores                                      | 15                                                                                           | Número Entero                          |
| DROP_ITEM_ALIVE_MAX_HOURS                 | Tiempo hasta que los objetos desaparezcan en horas                 | 1.000000                                                                                     | Número Decimal                         |
| AUTO_RESET_GUILD_NO_ONLINE_PLAYERS        | Reinicia automáticamente el gremio cuando no hay jugadores en línea| False                                                                                       | Verdadero o Falso                      |
| AUTO_RESET_GUILD_TIME_NO_ONLINE_PLAYERS   | Tiempo para reiniciar automáticamente el gremio cuando no hay jugadores en línea | 72.000000                                                        | Número Decimal                         |
| GUILD_PLAYER_MAX_NUM                      | Número máximo de jugadores en el Gremio                            | 20                                                                                           | Número Entero                          |
| PAL_EGG_DEFAULT_HATCHING_TIME             | Tiempo (h) para incubar huevo masivo                               | 72.000000                                                                                    | Número Decimal                         |
| WORK_SPEED_RATE                           | Multiplicador de velocidad de trabajo                              | 1.000000                                                                                     | Número Decimal                         |
| IS_MULTIPLAY                              | Habilita multijugador                                              | False                                                                                        | Verdadero o Falso                      |
| IS_PVP                                    | Habilita PVP                                                       | False                                                                                        | Verdadero o Falso                      |
| CAN_PICKUP_OTHER_GUILD_DEATH_PENALTY_DROP | Permite que jugadores de otros gremios recojan objetos de penalización por muerte | False                                                           | Verdadero o Falso                      |
| ENABLE_NON_LOGIN_PENALTY                  | Habilita penalización por no entrar                                | True                                                                                         | Verdadero o Falso                      |
| ENABLE_FAST_TRAVEL                        | Habilita viaje rápido                                              | True                                                                                         | Verdadero o Falso                      |
| IS_START_LOCATION_SELECT_BY_MAP           | Habilita selección de ubicación de inicio                          | True                                                                                         | Verdadero o Falso                      |
| EXIST_PLAYER_AFTER_LOGOUT                 | Alternar para eliminar jugadores cuando se desconectan             | False                                                                                        | Verdadero o Falso                      |
| ENABLE_DEFENSE_OTHER_GUILD_PLAYER         | Permite defensa contra jugadores de otros gremios                  | False                                                                                        | Verdadero o Falso                      |
| COOP_PLAYER_MAX_NUM                       | Número máximo de jugadores en un gremio                            | 4                                                                                            | Número Entero                          |
| REGION                                    | Región                                                             |                                                                                              | String                                 |
| USEAUTH                                   | Usa autenticación                                                  | True                                                                                         | Verdadero o Falso                      |
| BAN_LIST_URL                              | Qué lista de baneos usar                                           | [https://api.palworldgame.com/api/banlist.txt](https://api.palworldgame.com/api/banlist.txt) | string                                 |

### 2. Lista de Comandos de Administrador del Servidor Dedicado de Palworld
Estos comandos de administración del servidor de Palworld se usan dentro del juego, a través del chat, permitiendo al administrador gestionar y controlar varios aspectos del servidor en tiempo real.

| Comando                                   | Descripción                                                                                                                                                                                                                           |
|-------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /AdminPassword {Contraseña}               | Otorga permiso de administrador.                                                                                                                                                                                                      |
| /Shutdown {Segundos} {TextoMensaje}       | Apaga el servidor. Puedes definir un tiempo en segundos para el apagado y un mensaje opcional para notificar a los jugadores en el servidor.                                                                                         |
| /DoExit                                   | Apaga el servidor inmediatamente. Usar solo en caso de problemas técnicos, ya que algunos datos pueden perderse.                                                                                                                     |
| /Broadcast {TextoMensaje}                 | Envía un mensaje a todos los jugadores en el servidor.                                                                                                                                                                               |
| /KickPlayer {UIDJugador o SteamID}        | Expulsa un jugador del servidor.                                                                                                                                                                                                     |
| /BanPlayer {UIDJugador o SteamID}         | Banea a un jugador del servidor. El jugador no podrá reconectarse al servidor hasta ser desbaneado.                                                                                                                                  |
| /TeleportToPlayer {UIDJugador o SteamID}  | Te teletransporta a un jugador específico.                                                                                                                                                                                           |
| /TeleportToMe {UIDJugador o SteamID}      | Teletransporta a un jugador específico hacia ti.                                                                                                                                                                                     |
| /ShowPlayers                              | Muestra la lista de todos los jugadores conectados.                                                                                                                                                                                  |
| /Info                                     | Muestra información del servidor.                                                                                                                                                                                                    |
| /Save                                     | Guarda manualmente los datos del juego, como la progresión de jugadores y criaturas.                                                                                                                                                 |

---

## 💾 Cómo Hacer Copias de Seguridad Automáticas y Restaurar Guardados del Servidor de Palworld
Esta guía enseña cómo realizar copias de seguridad automáticas y manuales de tu servidor dedicado de Palworld, además de cómo restaurar guardados antiguos.

> [!IMPORTANT]
> Estos comandos solo funcionarán si actualizas tu imagen de Docker a la versión más reciente: [Cómo Actualizar la Imagen de Docker](#-cómo-actualizar-la-imagen-de-docker)

### 1. Crear una Copia de Seguridad
  - Para crear una copia de seguridad del guardado del juego en el momento actual, usa el comando:
    ```
    docker exec palworld-server backup
    ```
    El servidor ejecutará un guardado antes de la copia de seguridad si RCON está habilitado y creará una copia de seguridad en ```/palworld/backups/```.
    
### 2. Restaurar desde una Copia de Seguridad
  - Para restaurar desde una copia de seguridad, usa el comando:
    ```
    docker exec -it palworld-server restore
    ```
    Ahora solo selecciona qué copia de seguridad quieres restaurar.

### 3. Restaurar una Copia de Seguridad Manualmente
  - Detén el servidor:
    ```
    docker compose down
    ```
  - Localiza la copia de seguridad que deseas restaurar en ```/servidor/backups/``` y descomprime el archivo.
  - Elimina la carpeta con la configuración actual del servidor: ```servidor/Pal/Saved/SaveGames/0/<hash_antiguo>```.
  - Copia el contenido de la carpeta de copia de seguridad ```Saved/SaveGames/0/<hash_nuevo>``` a ```servidor/Pal/Saved/SaveGames/0/<hash_nuevo>```.
  - Reemplaza el ```DedicatedServerName``` dentro de ```servidor/Pal/Saved/Config/LinuxServer/GameUserSettings.ini``` con el nuevo nombre de carpeta.
    ```
    DedicatedServerName=<hash_nuevo>  # Reemplaza con el nombre de tu carpeta.
    ```
  - Reinicia el servidor:
    ```
    docker compose up -d
    ```

> - Para transferir la copia de seguridad a tu PC u otro servidor, usa este comando en la carpeta de copia de seguridad:
>   ```
>   curl --upload-file <nombre de la copia de seguridad> https://transfer.sh/<nombre de la copia de seguridad>
>   ```
> - Copia el enlace proporcionado y descarga el archivo en tu PC o en el otro servidor.

---

<div align="center">

**Developed by [Rafael Vieira](https://github.com/TechBeme)**

[![GitHub](https://img.shields.io/badge/GitHub-TechBeme-181717?logo=github)](https://github.com/TechBeme)
[![Fiverr](https://img.shields.io/badge/Fiverr-Tech__Be-1DBF73?logo=fiverr)](https://www.fiverr.com/tech_be)
[![Upwork](https://img.shields.io/badge/Upwork-Profile-14a800?logo=upwork)](https://www.upwork.com/freelancers/~01f0abcf70bbd95376)
[![Email](https://img.shields.io/badge/Email-contact@techbe.me-EA4335?logo=gmail)](mailto:contact@techbe.me)

</div>
