# Команды ssh которые нужно знать

## Архивы

#### Запаковать в tar.gz:  
tar -zcvf имя_архива архивируемая_папка   
например  
tar -zcvf folder.tar.gz folder/  
- c – значит, что создается новый архив;
- f – посредством этого флага задается имя создаваемого архива;
- z – архивация будет происходить посредством архиватора gzip;
- v – в консоль будет выводиться информация о процессе архивации.

#### Распаковать файлы из tar.gz через SSH
tar -xzvf имя_архива.tar.gz  
например  
tar -xzvf archive.tar.gz

#### Запаковать файлы в zip
zip -r имя_архива архивируемая_папка  
например  
zip -r logs.zip logs/  
r означает, что нужно искать файлы в папке рекурсивно, иначе в архиве бы оказалась одна пустая папка.

#### Распаковать файлы из zip через SSH
unzip имя_архива.zip  
например  
unzip archive.zip  


## Переносим файлы через ssh

Скопировать локальный файл на сервер:  
scp file.gz root@server.my:/home/dir
  
Скопировать всё содержимое папки на сервере (рекурсивно) в локальную папку (с подробным выводом):  
scp -r root@server.my:/home/dir/ /home/local/my/

Между серверами:  
scp -r root@server1.my:/home/dir/ root@server2.my:/home/dir/

С указанием порта:  
scp -P 9999 file.zip user@server.my:~/

###Дополнительные флаги
-r - рекурсивное копирование (для директорий)  
-C - использовать сжатие при передачи  
-P - порт ssh (P большая! и -P указывает перед ssh хостом)  
-p - сохранить информацию о времени создания, модификации файла.  

Для передачи файлов часто бывает лучше использовать утилиту rsync.  


##Базы данных MySQL

Импорт дампа базы данных  
mysql -u db_user -p -h localhost db_name < dump.sql

Импорт дампа базы данных, упакованных в gzip (*.sql.gz)  
gunzip < dump.sql.gz | mysql -u db_user -p db_name

Экспорт базы данных (создание дампа)  
mysqldump -u db_user -p -h localhost db_name > dump.sql

Создание архива GZip с дампом БД  
mysqldump -u db_user -p -h localhost db_name | gzip > dump.tar.gz

Создание дампа нескольких баз данных одновременно  
mysqldump -u db_user -p -h localhost -B db_name1 db_name2 db_name3 > databases.sql

Создание дампа всех баз данных  
mysqldump -u db_user -p -h localhost -A > all-databases.sql

Сохранить только структуру БД  
mysqldump --no-data -u db_user -p -h localhost db_name > schema.sql

Создание дампа только одной или нескольких таблиц БД  
mysqldump -u db_user -p -h localhost db_name tbl_name1 tbl_name2 tbl_name3 > dump.sql

Дополнительные атрибуты (уменьшают размер дампа и повышают скорость работы)  
mysqldump -Q -c -e -u db_user -p -h localhost db_name > /path/to/file/dump.sql

-Q - оборачивает имена обратными кавычками;  
-c - делает полную вставку, включая имена колонок;  
-e - делает расширенную вставку.  
