
git clone https://github.com/<your-organization>/n8n.git n8n
cd n8n

Carga de la imagen personalizada

reemplazar imagen en formato svg en el repositorio.

mv ./Alex700034/n8n-logo.svg ~n8n/packages/frontend/editor-ui/src/components/Logo/n8n-logo.svg

npm install
npm run build
npm run start

Configuracion de SSl

### Step 6: Install Nginx
```bash
sudo apt install nginx
```

### Step 7: Create an Nginx configuration file for N8N
```bash
sudo nano /etc/nginx/sites-available/n8n
```
Add the following content:
```nginx
server {
    listen 80;
    server_name atomchallenge.cloudns.cl;

    location / {
        proxy_pass http://localhost:5678;
        proxy_http_version 1.1;
        chunked_transfer_encoding off;
        proxy_buffering off;
        proxy_cache off;

        # Headers for WebSocket support
        proxy_set_header Connection 'Upgrade';
        proxy_set_header Upgrade $http_upgrade;

        # Additional headers for forwarding client info
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
Save and exit using:
```bash
CTRL+O, ENTER, CTRL+X
```

### Step 8: Enable the Nginx configuration
```bash
sudo ln -s /etc/nginx/sites-available/n8n /etc/nginx/sites-enabled/
```

### Step 9: Test Nginx configuration
```bash
sudo nginx -t
```

### Step 10: Restart Nginx service
```bash
sudo systemctl restart nginx
```

### Step 11: Install Certbot for SSL Certificates
```bash
sudo apt install certbot python3-certbot-nginx
```

### Step 12: Obtain SSL Certificate
```bash
sudo certbot --nginx -d atomchallenge.zapto.org
