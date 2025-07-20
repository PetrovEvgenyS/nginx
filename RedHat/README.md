# Nginx на Red Hat

## Описание
Этот проект содержит инструкции по установке и базовой настройке веб-сервера Nginx на дистрибутивах Red Hat (RHEL, CentOS, Almalinux).

## Установка
```bash
dnf -y install nginx
```

## Запуск и автозагрузка
```bash
systemctl enable nginx --now
```

## Проверка статуса
```bash
systemctl status nginx
```

## Настройка фаервола
На Red Hat после установки нужно разрешить HTTP/HTTPS в фаерволе:
```bash
firewall-cmd --permanent --add-service=http --add-service=https
firewall-cmd --reload && firewall-cmd --list-all
```
В блоке Services должны отображаться http и https.

## Размещение сайта и настройка прав
1. Создайте каталог для сайта:
   ```bash
   mkdir -p /usr/share/nginx/example
   chown -R nginx:nginx /usr/share/nginx/example
   chmod -R 755 /usr/share/nginx/example
   ```
2. Создайте файл index.html:
   В директории `Simple-HTML` этого репозитория находится пример файла `index.html`, который можно использовать для проверки работы. Просто скопируйте его содержимое в:
   ```bash
   vim /usr/share/nginx/example/index.html
   ```

3. Измените права доступа:
   ```bash
   chown nginx:nginx /usr/share/nginx/example/index.html
   chmod 644 /usr/share/nginx/example/index.html
   ```

## Настройка виртуального хоста
1. Создайте новый конфиг:
   ```bash
   vim /etc/nginx/conf.d/example.conf
   ```
   Пример содержимого:
   ```nginx
   server {
       listen 80;
       server_name example.com;
       root /usr/share/nginx/example;

       index index.html;

       access_log /var/log/nginx/example_access.log;
       error_log /var/log/nginx/example_error.log;

       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```
2. Проверьте конфигурацию:
   ```bash
   nginx -t
   ```
   Должно быть: `syntax is ok`

3. Перезапустите Nginx:
   ```bash
   systemctl reload nginx
   ```

## Проверка работы
Откройте IP-адрес сервера в браузере — должна открыться стартовая страница Nginx или ваш сайт.

## Структура каталогов
- `/usr/share/nginx/html` — основной каталог для размещения сайта по умолчанию
- `/usr/share/nginx/example` — каталог для вашего сайта
- `/etc/nginx/nginx.conf` — основной конфиг
- `/etc/nginx/conf.d/` — рекомендуемое место для пользовательских конфигов (виртуальных хостов)

## Документация
- [Официальная документация Nginx](https://nginx.org/ru/docs/)
