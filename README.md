# OTUS4_ZFS
 Homework
�������� ��:

    ���������� �������� � ��������� �������. �����: ������������ ������ ������ � ��������� ����� � ���������� ����������. ������� ��������� ������. ����:

    ���������� ����� ��������� ������ ������������ zfs (gzip gzip-N, zle lzjb, lz4);
    ������� 4 �������� ������� �� ������ ��������� ���� �������� ������; ��� ������ ������������ ���� ��������� ���� ���� ������ ������:
    ������� ���� ������ � ��� � ����������� �� �������� ������� wget -O War_and_Peace.txt http://www.gutenberg.org/ebooks/2600.txt.utf-8, ���� ������� ���� ���� ����������� � ����������� �� �������� �������. ���������:
    ������ ������ �������� ������� ��������� � �� ��������;
    ����� ������� �� ������� ����� ����� �� ���������� �����.

    ���������� ��������� pool�a. �����: ��� �������� ������ ����� ��������� ������������ ������� export/import. ������������ ������ ������ � �������� �������� ZFS. ����:

    ��������� ����� � ������� ��������. https://drive.google.com/open?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg �����������.
    � ������� ������� zfs import ������� pool ZFS;
    ��������� zfs ���������� ���������:
    ������ ���������;
    ��� pool;
    �������� recordsize;
    ����� ������ ������������;
    ����� ����������� ����� ������������. ���������:
    ������ ������ �������� ������������ pool . ���������� � Output ������;
    ���� � ��������� �������� settings.

    ����� ��������� �� ��������������. �����: ��� ������ ������������ ���������� snapshot. Snapshot ����� ���������� ����� ������� � ��������������� � ������� send/receive. ������������ ������ �������������� snapshot � �������� �����. ����:

    ����������� ���� �� ��������� ����������. https://drive.google.com/file/d/1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG/view?usp=sharing ���� ��� ������� �������� zfs send otus/storage@task2 > otus_task2.file
    ������������ ���� ��������. zfs receive
    ����� ������������� ��������� � ����� secret_message ���������:
    ������ ����� �������� ���������������;
    ������������� ���������.


��������� ������ �� Vagrantfile - vagrant up
������ ������ ���� � �������� ��������� � � ������ ������� ������ � ������ �� ���

zpool create home1 mirror /dev/sdb /dev/sdc
zpool create home2 mirror /dev/sdd /dev/sde
zpool create home3 mirror /dev/sdf /dev/sdg
zpool create home4 mirror /dev/sdh /dev/sdi

zfs set compression=lzjb home1
zfs set compression=lz4 home2
zfs set compression=gzip-9 home3
zfs set compression=zle home4

������ ��������� ���� �� ��� ����
for i in {1..4}; do wget -P /home$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done
�������� ������� ����� �� ���� �����
ls -l /home*

������ ������� gzip-9 ��������� ����� ����������� - zfs get all | grep compressratio | grep -v ref

������ �archive.tar.gz�, �������������� ���, ������������ ������� � ��� home
wget -O archive.tar.gz --no-check-certificate �https://drive.google.com/u/0/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&e xport=download�
tar -xzvf archive.tar.gz
zpool import -d zpoolexport/ home

�������� ��������� ���� � �������� �������
zpool get all otus
zfs get all otus

�������� grep ������� ������������ ���������
zfs get dedup otus
zfs get rootcontext home1
zfs get devices home2

������ otus_task2.file � ����������� �������� ������� �� ����� ��������
wget -O otus_task2.file --no-check-certificate �https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&e xport=download�
zfs receive otus/test@today < otus_task2.file

� ����� stcret_message ����� ������ �� ��� � ���������� ������ � �������� ������ �������
https://github.com/sindresorhus/awesome

zfs receive otus/test@today < otus_task2.file