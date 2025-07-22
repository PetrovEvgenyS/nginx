# Nginx: Redirect

## Описание
Инструкции по настройке HTTP-редиректов в Nginx.

## Пример постоянного редиректа всего сайта
```nginx
server {
    listen 80;
    server_name oldsite.com;
    return 301 http://newsite.com$request_uri;
}
```

## Пример постоянного редиректа с одного URL на другой
```nginx
location /oldpage {
    return 301 /newpage;
}
```

## Пример временного редиректа (302)
```nginx
location /promo {
    return 302 http://external-site.com/promo;
}
```

## Документация
- [Nginx: Redirects](https://nginx.org/ru/docs/http/ngx_http_rewrite_module.html)
