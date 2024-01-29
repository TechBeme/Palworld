# COMO CRIAR UM SERVIDOR DEDICADO DE PALWORLD
[![Crie seu Próprio Servidor Dedicado de Palworld GRÁTIS](https://github.com/TechBeme/Palworld/assets/101749351/970e4455-bc9e-4406-be1a-a43183c178d4)](https://youtu.be/ZXk4wE1rcXM)

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
   nano docker-compose.yml
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
            - ADMIN_PASSWORD="Senha do administrador"
            - COMMUNITY=true  # Mostra o seu servidor na listagem da comunidade.
            - SERVER_PASSWORD="senha do servidor"
            - SERVER_NAME="Nome do servidor"
         volumes:
            - ./servidor:/palworld/
   ```
   - Defina a porta, número de jogadores, senha e outras configurações.
   - Acesse a documentação completa [aqui](https://github.com/thijsvanloef/palworld-server-docker).
- **Salve o Arquivo de Configuração.**
- **Inicie o Servidor:**
   ```
   sudo docker compose up
   ``` 
   - Aguarde o servidor iniciar completamente.

### 4. Acesso ao Servidor
- **Abra a porta no Firewall:**
   - Crie uma regra UDP para o jogo com a porta que você definiu (Padrão: 8211).
- **Obtenha o IP Externo da VM.**
- **Abra o Palworld e Conecte-se ao Servidor:**
   - Vá para multiplayer e use IP:PORTA para se conectar.

# COMO ATUALIZAR O SERVIDOR DE PALWORLD
Para atualizar o servidor para a versão mais recente do Palworld basta reiniciar o servidor.
Confira se você está como usuário root e se está no mesmo diretório do arquivo [docker-compose.yml](/docker-compose.yml).
* Reiniciar o servidor:
```
sudo docker compose restart
```

# COMO ALTERAR AS CONFIGURAÇÕES DO SERVIDOR DE PALWORLD

### **Lista de Configurações do Servidor Dedicado do Palworld**

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
| PlayerStomachDecreaseRate                | Ajusta a taxa na qual o estômago do jogador diminui.                                               |
| PlayerStaminaDecreaseRate                | Ajusta a taxa na qual a stamina do jogador diminui.                                                |
| PlayerAutoHPRegeneRate                   | Ajusta a taxa de regeneração automática de saúde do jogador.                                       |
| PlayerAutoHpRegeneRateInSleep            | Ajusta a taxa de regeneração automática de saúde do jogador durante o sono.                        |
| PalStomachDecreaseRate                   | Ajusta a taxa na qual o estômago da criatura Pal diminui.                                          |
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
| GuildPlayerMaxNum                        | Define o número máximo de jogadores em uma guilda                                                  |


