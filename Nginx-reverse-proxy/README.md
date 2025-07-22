# Nginx: Reverse Proxy

## Описание
Инструкции по настройке обратного прокси (reverse proxy) в Nginx.

## Пример reverse proxy для backend-сервера
```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Документация
- [Nginx: Reverse Proxy](https://nginx.org/ru/docs/http/ngx_http_proxy_module.html)
