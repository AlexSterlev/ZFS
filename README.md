## Выполнено ДЗ №4

## В процессе сделано:
 ### Часть 1

  ````
zpool create tank /dev/sdb
zfs create tank/lz4
zfs create tank/gzip
zfs create tank/gzip-N
zfs create tank/zle
zfs create tank/lzjb
  ````

  - Включаем разные методы компрессии

  ````
zfs set compression=lz4 tank/lz4
zfs set compression=gzip tank/gzip
zfs set compression=gzip-9 tank/gzip-9
zfs set compression=zle tank/zle
zfs set compression=lzjb tank/lzjb
zfs set dedup=on tank
  ````

  - Вывод zfs list

  ````
 NAME          USED  AVAIL     REFER  MOUNTPOINT
tank          268K   832M     29.5K  /tank
tank/gzip      24K   832M       24K  /tank/gzip
tank/gzip-9    24K   832M       24K  /tank/gzip-9
tank/lz4       24K   832M       24K  /tank/lz4
tank/lzjb      24K   832M       24K  /tank/lzjb
tank/zle       24K   832M       24K  /tank/zle
  ````

  - Скачаем файл ядра, распакуем и копируем папку kernel в каталоги с разной компрессией

  ````
[root@server linux-5.9]# cp -r kernel /tank/gzip
[root@server linux-5.9]# cp -r kernel /tank/gzip-9
[root@server linux-5.9]# cp -r kernel /tank/lz4
[root@server linux-5.9]# cp -r kernel /tank/lzjb
[root@server linux-5.9]# cp -r kernel /tank/zle

  ````

  - Вывод zfs list после копирования:

  ````
  NAME          USED  AVAIL     REFER  MOUNTPOINT
tank         27.8M   805M     29.5K  /tank
tank/gzip    3.18M   805M     3.18M  /tank/gzip
tank/gzip-9  3.17M   805M     3.17M  /tank/gzip-9
tank/lz4     4.85M   805M     4.85M  /tank/lz4
tank/lzjb    5.81M   805M     5.81M  /tank/lzjb
tank/zle     9.99M   805M     9.99M  /tank/zle

  ````

  - Вывод zfs get compression,compressratio:

  ````
  NAME         PROPERTY       VALUE     SOURCE
tank         compression    off       default
tank         compressratio  2.03x     -
tank/gzip    compression    gzip      local
tank/gzip    compressratio  3.53x     -
tank/gzip-9  compression    gzip-9    local
tank/gzip-9  compressratio  3.54x     -
tank/lz4     compression    lz4       local
tank/lz4     compressratio  2.28x     -
tank/lzjb    compression    lzjb      local
tank/lzjb    compressratio  1.90x     -
tank/zle     compression    zle       local
tank/zle     compressratio  1.09x     -

  ````

 Делаем вывод что степень сжатия самая высокая gzip-9
 
  ### Часть 2
  - Выкачиваем с гугл диска архив.
  - Копируем архив с Vagrant-хоста в гостевую виртуальную машину и распаковываем:

 ````
 vagrant scp /home/san4ez/zfs_task1.tar.gz server:/home/vagrant/
 
  ````
  
   ````
  tar -czv zfs_task1.tar.gz
 
  ````
  
  - Импортируем пулов устройств хранения данных
  
   ````
  sudo zpool import -d /home/vagrant/zpoolexport/ otus
 
  ````
  
