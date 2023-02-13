# Написание скриптов Bash. Работа с утилитами и командами для работы с текстом.

##  Мануал по скрипту.

Скрипт `parslognginx.sh` помещаем (с необходимыми правами) в католог почасового запуска сервиса CRON `/etc/cron.hourly`

Для проверки работы скрипта был использован файл лога `access-4560-644067.log` прикрепленный к материалам занятия. Скрипт разработан исключительно для этого формата лог файла.

Так как срипт предназначен для работы в фоновом режиме (запуск CRON), то не предусматривает интерактивного ввода параметров. Все файлы `access.log` , `time.log` , `temp.log` , `mail.log` указанные в скрипте дожны находится в каталоге самого скрипта, либо в теле скрипта по тексту к ним долже быть указан явный путь.

Для первого запуска скрипта необходимо создать `time.log` для хранения временных меток последнего запуска скрипта.

Ошибки веб-сервера/приложения выводятся в отчет в явном (неизмененном виде), так как это максимально информативно.
Ошибками принимаюся те ответы сервера HTTP-коды которых не входящие в диапазон 200-299. 

```
cat temp.log | awk 'BEGIN { FS = "\" "; OFS= "#"} ; {print $0,$2}' | awk 'BEGIN { FS = "#" }; { if (!(match($2,/2.*/))) { print $1 }}' >> mail.log
```
При условии наличия настроенного Postfix серврера (инструкция по настройке https://www.devopszones.com/2020/03/how-to-send-mail-through-gmail-on.html) отправка вывода скрипта выглядит следующим образом

```
mail.log | mail -s "Message Subject" otus2023@example.ru
```

Пошаговое разъяснение работы скрипта содержится в коментариях тела самого скрипта.

## Вариант вывод:

```
Обрабатываемый временной диапазон c  14 Aug 2019 19:02:51  по  15 Aug 2019 00:25:46
Список 5-ти IP адресов (с наибольшим кол-вом запросов) c момента последнего запуска скрипта (формат вывода колл-во/IP адрес)
     20 185.6.8.9
     15 93.158.167.130
     12 185.142.236.35
     10 87.250.233.68
      8 62.75.198.172
Список 5-ти запрашиваемых URL (с наибольшим кол-вом запросов) c момента последнего запуска скрипта (формат вывода колл-во/URL)
     44 /
     32 /wp-login.php
     16 /xmlrpc.php
      7 /robots.txt
      7 
Ошибки веб-сервера/приложения c момента последнего запуска
87.250.233.68 - -  14 Aug 2019 19:02:51 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
93.158.167.130 - -  14 Aug 2019 19:16:50 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
185.142.236.35 - -  14 Aug 2019 19:23:14 +0300  "GET / HTTP/1.1" 301 5 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1847.137 Safari/537.36"rt=0.192 uct="0.000" uht="0.192" urt="0.192"
185.142.236.35 - -  14 Aug 2019 19:23:14 +0300  "" 400 0 "-" "-"rt=0.000 uct="-" uht="-" urt="-"
185.142.236.35 - -  14 Aug 2019 19:23:15 +0300  "" 400 0 "-" "-"rt=0.000 uct="-" uht="-" urt="-"
185.142.236.35 - -  14 Aug 2019 19:23:15 +0300  "" 400 0 "-" "-"rt=0.000 uct="-" uht="-" urt="-"
185.142.236.35 - -  14 Aug 2019 19:23:15 +0300  "" 400 0 "-" "-"rt=0.000 uct="-" uht="-" urt="-"
185.142.236.35 - -  14 Aug 2019 19:23:18 +0300  "quit" 400 173 "-" "-"rt=0.011 uct="-" uht="-" urt="-"
185.142.236.35 - -  14 Aug 2019 19:23:18 +0300  "GET /.well-known/security.txt HTTP/1.1" 404 169 "-" "-"rt=0.000 uct="-" uht="-" urt="-"
185.142.236.35 - -  14 Aug 2019 19:23:22 +0300  "" 400 0 "-" "-"rt=0.000 uct="-" uht="-" urt="-"
93.158.167.130 - -  14 Aug 2019 19:23:49 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.68 - -  14 Aug 2019 19:26:40 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.120 - -  14 Aug 2019 19:33:24 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexBot/3.0; +http://yandex.com/bots)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.68 - -  14 Aug 2019 20:03:43 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.75 - -  14 Aug 2019 20:33:15 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
93.158.167.130 - -  14 Aug 2019 20:40:19 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.68 - -  14 Aug 2019 20:42:50 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
107.179.102.58 - -  14 Aug 2019 20:46:42 +0300  "GET /wp-content/plugins/uploadify/includes/check.php HTTP/1.1" 301 185 "http://dbadmins.ru/wp-content/plugins/uploadify/includes/check.php" "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.152 Safari/537.36"rt=0.153 uct="-" uht="-" urt="-"
107.179.102.58 - -  14 Aug 2019 20:46:45 +0300  "GET /wp-content/plugins/uploadify/includes/check.php HTTP/1.1" 500 595 "http://dbadmins.ru/wp-content/plugins/uploadify/includes/check.php" "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.152 Safari/537.36"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.76 - -  14 Aug 2019 20:49:48 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
185.6.8.9 - -  14 Aug 2019 20:59:06 +0300  "GET /robots.txt HTTP/1.1" 301 185 "-" "(info@domaincrawler.com; http://www.domaincrawler.com/dbadmins.ru)"rt=0.026 uct="-" uht="-" urt="-"
185.6.8.9 - -  14 Aug 2019 20:59:08 +0300  "GET / HTTP/1.1" 301 185 "-" "(info@domaincrawler.com; http://www.domaincrawler.com/dbadmins.ru)"rt=0.027 uct="-" uht="-" urt="-"
185.6.8.9 - -  14 Aug 2019 20:59:43 +0300  "GET /%D0%A3%D0%B4%D0%B0%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B5-%D0%B0%D0%B4%D0%BC%D0%B8%D0%BD%D0%B8%D1%81%D1%82%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%A1%D0%A3%D0%91%D0%94-oracle/ HTTP/1.1" 301 185 "-" "(info@domaincrawler.com; http://www.domaincrawler.com/dbadmins.ru)"rt=0.027 uct="-" uht="-" urt="-"
5.45.203.15 - -  14 Aug 2019 21:36:19 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
5.45.203.12 - -  14 Aug 2019 21:50:58 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
193.106.30.99 - -  14 Aug 2019 22:04:04 +0300  "POST /wp-content/uploads/2018/08/seo_script.php HTTP/1.1" 500 595 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36"rt=0.062 uct="-" uht="-" urt="-"
93.158.167.130 - -  14 Aug 2019 22:05:00 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
93.158.167.130 - -  14 Aug 2019 22:15:47 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.68 - -  14 Aug 2019 22:18:00 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.68 - -  14 Aug 2019 22:56:43 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
172.104.242.173 - -  14 Aug 2019 23:22:13 +0300  "9\xCD\xC3V\x8C&\x12Dz/\xB7\xC0t\x96C\xE2" 400 173 "-" "-"rt=0.010 uct="-" uht="-" urt="-"
54.208.102.37 - -  14 Aug 2019 23:23:59 +0300  "GET / HTTP/1.1" 301 185 "http://dbadmins.ru/" "Mozilla/5.0 (compatible; DuckDuckGo-Favicons-Bot/1.0; +http://duckduckgo.com)"rt=0.000 uct="-" uht="-" urt="-"
87.250.233.75 - -  14 Aug 2019 23:24:32 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
93.158.167.130 - -  14 Aug 2019 23:31:56 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
5.255.251.4 - -  14 Aug 2019 23:38:25 +0300  "GET / HTTP/1.1" 301 185 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
209.17.96.74 - -  14 Aug 2019 23:44:04 +0300  "GET / HTTP/1.1" 301 5 "-" "Mozilla/5.0 (compatible; Nimbostratus-Bot/v1.3.2; http://cloudsystemnetworks.com)"rt=0.185 uct="0.000" uht="0.185" urt="0.185"
77.247.110.165 - -  14 Aug 2019 23:44:18 +0300  "HEAD /robots.txt HTTP/1.0" 404 0 "-" "-"rt=0.017 uct="-" uht="-" urt="-"
87.250.233.68 - -  15 Aug 2019 00:00:37 +0300  "GET / HTTP/1.1" 404 169 "-" "Mozilla/5.0 (compatible; YandexMetrika/2.0; +http://yandex.com/bots yabs01)"rt=0.000 uct="-" uht="-" urt="-"
182.254.243.249 - -  15 Aug 2019 00:24:38 +0300  "PROPFIND / HTTP/1.1" 405 173 "-" "-"rt=0.214 uct="-" uht="-" urt="-"
Список всех кодов HTTP ответа с указанием их кол-ва с момента последнего запуска скрипта (формат вывода колл-во/код HTTP ответа)
    106 200
     17 301
     13 404
      7 400
      2 500
      1 499
      1 405
```