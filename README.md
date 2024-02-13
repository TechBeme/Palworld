# COMO CRIAR UM SERVIDOR DEDICADO DE PALWORLD
[![Crie seu Próprio Servidor Dedicado de Palworld GRÁTIS](https://github.com/TechBeme/Palworld/assets/101749351/970e4455-bc9e-4406-be1a-a43183c178d4)](https://youtu.be/ZXk4wE1rcXM)

# Sumário
- [Introdução](#introdução)
- [Passos para a Criação do Servidor](#passos-para-a-criação-do-servidor)
  - [Escolha do Serviço de Hospedagem](#1-escolha-do-serviço-de-hospedagem)
  - [Acesso e Configuração da Máquina Virtual](#2-acesso-e-configuração-da-máquina-virtual)
  - [Configuração do Servidor Palworld](#3-configuração-do-servidor-palworld)
  - [Acesso ao Servidor](#4-acesso-ao-servidor)
- [Como Atualizar o Servidor de Palworld](#como-atualizar-o-servidor-de-palworld)
- [Como Atualizar a Imagem do Docker](#como-atualizar-a-imagem-do-docker)
- [Como Alterar as Configurações do Servidor de Palworld](#como-alterar-as-configurações-do-servidor-de-palworld)
  - [Lista dos Parâmetros de Configuração do Servidor Dedicado do Palworld](#1-lista-dos-parâmetros-de-configuração-do-servidor-dedicado-do-palworld)
  - [Lista de Comandos de Administrador do Servidor Dedicado do Palworld](#2-lista-de-comandos-de-administrador-do-servidor-dedicado-do-palworld)
- [Como Fazer Backup Automático e Restaurar Saves do Servidor de Palworld](#como-fazer-backup-automático-e-restaurar-saves-do-servidor-de-palworld)

# INTRODUÇÃO
Criar um servidor dedicado para Palworld permite jogar com amigos a qualquer momento, sem depender do seu PC ou internet pessoal. Este guia aborda a criação de um servidor utilizando serviços de nuvem como Oracle Cloud, Google Cloud, AWS e Hostinger.

> [!WARNING]
> Atualmente, servidores dedicados estão disponíveis exclusivamente para usuários da Steam. Infelizmente, jogadores do Xbox Gamepass ou do Xbox Console não conseguem se conectar a servidores dedicados. Para esses jogadores, a única opção disponível é participar de sessões privadas por convite que comportam no máximo 4 jogadores.

# PASSOS PARA A CRIAÇÃO DO SERVIDOR
### 1. Escolha do Serviço de Hospedagem

| VPS | CPUs | Ram | Preço |
|-----|------|-----|-------|
| [Oracle Cloud](https://www.oracle.com/br/cloud/free) | 4 | 24 GB | Grátis |
| [Google Cloud](https://cloud.google.com) | X | X | 300$ de crédito grátis por 3 meses |
| [AWS](https://aws.amazon.com/pt/free) | X | X | Grátis por 1 ano |
| [Hostinger](https://hostinger.com.br?REFERRALCODE=1RFSV68) | 2 | 8 GB | R$34,99 |

### 2. Acesso e Configuração da Máquina Virtual
- **Crie uma Conta no serviço escolhido.**
- **Crie uma Nova Máquina Virtual (VM):**
   - **Localização:** Escolha a mais próxima para **menor latência** (exemplo: São Paulo, Brasil).
   - **Configurações:** Neste guia vou utilizar o Sistema Operacional **Ubuntu**.
- **Acesse a VM via SSH.**
- **Obtenha Acesso Root e Atualize a Máquina:**
   ```
   sudo su
   ```
   ```
   sudo apt update
   sudo apt upgrade
   ```
- **Instale o Docker:**
   - Siga as instruções do [site oficial](https://docs.docker.com/engine/install/) do Docker para instalação.
      - [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
      - [Debian](https://docs.docker.com/engine/install/debian/)
      - [Fedora](https://docs.docker.com/engine/install/fedora/)
      - [CentOS](https://docs.docker.com/engine/install/centos/)
      - [Raspberry Pi OS (32-bit)](https://docs.docker.com/engine/install/raspberry-pi-os/)
      - [RHEL (s390x)](https://docs.docker.com/engine/install/rhel/)
      - [SLES](https://docs.docker.com/engine/install/sles/)
      - [Binaries](https://docs.docker.com/engine/install/binaries/)
- **Verifique a instalação**:
  ```
  docker --version
  ```
   - **Caso apareça o erro "dial unix /var/run/docker.sock: connect: permission denied" significa que você não está como o usuário root.**
      - Basta adicionar o comando sudo antes de qualquer comando docker:
         ```
         sudo docker --version
         ```
      - ou trocar para o usuário root e executar os comandos normalmente:
         ```
         sudo su
         docker --version
         ```

### 3. Configuração do Servidor Palworld
- **Crie um arquivo chamado [docker-compose.yml](/docker-compose.yml).**
   ```
   sudo nano docker-compose.yml
   ```
- **Insira as configurações do servidor de sua preferência.**
   - Exemplo de configuração:
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
            - PLAYERS=16 # Número de vagas do servidor
            - MULTITHREADING=true
            - RCON_ENABLED=true
            - RCON_PORT=25575
            - TZ=UTC-3
            - COMMUNITY=true  # Mostra o seu servidor na listagem da comunidade.
            - SERVER_NAME=Nome do servidor
            - SERVER_DESCRIPTION=Descrição do servidor
            #- SERVER_PASSWORD= # Senha do servidor. (obs: remova o Hashtag antes de "- SERVER_PASSWORD" para utilizar a senha)
            - ADMIN_PASSWORD=admin # Senha do administrador.
            - DISCORD_WEBHOOK_URL= # Insira a sua webhook do discord, algo tipo: https://discord.com/api/webhooks/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
         volumes:
            - ./servidor:/palworld/
   ```
   - Defina a porta, número de jogadores, senha e outras configurações.
   - Acesse a documentação completa [aqui](https://github.com/thijsvanloef/palworld-server-docker).
- **Salve o Arquivo de Configuração.**
- **Crie o servidor:**
  > Atenção: Não utilize esse comando para iniciar o servidor, ele é apenas para criar o servidor.
  > Para iniciar utilize o comando ```sudo docker compose start```.
   ```
   sudo docker compose up
   ```
   - Pronto, agora basta aguardar o servidor iniciar completamente.

### 4. Acesso ao Servidor
- **Abra a porta no Firewall:**
   - Crie uma regra UDP para o jogo com a porta que você definiu (Padrão: 8211).
- **Obtenha o IP Externo da VM.**
- **Abra o Palworld e Conecte-se ao Servidor:**
   - Vá para multiplayer e use IP:PORTA para se conectar.

# COMO ATUALIZAR O SERVIDOR DE PALWORLD
[![Como ATUALIZAR seu Servidor Dedicado de Palworld para a Nova Versão](https://github.com/TechBeme/Palworld/assets/101749351/78e6a562-8420-418b-9a2b-5afa823fba92)](https://youtu.be/6tdbGYvDKOU)

Para atualizar o servidor para a versão mais recente do Palworld basta reiniciar o servidor.
Confira se você está como usuário root e se está no mesmo diretório do arquivo [docker-compose.yml](/docker-compose.yml).
* Reiniciar o servidor:
```
sudo docker compose restart
```

# COMO ATUALIZAR A IMAGEM DO DOCKER
### Para Usuários do Windows:
  - **Acesse a Interface Gráfica do Docker:**
    - Navegue até 'Containers' e exclua o container atual clicando no ícone da lixeira.
    - Em seguida, vá até 'Imagens', localize a imagem do servidor Palworld, clique nos três pontos e selecione 'Pull' para atualizar a imagem.

### Para Usuários do Linux ou Linha de Comando:
> **Comandos úteis:**
>   - Ver a lista de containers:
>   ```
>   docker container list
>   ```
>   - Encontrar o diretório do servidor:
>   ```
>   docker container inspect palworld-server -f '{{ json .Mounts }}'
>   ```
  1. Abra o terminal na pasta do Docker Compose.
  2. Pare e remova o container do servidor:
        ```
        docker stop palworld-server
        docker rm palworld-server
        ```
  3. Atualize a imagem do servidor:
        ```
        docker pull thijsvanloef/palworld-server-docker:latest
        ```
  4. Certifique-se de que seu arquivo [docker-compose.yml](/docker-compose.yml) está atualizado com as configurações mais recentes.
  5. Recrie o container com o comando:
        ```
        docker compose up -d
        ```

# COMO ALTERAR AS CONFIGURAÇÕES DO SERVIDOR DE PALWORLD
[![Como Alterar as Configurações do seu Servidor Dedicado de Palworld](https://github.com/TechBeme/Palworld/assets/101749351/243722b4-e1d2-425d-9735-55ac86222cfd)](https://youtu.be/GK9mh071Mzs)

Este guia explica como alterar várias configurações no seu servidor dedicado de Palworld, por exemplo, desativar o drop de itens ao morrer, reduzir o tempo de incubação de ovos e acelerar o crafting.
ATENÇÃO!!!
> [!WARNING]
> NÃO É MAIS NECESSÁRIO ALTERAR O ARQUIVO ```PalWorldSettings.ini```
>
> Agora é possível alterar as configurações diretamente no seu [docker-compose.yml](/docker-compose.yml)
> 
> ENTRETANTO, PRIMEIRO É  NECESSÁRIO ATUALIZAR SUA IMAGEM DO DOCKER CONFORME ESSE GUIA: [Como Atualizar a Imagem do Docker](#como-atualizar-a-imagem-do-docker)


### 1. Lista dos Parâmetros de Configuração do Servidor Dedicado do Palworld
Estes parâmetros DEMVEM SER ALTERADOR NO [docker-compose.yml](/docker-compose.yml) (NÃO É NO ```PalWorldSettings.ini```). Estas configurações permitem personalizar a experiência de jogo do servidor.

| parâmetros                                | Descrição                                                          | Valor Padrão                                                                                 | Valor Permitido                        |
|-------------------------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------|----------------------------------------|
| DIFFICULTY                                | Dificuldade do jogo                                                | None                                                                                         | `None`,`Normal`,`Difficult`            |
| DAYTIME_SPEEDRATE                         | Velocidade do dia - Números menores significam dias mais curtos    | 1.000000                                                                                     | Número Decimal                         |
| NIGHTTIME_SPEEDRATE                       | Velocidade da noite - Números menores significam noites mais curtas| 1.000000                                                                                     | Número Decimal                         |
| EXP_RATE                                  | Taxa de ganho de EXP                                               | 1.000000                                                                                     | Número Decimal                         |
| PAL_CAPTURE_RATE                          | Taxa de captura de Pal                                             | 1.000000                                                                                     | Número Decimal                         |
| PAL_SPAWN_NUM_RATE                        | Taxa de aparição de Pal                                            | 1.000000                                                                                     | Número Decimal                         |
| PAL_DAMAGE_RATE_ATTACK                    | Multiplicador de dano de Pals                                      | 1.000000                                                                                     | Número Decimal                         |
| PAL_DAMAGE_RATE_DEFENSE                   | Multiplicador de dano em Pals                                      | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_DAMAGE_RATE_ATTACK                 | Multiplicador de dano do jogador                                   | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_DAMAGE_RATE_DEFENSE                | Multiplicador de dano no jogador                                   | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_STOMACH_DECREASE_RATE              | Taxa de esgotamento da fome do jogador                             | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_STAMINA_DECREASE_RATE              | Taxa de redução de stamina do jogador                              | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_AUTO_HP_REGEN_RATE                 | Taxa de regeneração automática de HP do jogador                    | 1.000000                                                                                     | Número Decimal                         |
| PLAYER_AUTO_HP_REGEN_RATE_IN_SLEEP        | Taxa de regeneração de HP do jogador durante o sono                | 1.000000                                                                                     | Número Decimal                         |
| PAL_STOMACH_DECREASE_RATE                 | Taxa de esgotamento da fome de Pal                                 | 1.000000                                                                                     | Número Decimal                         |
| PAL_STAMINA_DECREASE_RATE                 | Taxa de redução de stamina de Pal                                  | 1.000000                                                                                     | Número Decimal                         |
| PAL_AUTO_HP_REGEN_RATE                    | Taxa de regeneração automática de HP de Pal                        | 1.000000                                                                                     | Número Decimal                         |
| PAL_AUTO_HP_REGEN_RATE_IN_SLEEP           | Taxa de regeneração de saúde de Pal durante o sono (em Palbox)     | 1.000000                                                                                     | Número Decimal                         |
| BUILD_OBJECT_DAMAGE_RATE                  | Multiplicador de dano em estruturas                                | 1.000000                                                                                     | Número Decimal                         |
| BUILD_OBJECT_DETERIORATION_DAMAGE_RATE    | Taxa de deterioração de estruturas                                 | 1.000000                                                                                     | Número Decimal                         |
| COLLECTION_DROP_RATE                      | Multiplicador de itens coletáveis                                  | 1.000000                                                                                     | Número Decimal                         |
| COLLECTION_OBJECT_HP_RATE                 | Multiplicador de HP de objetos coletáveis                          | 1.000000                                                                                     | Número Decimal                         |
| COLLECTION_OBJECT_RESPAWN_SPEED_RATE      | Intervalo de reaparecimento de objetos coletáveis - Quanto menor o número, mais rápida a regeneração | 1.000000                                                   | Número Decimal                         |
| ENEMY_DROP_ITEM_RATE                      | Multiplicador de itens largados por inimigos                       | 1.000000                                                                                     | Número Decimal                         |
| DEATH_PENALTY                             | Penalidade por morte</br>Nenhum: Sem penalidade por morte</br>Item: Larga itens exceto equipamentos</br>ItemAndEquipment: Larga todos os itens</br>All: Larga todos os PALs e todos os itens. | All        | `None`,`Item`,`ItemAndEquipment`,`All` |
| ENABLE_PLAYER_TO_PLAYER_DAMAGE            | Permite que jogadores causem dano a outros jogadores               | False                                                                                        | Verdadeiro ou Falso                    |
| ENABLE_FRIENDLY_FIRE                      | Permite fogo amigo                                                 | False                                                                                        | Verdadeiro ou Falso                    |
| ENABLE_INVADER_ENEMY                      | Habilita invasores                                                 | True                                                                                         | Verdadeiro ou Falso                    |
| ACTIVE_UNKO                               | Habilita UNKO (?)                                                  | False                                                                                        | Verdadeiro ou Falso                    |
| ENABLE_AIM_ASSIST_PAD                     | Habilita assistência de mira para controle                         | True                                                                                         | Verdadeiro ou Falso                    |
| ENABLE_AIM_ASSIST_KEYBOARD                | Habilita assistência de mira para teclado                          | False                                                                                        | Verdadeiro ou Falso                    |
| DROP_ITEM_MAX_NUM                         | Número máximo de itens soltos no mundo                             | 3000                                                                                         | Integer                                |
| DROP_ITEM_MAX_NUM_UNKO                    | Número máximo de solturas de UNKO no mundo                         | 100                                                                                          | Integer                                |
| BASE_CAMP_MAX_NUM                         | Número máximo de acampamentos base                                 | 128                                                                                          | Integer                                |
| BASE_CAMP_WORKER_MAX_NUM                  | Número máximo de trabalhadores                                     | 15                                                                                           | Integer                                |
| DROP_ITEM_ALIVE_MAX_HOURS                 | Tempo até os itens desaparecerem em horas                          | 1.000000                                                                                     | Número Decimal                         |
| AUTO_RESET_GUILD_NO_ONLINE_PLAYERS        | Reseta automaticamente a guilda quando nenhum jogador está online  | False                                                                                        | Verdadeiro ou Falso                    |
| AUTO_RESET_GUILD_TIME_NO_ONLINE_PLAYERS   | Tempo para resetar automaticamente a guilda quando nenhum jogador está online | 72.000000                                                                         | Número Decimal                         |
| GUILD_PLAYER_MAX_NUM                      | Número máximo de jogadores na Guilda                               | 20                                                                                           | Integer                                |
| PAL_EGG_DEFAULT_HATCHING_TIME             | Tempo (h) para incubar ovo massivo                                 | 72.000000                                                                                    | Número Decimal                         |
| WORK_SPEED_RATE                           | Multiplicador de velocidade de trabalho                            | 1.000000                                                                                     | Número Decimal                         |
| IS_MULTIPLAY                              | Habilita multijogador                                              | False                                                                                        | Verdadeiro ou Falso                    |
| IS_PVP                                    | Habilita PVP                                                       | False                                                                                        | Verdadeiro ou Falso                    |
| CAN_PICKUP_OTHER_GUILD_DEATH_PENALTY_DROP | Permite que jogadores de outras guildas peguem itens de penalidade por morte | False                                                                              | Verdadeiro ou Falso                    |
| ENABLE_NON_LOGIN_PENALTY                  | Habilita penalidade por não entrar                                 | True                                                                                         | Verdadeiro ou Falso                    |
| ENABLE_FAST_TRAVEL                        | Habilita viagem rápida                                             | True                                                                                         | Verdadeiro ou Falso                    |
| IS_START_LOCATION_SELECT_BY_MAP           | Habilita seleção de local de início                                | True                                                                                         | Verdadeiro ou Falso                    |
| EXIST_PLAYER_AFTER_LOGOUT                 | Alternância para deletar jogadores quando eles saem                | False                                                                                        | Verdadeiro ou Falso                    |
| ENABLE_DEFENSE_OTHER_GUILD_PLAYER         | Permite defesa contra jogadores de outras guildas                  | False                                                                                        | Verdadeiro ou Falso                    |
| COOP_PLAYER_MAX_NUM                       | Número máximo de jogadores em uma guilda                           | 4                                                                                            | Integer                                |
| REGION                                    | Região                                                             |                                                                                              | String                                 |
| USEAUTH                                   | Usa autenticação                                                   | True                                                                                         | Verdadeiro ou Falso                    |
| BAN_LIST_URL                              | Qual lista de banimento usar                                       | [https://api.palworldgame.com/api/banlist.txt](https://api.palworldgame.com/api/banlist.txt) | string                                 |

### 2. Lista de Comandos de Administrador do Servidor Dedicado do Palworld
Estes comandos de administração do servidor de Palworld são utilizados dentro do jogo, através do chat, permitindo ao administrador gerenciar e controlar diversos aspectos do servidor em tempo real.

| Comando                                   | Descrição                                                                                                                                                                                                                             |
|-------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /AdminPassword {Senha}                    | Concede permissão de administrador.                                                                                                                                                                                                   |
| /Shutdown {Segundos} {TextoMensagem}      | Desliga o servidor. Você pode definir um tempo em segundos para o desligamento e uma mensagem opcional para notificar os jogadores no servidor.                                                                                       |
| /DoExit                                   | Desliga o servidor imediatamente. Usar apenas em caso de problemas técnicos, pois alguns dados podem ser perdidos.                                                                                                                    |
| /Broadcast {TextoMensagem}                | Envia uma mensagem para todos os jogadores no servidor.                                                                                                                                                                               |
| /KickPlayer {UIDJogador ou SteamID}       | Expulsa um jogador do servidor.                                                                                                                                                                                                       |
| /BanPlayer {UIDJogador ou SteamID}        | Bane um jogador do servidor. O jogador não poderá se reconectar ao servidor até ser desbanido.                                                                                                                                        |
| /TeleportToPlayer {UIDJogador ou SteamID} | Teleporta você até um jogador específico.                                                                                                                                                                                             |
| /TeleportToMe {UIDJogador ou SteamID}     | Teleporta um jogador específico até você.                                                                                                                                                                                             |
| /ShowPlayers                              | Exibe a lista de todos os jogadores conectados.                                                                                                                                                                                       |
| /Info                                     | Mostra informações do servidor.                                                                                                                                                                                                       |
| /Save                                     | Salva manualmente os dados do jogo, como a progressão dos jogadores e criaturas.                                                                                                                                                      |



# COMO FAZER BACKUP AUTOMÁTICO E RESTAURAR SAVES DO SERVIDOR DE PALWORLD
Este guia ensina como realizar backups automáticos e manuais do seu servidor dedicado de Palworld, além de como restaurar saves antigos.
> [!IMPORTANT]
> Estes comandos só irão funcionar se você atualizar sua imagem do Docker para a versão mais recente: [Como Atualizar a Imagem do Docker](#como-atualizar-a-imagem-do-docker)

### 1. Criar um Backup
  - Para criar um backup do save do jogo no momento atual, use o comando:
    ```
    docker exec palworld-server backup
    ```
    O servidor executará um save antes do backup se o RCON estiver habilitado e criará um backup em ```/palworld/backups/```.
    
### 2. Restaurar de um Backup
  - Para restaurar a partir de um backup, use o comando:
    ```
    docker exec -it palworld-server restore
    ```
    Agora basta selecionar qual backup você quer restaurar.

### 3. Restaurar um Backup Manualmente
  - Pare o servidor:
    ```
    docker compose down
    ```
  - Localize o backup que deseja restaurar em ```/servdior/backups/``` e descompacte o arquivo.
  - Delete a pasta com as configurações atuais do servidor: ```servidor/Pal/Saved/SaveGames/0/<hash_antigo>```.
  - Copie o conteúdo da pasta de backup ```Saved/SaveGames/0/<hash_novo>``` para ```servidor/Pal/Saved/SaveGames/0/<hash_novo>```.
  - Substitua o ```DedicatedServerName``` dentro de ```servidor/Pal/Saved/Config/LinuxServer/GameUserSettings.ini``` pelo novo nome da pasta.
    ```
    DedicatedServerName=<hash_novo>  # Substitua pelo nome da sua pasta.
    ```
  - Reinicie o servidor:
    ```
    docker compose up -d
    ```

> - Para transferir o backup para o seu PC ou outro servidor basta utlizar este comando na pasta de backup:
>   ```
>   curl --upload-file <nome do backup> https://transfer.sh/<nome do backup>
>   ```
> - Copie o link fornecido e baixe o arquivo no seu PC ou no outro servidor.
