app-to-deploy
=============
# Приложение для воркшопа по деплойменту.

## Как запустить:

- `npm install`
- `npm run start`
- приложение начнёт слушать на 3000 порту (можно поменять через переменную окружения `PORT`)

## Конфигурация на сервере:

File app.conf в /etc/nginx/sites-enabled/ и в etc/nginx/sites-available/
Для того, чтобы nginx прослушивал порт localhost:3000 на 80 порту
```
server {
    listen 80;

server_name 111.11.111.11; // your IP

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

File deploythatapp.service в etc/systemd/system/
Для рестарта nodejs-сервера при его падении
```
[Unit]
Description=My node.js App
After=nginx.service

[Service]
User=app
Group=app
WorkingDirectory=/home/app/app-to-deploy
EnvironmentFile=/home/app/app_config
Environment=NODE_ENV=production
ExecStart=/home/app/app-to-deploy/www.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

## Лиценизия

MIT.
