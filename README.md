# Команды ssh которые нужно знать

## Переносим файлы через ssh

Скопировать локальный файл на сервер:  
scp file.gz root@server.my:/home/dir
  
Скопировать всё содержимое папки на сервере (рекурсивно) в локальную папку (с подробным выводом):  
scp -r root@server.my:/home/dir/ /home/local/my/

Между серверами:  
scp -r root@server1.my:/home/dir/ root@server2.my:/home/dir/

С указанием порта:  
scp -P 9999 file.zip user@server.my:~/
