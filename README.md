# OTUS4_ZFS
 Homework
Описание ДЗ:

    Определить алгоритм с наилучшим сжатием. Зачем: отрабатываем навыки работы с созданием томов и установкой параметров. Находим наилучшее сжатие. Шаги:

    определить какие алгоритмы сжатия поддерживает zfs (gzip gzip-N, zle lzjb, lz4);
    создать 4 файловых системы на каждой применить свой алгоритм сжатия; Для сжатия использовать либо текстовый файл либо группу файлов:
    скачать файл “Война и мир” и расположить на файловой системе wget -O War_and_Peace.txt http://www.gutenberg.org/ebooks/2600.txt.utf-8, либо скачать файл ядра распаковать и расположить на файловой системе. Результат:
    список команд которыми получен результат с их выводами;
    вывод команды из которой видно какой из алгоритмов лучше.

    Определить настройки pool’a. Зачем: для переноса дисков между системами используется функция export/import. Отрабатываем навыки работы с файловой системой ZFS. Шаги:

    загрузить архив с файлами локально. https://drive.google.com/open?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg Распаковать.
    с помощью команды zfs import собрать pool ZFS;
    командами zfs определить настройки:
    размер хранилища;
    тип pool;
    значение recordsize;
    какое сжатие используется;
    какая контрольная сумма используется. Результат:
    список команд которыми восстановили pool . Желательно с Output команд;
    файл с описанием настроек settings.

    Найти сообщение от преподавателей. Зачем: для бэкапа используются технологии snapshot. Snapshot можно передавать между хостами и восстанавливать с помощью send/receive. Отрабатываем навыки восстановления snapshot и переноса файла. Шаги:

    скопировать файл из удаленной директории. https://drive.google.com/file/d/1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG/view?usp=sharing Файл был получен командой zfs send otus/storage@task2 > otus_task2.file
    восстановить файл локально. zfs receive
    найти зашифрованное сообщение в файле secret_message Результат:
    список шагов которыми восстанавливали;
    зашифрованное сообщение.


Установил машину из Vagrantfile - vagrant up
Создал четыре пула с фаловыми системами с с разным методом сжатия в каждой из них

zpool create home1 mirror /dev/sdb /dev/sdc
zpool create home2 mirror /dev/sdd /dev/sde
zpool create home3 mirror /dev/sdf /dev/sdg
zpool create home4 mirror /dev/sdh /dev/sdi

zfs set compression=lzjb home1
zfs set compression=lz4 home2
zfs set compression=gzip-9 home3
zfs set compression=zle home4

Скачал текстовый файл во все пулы
for i in {1..4}; do wget -P /home$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done
Проверил наличие файла во всех пулах
ls -l /home*

Сжатие методом gzip-9 оказалось самым эффективным - zfs get all | grep compressratio | grep -v ref

Скачал ‘archive.tar.gz’, разархивировал его, импортировал каталог в пул home
wget -O archive.tar.gz --no-check-certificate ‘https://drive.google.com/u/0/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&e xport=download’
tar -xzvf archive.tar.gz
zpool import -d zpoolexport/ home

Запросил параметры пула и файловой системы
zpool get all otus
zfs get all otus

Командой grep уточнил произвольные параметры
zfs get dedup otus
zfs get rootcontext home1
zfs get devices home2

Скачал otus_task2.file и восстановил файловую систему из этого снапшота
wget -O otus_task2.file --no-check-certificate ‘https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&e xport=download’
zfs receive otus/test@today < otus_task2.file

В файле stcret_message нашел ссылку на гит с украинским флагом и призывом помочь Украине
https://github.com/sindresorhus/awesome

zfs receive otus/test@today < otus_task2.file