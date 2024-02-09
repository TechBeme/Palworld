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
- [Como Alterar as Configurações do Servidor de Palworld](#como-alterar-as-configurações-do-servidor-de-palworld)
  - [Método 1](#método-1)
  - [Método 2](#método-2)
  - [Lista dos Parâmetros de Configuração do Servidor Dedicado do Palworld](#1-lista-dos-parâmetros-de-configuração-do-servidor-dedicado-do-palworld)
  - [Lista de Comandos de Administrador do Servidor Dedicado do Palworld](#2-lista-de-comandos-de-administrador-do-servidor-dedicado-do-palworld)

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

# COMO ALTERAR AS CONFIGURAÇÕES DO SERVIDOR DE PALWORLD
[![Como Alterar as Configurações do seu Servidor Dedicado de Palworld](https://github.com/TechBeme/Palworld/assets/101749351/243722b4-e1d2-425d-9735-55ac86222cfd)](https://youtu.be/GK9mh071Mzs)

Este guia explica como alterar várias configurações no seu servidor dedicado de Palworld, por exemplo, desativar o drop de itens ao morrer, reduzir o tempo de incubação de ovos e acelerar o crafting.

> [!WARNING]
> Atenção, qualquer parâmetro definido na seção environment do arquivo [docker-compose.yml](/docker-compose.yml) terá prioridade sobre as configurações presentes no arquivo PalWorldSettings.ini. Isso significa que, se uma configuração específica for definida tanto no Docker quanto no arquivo PalWorldSettings.ini, a configuração do Docker Compose será a que prevalecerá no servidor. Caso prefira gerenciar as configurações diretamente pelo arquivo PalWorldSettings.ini, é importante remover ou comentar as linhas correspondentes na seção environment do seu docker-compose.yml.

### Método 1:
- Acesse a pasta do seu servidor (se você criou o servidor na versão mais atual do [docker-compose.yml](/docker-compose.yml) ela irá chamar "servidor", caso contrário será "palworld").
- Copie o conteúdo do arquivo DefaultPalWorldSettings.ini para PalWorldSettings.ini com o comando:
   ```
   sudo cp DefaultPalWorldSettings.ini Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
   ```
- Abra o arquivo de Configuração com:
   ```
   sudo nano Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
   ```
- O arquivo PalWorldSettings.ini agora contém as configurações padrão. Altere conforme sua preferência.
- Para salvar basta pressionar "Ctrl + X", digitar "y" e pressionar "Enter".
- Após fazer as alterações, reinicie o servidor para que as novas configurações tenham efeito.
   ```
   sudo docker compose restart
   ```
### Método 2:
- Acesse a pasta do seu servidor (se você criou o servidor na versão mais atual do [docker-compose.yml](/docker-compose.yml) ela irá chamar "servidor", caso contrário será "palworld").
- Faça o upload do arquivo DefaultPalWorldSettings.ini para a nuvem:
   ```
   curl --upload-file DefaultPalWorldSettings.ini https://transfer.sh/DefaultPalWorldSettings.ini
   ```
- Copie o link fornecido e baixe o arquivo no seu PC.
- Edite o arquivo no seu PC (por exemplo, com o Notepad) e copie o conteúdo editado.
- Remove o arquivo PalWorldSettings.ini do seu servidor e crie um novo com os comandos:
   ```
   sudo rm Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
   sudo nano Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
   ```
- Cole o código que você alterou no seu PC.
- Para salvar basta pressionar "Ctrl + X", digitar "y" e pressionar "Enter".
- Após fazer as alterações, reinicie o servidor para que as novas configurações tenham efeito.
   ```
   sudo docker compose restart
   ```
### 1. Lista dos Parâmetros de Configuração do Servidor Dedicado do Palworld
Estes parâmetros devem ser alterados nos arquivos do servidor, especificamente no arquivo PalWorldSettings.ini. Estas configurações permitem personalizar a experiência de jogo do servidor.


| Parâmetro                                | Descrição                                                                                          |
|------------------------------------------|----------------------------------------------------------------------------------------------------|
| Difficulty                               | Ajusta a dificuldade geral do jogo.                                                                |
| DayTimeSpeedRate                         | Modifica a velocidade do tempo in-game durante o dia.                                              |
| NightTimeSpeedRate                       | Modifica a velocidade do tempo in-game durante a noite.                                            |
| ExpRate                                  | Altera a taxa de ganho de experiência para jogadores e criaturas.                                  |
| PalCaptureRate                           | Ajusta a taxa na qual criaturas Pal podem ser capturadas.                                          |
| PalSpawnNumRate                          | Ajusta a taxa na qual criaturas Pal aparecem.                                                      |
| PalDamageRateAttack                      | Ajusta o dano causado por criaturas Pal.                                                           |
| PalDamageRateDefense                     | Ajusta o dano recebido por criaturas Pal.                                                          |
| PlayerDamageRateAttack                   | Ajusta o dano causado pelos jogadores.                                                             |
| PlayerDamageRateDefense                  | Ajusta o dano recebido pelos jogadores.                                                            |
| PlayerStomachDecreaseRate                | Ajusta a taxa na qual a fome do jogador aumenta.                                                   |
| PlayerStaminaDecreaseRate                | Ajusta a taxa na qual a stamina do jogador diminui.                                                |
| PlayerAutoHPRegeneRate                   | Ajusta a taxa de regeneração automática de saúde do jogador.                                       |
| PlayerAutoHpRegeneRateInSleep            | Ajusta a taxa de regeneração automática de saúde do jogador durante o sono.                        |
| PalStomachDecreaseRate                   | Ajusta a taxa na qual a fome dos pals aumenta.                                                     |
| PalStaminaDecreaseRate                   | Ajusta a taxa na qual a stamina da criatura Pal diminui.                                           |
| PalAutoHPRegeneRate                      | Ajusta a taxa de regeneração automática de saúde da criatura Pal.                                  |
| PalAutoHpRegeneRateInSleep               | Ajusta a taxa de regeneração automática de saúde da criatura Pal durante o sono.                   |
| BuildObjectDamageRate                    | Ajusta a taxa na qual objetos construídos recebem dano.                                            |
| BuildObjectDeteriorationDamageRate       | Ajusta a taxa na qual objetos construídos se deterioram.                                           |
| CollectionDropRate                       | Ajusta a taxa de queda de itens coletados.                                                         |
| CollectionObjectHpRate                   | Ajusta a saúde de objetos coletados.                                                               |
| CollectionObjectRespawnSpeedRate         | Ajusta a velocidade de respawn de objetos coletados.                                               |
| EnemyDropItemRate                        | Ajusta a taxa de queda de itens de inimigos derrotados.                                            |
| DeathPenalty                             | Define a penalidade após a morte do jogador. Valores: 0 ou 'None' (Sem penalidades após a morte), 1 ou 'Item' (Itens na mochila caem ao morrer), 2 ou 'ItemAndEquipment' (Itens na mochila e equipados caem ao morrer), 3 ou 'All' (Itens na mochila e equipados caem, incluindo Pals).           |
| bEnablePlayerToPlayerDamage              | Habilita ou desabilita o dano de jogador para jogador.                                             |
| bEnableFriendlyFire                      | Habilita ou desabilita o fogo amigo.                                                               |
| bEnableInvaderEnemy                      | Habilita ou desabilita inimigos invasores.                                                         |
| bActiveUNKO                              | Ativa ou desativa UNKO (Unidentified Nocturnal Knock-off).                                         |
| bEnableAimAssistPad                      | Habilita ou desabilita a assistência de mira para controles.                                       |
| bEnableAimAssistKeyboard                 | Habilita ou desabilita a assistência de mira para teclados.                                        |
| DropItemMaxNum                           | Define o número máximo de itens caídos no jogo.                                                    |
| DropItemMaxNum_UNKO                      | Define o número máximo de itens UNKO caídos no jogo.                                               |
| BaseCampMaxNum                           | Define o número máximo de acampamentos base que podem ser construídos.                             |
| BaseCampWorkerMaxNum                     | Define o número máximo de trabalhadores em um acampamento base.                                    |
| DropItemAliveMaxHours                    | Define o tempo máximo que itens permanecem ativos após serem caídos.                               |
| bAutoResetGuildNoOnlinePlayers           | Reseta automaticamente guildas sem jogadores online.                                               |
| AutoResetGuildTimeNoOnlinePlayers        | Define o tempo após o qual guildas sem jogadores online são resetadas automaticamente.             |
| GuildPlayerMaxNum                        | Define o número máximo de jogadores em uma guilda.                                                 |
| PalEggDefaultHatchingTime                | Define o tempo padrão de incubação para ovos de Pal.                                               |
| WorkSpeedRate                            | Ajusta a velocidade geral de trabalho no jogo.                                                     |
| bIsMultiplay                             | Ativa ou desativa o modo multijogador.                                                             |
| bIsPvP                                   | Ativa ou desativa o modo jogador contra jogador (PvP).                                             |
| bCanPickupOtherGuildDeathPenaltyDrop     | Ativa ou desativa a coleta de itens de penalidade de morte de outras guildas.                      |
| bEnableNonLoginPenalty                   | Ativa ou desativa penalidades para não login.                                                      |
| bEnableFastTravel                        | Ativa ou desativa a viagem rápida.                                                                 |
| bIsStartLocationSelectByMap              | Ativa ou desativa a seleção de locais de início no mapa.                                           |
| bExistPlayerAfterLogout                  | Ativa ou desativa a existência de jogadores após o logout.                                         |
| bEnableDefenseOtherGuildPlayer           | Ativa ou desativa a defesa de jogadores de outras guildas.                                         |
| CoopPlayerMaxNum                         | Define o número máximo de jogadores cooperativos em uma sessão.                                    |
| ServerPlayerMaxNum                       | Define o número máximo de jogadores permitidos no servidor.                                        |
| ServerName                               | Define o nome do servidor Palworld.                                                                |
| ServerDescription                        | Fornece uma descrição para o servidor Palworld.                                                    |
| AdminPassword                            | Define a senha para administração do servidor.                                                     |
| ServerPassword                           | Define a senha para entrar no servidor Palworld.                                                   |
| PublicPort                               | Define a porta pública para o servidor Palworld.                                                   |
| PublicIP                                 | Define o endereço IP público para o servidor Palworld.                                             |
| RCONEnabled                              | Ativa ou desativa o Console Remoto (RCON) para administração do servidor.                          |
| RCONPort                                 | Define a porta para comunicação do Console Remoto (RCON).                                          |
| Region                                   | Define a região do servidor Palworld.                                                              |
| bUseAuth                                 | Ativa ou desativa a autenticação do servidor.                                                      |
| BanListURL                               | Define a URL para a lista de banidos do servidor.                                                  |

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

# COMO FAZER BACKUP AUTOMÁTICO E RESTAURAR SAVES DO SERVIDOR DE PALWORLD
Este guia ensina como realizar backups automáticos e manuais do seu servidor dedicado de Palworld, além de como restaurar saves antigos.
Primeiro, precisamos atualizar nossa imagem do Docker para garantir que estamos usando a versão mais recente do servidor: 
      
      
