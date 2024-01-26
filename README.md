# Como criar um servidor dedicado de Palworld
[![Crie seu Próprio Servidor Dedicado de Palworld GRÁTIS](https://github.com/TechBeme/Palworld/assets/101749351/970e4455-bc9e-4406-be1a-a43183c178d4)](https://youtu.be/ZXk4wE1rcXM)

## Introdução
Criar um servidor dedicado para Palworld permite jogar com amigos a qualquer momento, sem depender do seu PC ou internet pessoal. Este guia aborda a criação de um servidor utilizando serviços de nuvem como Oracle Cloud, Google Cloud, AWS e Hostinger.

## Passos para a Criação do Servidor

### 1. Escolha do Serviço de Hospedagem
- **Oracle Cloud:** 4 CPUs e 24 GB de RAM grátis para sempre.
- **Google Cloud:** $300 de crédito grátis por três meses.
- **AWS:** Teste gratuito por 12 meses.
- **Hostinger:** Opção paga, mas acessível: 2 CPUs 8 GB de RAM por R$34,99/mês.

### 2. Configuração da Máquina Virtual
1. **Crie uma Conta no serviço escolhido.**
2. **Crie uma Nova Máquina Virtual (VM):**
   - Localização: Escolha a mais próxima para menor latência (exemplo: São Paulo, Brasil).
   - Configurações: Neste guia vou utilizar o Sistema Operacional Ubuntu.

### 3. Acesso e Configuração do Servidor
1. **Acesse a VM via SSH.**
2. **Obtenha Acesso Root e Atualize a Máquina:**
```
sudo su
```
```
sudo apt update
sudo apt upgrade
```
3. **Instale o Docker:**
   - Siga as instruções do [site oficial](https://docs.docker.com/engine/install/) do Docker para instalação.
   - Verificar a instalação:
```
docker --version
```

### 4. Configuração do Servidor Palworld
1. **Baixe e Edite o Arquivo de Configuração do Servidor Palworld:**
   - Crie um arquivo chamado docker-compose.yml e insira as configurações do servidor de sua preferência.
```
nano docker-compose.yml
```
Exemplo de configuração:
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
Acesse a documentação completa [aqui](https://github.com/thijsvanloef/palworld-server-docker).
   - Defina a porta, número de jogadores, senha e outras configurações.
3. **Salve o Arquivo de Configuração.**
4. **Inicie o Servidor:**
```
docker compose up
``` 
   - Aguarde o servidor iniciar completamente.

# COMO ATUALIZAR O SERVIDOR DE PALWORLD
* Para atualizar o servidor para a versão mais recente do Palworld basta reiniciar o servidor.
* Confira se você está como usuário root e se está no mesmo diretório em que está o arquivo docker-compose.yml.
* Se tornar usuário root:
```
sudo su
```
* Reiniciar o servidor:
```
docker container restart
```
