# Como criar um servidor dedicado de Palworld
## Comandos
* Alterar para o usuário root:
```
sudo su
```
* Atualizar:
```
sudo apt update
sudo apt upgrade
```
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
         - PORT=8211 # Optional but recommended
         - PLAYERS=16 # Optional but recommended
         - MULTITHREADING=true
         - RCON_ENABLED=true
         - RCON_PORT=25575
         - ADMIN_PASSWORD="adminPasswordHere"
         - COMMUNITY=false  # Enable this if you want your server to show up in the community servers tab, USE WITH SERVER_PASSWORD!
         # Enable the environment variables below if you have COMMUNITY=true
         # - SERVER_PASSWORD="worldofpals"
         # - SERVER_NAME="World of Pals"
      volumes:
         - ./palworld:/palworld/
```
