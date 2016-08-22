# Команды ssh которые нужно знать
- [Архивы](#arch)
- [Перенос файлов](#scp)
- [Базы данных](#db)
- [Работа с файлами](#files)

---------------------------------

## <a name="arch"></a> Архивы

#### Запаковать в tar.gz:  
tar -zcvf fdump.tar.gz folder/  
-c – значит, что создается новый архив;  
-f – посредством этого флага задается имя создаваемого архива;  
-z – архивация будет происходить посредством архиватора gzip;  
-v – в консоль будет выводиться информация о процессе архивации.  

#### Распаковать файлы из tar.gz через SSH
tar -xzvf fdump.tar.gz
-x - распаковать архив

#### Запаковать файлы в zip
zip -r fdump.zip folder/  
-r означает, что нужно искать файлы в папке рекурсивно, иначе в архиве бы оказалась одна пустая папка.

#### Распаковать файлы из zip
unzip fdump.zip  

------------------------------------

## <a name="scp"></a> Переносим файлы через ssh

####Скопировать локальный файл на сервер:  
scp fdump.zip root@server.my:/home/dir
  
####Скопировать всё содержимое папки на сервере (рекурсивно) в локальную папку (с подробным выводом):  
scp -r root@server.my:/home/dir/ /home/local/my/

####Между серверами:  
scp -r root@server1.my:/home/dir/ root@server2.my:/home/dir/

#####С указанием порта:  
scp -P 9999 fdump.zip user@server.my:~/

####Дополнительные флаги
-r - рекурсивное копирование (для директорий)  
-C - использовать сжатие при передачи  
-P - порт ssh (P большая! и -P указывает перед ssh хостом)  
-p - сохранить информацию о времени создания, модификации файла.  

Для передачи файлов часто бывает лучше использовать утилиту rsync.  

--------------------------------

## <a name="db"></a> Базы данных MySQL

#### Экспорт базы данных (создание дампа)  
mysqldump -u db_user -p -h localhost db_name > dump.sql  

####Создание архива GZip с дампом БД  
mysqldump -u db_user -p -h localhost db_name | gzip > dump.tar.gz

####Создание дампа нескольких баз данных одновременно  
mysqldump -u db_user -p -h localhost -B db_name1 db_name2 db_name3 > databases.sql

####Создание дампа всех баз данных  
mysqldump -u db_user -p -h localhost -A > all-databases.sql

####Сохранить только структуру БД  
''mysqldump --no-data -u db_user -p -h localhost db_name > schema.sql''

####Создание дампа только одной или нескольких таблиц БД  
mysqldump -u db_user -p -h localhost db_name tbl_name1 tbl_name2 tbl_name3 > dump.sql

Дополнительные атрибуты (уменьшают размер дампа и повышают скорость работы)  
mysqldump -Q -c -e -u db_user -p -h localhost db_name > /path/to/file/dump.sql

-Q - оборачивает имена обратными кавычками;  
-c - делает полную вставку, включая имена колонок;  
-e - делает расширенную вставку.  


#### Импорт дампа базы данных  
mysql -u db_user -p -h localhost db_name < dump.sql

####Импорт дампа базы данных, упакованных в gzip (*.sql.gz)  
gunzip < dump.sql.gz | mysql -u db_user -p db_name

------------------------------
## <a name="files"></a> Работа с файлами

####Перенос файлов:
mv ~/domains/site.com/temp/* ~/domains/site.kz/  
mv ~/domains/site.com/temp/.htaccess ~/domains/site.kz/  

####Перенос всех скрытых файлов
mv /home/user/path/.[!.]* ~/path/folder  
Важно указывать абсолютный путь до скрытых файлов, а в директорию куда переносим - необязательно.  

####Копировать файлы:
cp -R ~/domains/sites.com/temp/* ~/domains/site.kz/  

####Скопировать все скрытые файлы:
cp -p /home/user/path/.[!.]* ~/path/folder  

Параметры:  
-R - рекурсивно, т.е. скопировать все содержимое.  
-p - preserve, сохраняет время изменения, т.е. копирует файл полностью в оригинальном виде.  
