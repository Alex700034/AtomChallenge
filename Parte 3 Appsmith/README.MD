Video Explicativo:

https://www.loom.com/share/025679b75ce44837a470da7c83c34e11?sid=1cdd758d-904a-4051-b652-b8a58921e1a5

# Url Publica del front end

https://atomchallenge.cloudns.cl/app/atomchallenge/page1-687c5f841e32de4f7a1ad998

Despliegue de Appsmith

# Instalar Docker

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Actualizar e instalar Docker con todo su complemento

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Verificar Instalación

docker --version
docker compose version

# Despliegue de Appsmith

1- Crear carpeta para Appsmith
mkdir ~/appsmith
cd ~/appsmith

2-  Descargar el archivo docker-compose.yml

curl -L https://bit.ly/3AQzII6 -o docker-compose.yml

3- Levantar Appsmith

sudo docker compose up -d

4- Instalar Nginx y Certbot

sudo apt update
sudo apt install nginx certbot python3-certbot-nginx -y

5- Crear configuración de Nginx para Appsmith:

sudo nano /etc/nginx/sites-available/Appsmith

6- Archivo de configuración

server {
    listen 80;
    server_name atomchallenge.clodns.cl;  

    location / {
        proxy_pass http://localhost:8080;  
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_cache off;

        proxy_set_header Connection 'Upgrade';
        proxy_set_header Upgrade $http_upgrade;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

7- Solicitar SSL:

sudo certbot --nginx -d atomchallenge.cloudns.cl
