# Nginx на Ubuntu

## Описание
Этот проект содержит инструкции по установке и базовой настройке веб-сервера Nginx на дистрибутивах Ubuntu/Debian.

## Установка
```bash
apt update && apt -y install nginx
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
На Ubuntu после установки нужно разрешить HTTP/HTTPS в фаерволе:
```bash
ufw allow 'Nginx Full'
ufw reload
ufw status numbered
```
В статусе должны появиться правила для HTTP и HTTPS.

## Размещение сайта и настройка прав
1. Создайте каталог для сайта:
   ```bash
   mkdir -p /var/www/example
   ```

2. Создайте файл index.html:
   В директории `Simple-HTML` этого репозитория находится пример файла `index.html`, который можно использовать для проверки работы. Просто скопируйте его содержимое в:
   ```bash
   vim /var/www/example/index.html
   ```

3. Измените права доступа:
   ```bash
   chown -R www-data:www-data /var/www/example
   chmod -R 755 /var/www/example
   chmod 644 /var/www/example/index.html
   ```

## Настройка виртуального хоста
1. Создайте новый конфиг:
   ```bash
   vim /etc/nginx/sites-available/example
   ```
   Пример содержимого:
   ```nginx
   server {
      listen 80;
      server_name example.com;
      root /var/www/example;
      
      index index.html;

      access_log /var/log/nginx/example_access.log;
      error_log /var/log/nginx/example_error.log;

      location / {
         try_files $uri $uri/ =404; # Проверка существования файлов и директорий, иначе 404
      }

      error_page 404 /404.html;     # Настройка страницы ошибки 404
      location = /404.html {
         root /var/www/html;        # Директория, где находится файл ошибки 404
      }

      location /basic_status {
         stub_status;            # Включает модуль статуса
         allow 127.0.0.1;        # Разрешает доступ только с локального хоста
         deny all;               # Блокирует все остальные подключения
      }
   }
   ```

2. Деактивируйте сайт по умолчанию:

   ```bash
   rm /etc/nginx/sites-enabled/default
   ```

3. Активируйте новый сайт:
   ```bash
   ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/
   ```

4. Проверьте конфигурацию:
   ```bash
   nginx -t
   ```
   Должно быть: `syntax is ok`

5. Перезапустите Nginx:
   ```bash
   systemctl reload nginx
   ```

## Проверка работы
Откройте IP-адрес сервера в браузере — должна открыться стартовая страница Nginx или ваш сайт.

## Структура каталогов
- `/var/www/html` — основной каталог для размещения сайта по умолчанию
- `/var/www/example` — каталог для вашего сайта
- `/etc/nginx/nginx.conf` — основной конфиг
- `/etc/nginx/sites-available/` — рекомендуемое место для пользовательских конфигов (виртуальных хостов)
- `/etc/nginx/sites-enabled/` — активированные сайты

## Документация
- [Официальная документация Nginx](https://nginx.org/ru/docs/)
