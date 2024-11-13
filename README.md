Часть 1

   1.  Установить на ВМ php, если еще не установлен
          sudo apt install php

    
  2.   В директории /opt создать папку rot13
        pmn@pmn-ubuntu:/opt$ sudo mkdir rot13


    
  3.   В директории /opt/rot13 создать файл с именем rot13-server.php с кодом

<?php
$sock = socket_create(AF_INET, SOCK_DGRAM, SOL_UDP);
socket_bind($sock, '0.0.0.0', 10000);
for (;;) {
  socket_recvfrom($sock, $message, 1024, 0, $ip, $port);
  $reply = str_rot13($message);
  socket_sendto($sock, $reply, strlen($reply), 0, $ip, $port);
}

 4.    Запустить сервер командой php rot13-server.php и проверить что сервер работает: выполнить nc -u 127.0.0.1 10000 и ввести Hello World
    

  5. Написать юнит-файл для запуска rot13-server.php как сервиса

  sudo nano /etc/systemd/system/myphp.service 


  

  [Unit]
Description=php


[Service]
Type=simple
ExecStart=/usr/bin/php /opt/rot13/rot13-server.php
Restart=always  


[Install]
WantedBy=multi-user.target
 

Часть 2

    Написать юнит-файл для запуска скрипта как сервиса systemd, создать таймер для периодического выполнения задачи про таймеры можно посмотреть тут https://youtu.be/hTkaCE5Mz8I

если сервис хотя бы раз удачно отработал, то должен появиться файл /var/log/dsk-usage.log с записью примерно такого вида

1730956510 [ALARM] Disk usage for / is greater then 5: current value is 35

    Написать юнит-файл для запуска простого веб-сервера на python. После того как сервис запустится выполнить команду

curl 127.0.0.1:9999

ответ должен быть Hello from Python



Часть 3 (задача со звездочкой)

   1. Установить redis (install-redis-on-linux)
       sudo apt install redis-server
    
   2.  Посмотреть статус демона redis
    
    sudo systemctl status redis-server

    
    3. Запустить второй экземпляр демона redis, написав systemd unit

    [Unit]
Description=Redis 
After=network.target

[Service]
#User=redis
#Group=redis
ExecStart=/usr/local/bin/redis-server /etc/redis/redis_6380.conf
#ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target




 

   
 
