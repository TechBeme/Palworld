# COMO CRIAR UM SERVIDOR DEDICADO DE PALWORLD
[![Crie seu Próprio Servidor Dedicado de Palworld GRÁTIS](https://github.com/TechBeme/Palworld/assets/101749351/970e4455-bc9e-4406-be1a-a43183c178d4)](https://youtu.be/ZXk4wE1rcXM)

# INTRODUÇÃO
Criar um servidor dedicado para Palworld permite jogar com amigos a qualquer momento, sem depender do seu PC ou internet pessoal. Este guia aborda a criação de um servidor utilizando serviços de nuvem como Oracle Cloud, Google Cloud, AWS e Hostinger.

> [!WARNING]
> Atualmente, servidores dedicados funcionam apenas para jogadores que utilizam a Steam. Jogadores do Xbox Gamepass ou do Xbox Console não conseguem se conectar a servidores dedicados. Eles conseguem jogar apenas em sessões por convite que comportam no máximo 4 jogadores.

# PASSOS PARA A CRIAÇÃO DO SERVIDOR
### 1. Escolha do Serviço de Hospedagem
- **[Oracle Cloud:](https://www.oracle.com/br/cloud/free)** 4 CPUs e 24 GB de RAM grátis para sempre.
- **[Google Cloud:](https://cloud.google.com)** $300 de crédito grátis por três meses.
- **[AWS:](https://aws.amazon.com/pt/free)** Teste gratuito por 12 meses.
- **[Hostinger:](https://hostinger.com.br?REFERRALCODE=1RFSV68)** Opção paga, mas acessível: 2 CPUs 8 GB de RAM por R$34,99/mês.

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
- **Crie um arquivo chamado docker-compose.yml.**
   ```
   nano docker-compose.yml
   ```
- **Insira as configurações do servidor de sua preferência.**
   - Exemplo de configuração:
   ```
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
            - ./palworld:/palworld/
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
Confira se você está como usuário root e se está no mesmo diretório do arquivo docker-compose.yml.
* Reiniciar o servidor:
```
sudo docker container restart
```
