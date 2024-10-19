# Overview
Lexactyl is an helictyl based open-source pterodactyl management client area for with a clean good UI made in nodejs

### Features
Resource Management (Use it to create servers, edit them, etc.)

Coins (AFK Page earning)

Servers (create, view, edit servers)

User System (auth, regen password, etc)

OAuth2 (Discord)

Store (buy resources with coins)

Dashboard (view resources & servers)

Admin (set, add, remove coins/scan eggs & locations)


# Sponsors
Ko-fi sponsors are not working so come on our [discord](https://discord.gg/Z24TkRDSUH)

# Installation

Warning: You need Pterodactyl already setup on a domain for lexacyl to work
1. Upload the file above onto a Pterodactyl NodeJS server [Download the egg from Parkervcp's GitHub Repository](https://github.com/parkervcp/eggs/blob/master/generic/nodejs/egg-node-js-generic.json)
2. Unarchive the file and set the server to use NodeJS 16
3. Configure `config.tomel` with the scan or manuel
4. Run `npm i`
5. Start the server with `node index.js`
6. Login to your DNS manager, point the domain you want your dashboard to be hosted on to your VPS IP address. (Example: dashboard.domain.com 192.168.0.1)
7. Run `apt install nginx && apt install certbot` on the vps
8. Run `ufw allow 80` and `ufw allow 443` on the vps
9. Run `certbot certonly -d <Your domain>` then do 1 and put your email
10. Run `nano /etc/nginx/sites-enabled/lexactyl.conf`
11. Paste the configuration at the bottom of this and replace with the IP of the pterodactyl server including the port and with the domain you want your dashboard to be hosted on.
12. Run `systemctl restart nginx` and try open your domain.

# Nginx Proxy Config
```Nginx
server {
    listen 80;
    server_name <domain>;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;

    location /ws {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_pass "http://localhost:<port>/ws";
    }

    server_name <domain>;

    ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    location / {
      proxy_pass http://localhost:<port>/;
      proxy_buffering off;
      proxy_set_header X-Real-IP $remote_addr;
    }
}

```

Without ssl

```Nginx
server {
    listen 80;
    server_name <domain>;

    location /ws {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_pass "http://localhost:<port>/ws";
    }

    location / {
      proxy_pass http://localhost:<port>/;
      proxy_buffering off;
      proxy_set_header X-Real-IP $remote_addr;
    }
}
```

# License
(c) 2024 Lynix and contributors. All rights reserved. Licensed under the MIT License.

# Legacy Notice
Lexactyl 12.z is no more managed shift to Lexactyl 13
