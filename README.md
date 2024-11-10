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

  [Unit]
Description=php


[Service]
Type=simple
ExecStart=/usr/bin/php /opt/rot13/rot13-server.php
Restart=always  


[Install]
WantedBy=multi-user.target
 
 

   
 
