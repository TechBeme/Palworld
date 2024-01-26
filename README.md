# Como criar um servidor dedicado de Palworld
## Comandos
* Alterar para o usuário root:
```
sudo su
```
* Atualizar o linux:
```
sudo apt update
sudo apt upgrade
```
* Caso não esteja usando o Ubuntu, acesse esse [site](https://docs.docker.com/engine/install/), selecione a plataforma que você está utulizando e instale o docker com os comandos indicados no site ao invés dos comandos abaixo.
* Instalar o docker (Ubuntu):
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
* Verificar a instalação:
```
docker --version
```
# Configurar o servidor
* Crie um arquivo chamado docker-compose.yml e insira as configurações do servidor de sua preferência.
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
* Criar o servidor:
```
docker compose up
```

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
