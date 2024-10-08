### NFS FUSE

#### <a name='toc'>Содержание</a>
1. [Создаём виртуальные машины](#create_vm)
2. [Настраиваем сервер NFS](#setting_server)
3. [Настраиваем клиент NFS](#setting_client)
4. [Автоматизация Vagrant + Ansible](#creating_automated)
5. [Дополнительные источники](#recommended_sources)

#### 1. [[⬆]](#toc) <a name='create_vm'>Создаём виртуальные машины</a>
![image](https://github.com/user-attachments/assets/29285fa7-86c3-45ef-afbc-62b20a614d5f)

#### Результат выполнения команды `vagrant up`
![image](https://github.com/user-attachments/assets/afdd54d7-0c98-4d4b-ba07-2ecc96d83612)

#### 2. [[⬆]](#toc) <a name='setting_server'>Настраиваем сервер NFS</a>

##### Заходим на сервер:
```
vagrant ssh nfss
```
##### Обновляем, устанавливем и проверяем статус сервера:
```
sudo apt update
sudo apt install nfs-kernel-server
sudo systemctl status nfs-server
```
![image](https://github.com/user-attachments/assets/87315fae-024a-4a4f-b6a1-e4f32aba6847)

##### Проверяем наличие слушающих портов 2049/udp, 2049/tcp,111/udp, 111/tcp командой `sudo netstat -tunlp`
![image](https://github.com/user-attachments/assets/8801c591-c8fb-4f12-a5a7-e5f3454e98ce)

##### Создаём и настраиваем директорию, которая будет экспортирована в будущем
```
sudo mkdir -p /srv/share/upload
sudo chown -R nobody:nogroup /srv/share
sudo chmod 0777 /srv/share/upload
```

##### Создаём файл index.html в только что созданной директории `/srv/share/upload`
```
echo "Hello World" >> /srv/share/upload/index.html
```
![image](https://github.com/user-attachments/assets/1ffe51fa-2252-43e4-ab85-14da0aa0222d)

##### Cоздаём в файле /etc/exports структуру, которая позволит экспортировать ранее созданную директорию:
```
cat << EOF > /etc/exports 
/srv/share 192.168.50.11/32(rw,sync,root_squash)
EOF
```
##### Экспортируем ранее созданную директорию `exportfs -r` и проверяем `exportfs -s`:
```
exportfs -r
exportfs -s
```

![image](https://github.com/user-attachments/assets/0b263e92-221e-4fba-8b7a-fc0806fa7bb2)

#### 3. [[⬆]](#toc) <a name='setting_client'>Настраиваем клиент NFS</a>

##### Заходим на клиента:
```
vagrant ssh nfsс
```
##### Обновляем и устанавливем клиента:
```
sudo apt update
sudo apt install nfs-common
```
![image](https://github.com/user-attachments/assets/fb2706c2-774a-40d6-99a8-4252a53202dd)

##### Создаем директорию, монтируем ее и проверяем
```
sudo mkdir -p /mnt/nfs
sudo mount 192.168.50.10:/srv/share/ /mnt/nfs
```
![image](https://github.com/user-attachments/assets/f70992ad-02e1-43bc-b9c7-dd0a3aaedac6)

##### Добавляем в /etc/fstab строку и проверяем
```
echo "192.168.50.10:/srv/share/ /mnt/nfs vers=3,noauto,x-systemd.automount 0 0" >> /etc/fstab
```
![image](https://github.com/user-attachments/assets/93b3009e-ea9a-448b-b69f-892655988197)

##### Далее выполняем команды:
```
systemctl daemon-reload
systemctl restart remote-fs.target
```
![image](https://github.com/user-attachments/assets/2ac777c8-b8c9-442f-a25c-b9d9a39f917f)

##### Проверяем работу RPC `showmount -a 192.168.50.10`
```
sudo showmount -a 192.168.50.10
```
![image](https://github.com/user-attachments/assets/2e05d650-d7c0-47ae-bbc6-958e7845b868)


#### 4. [[⬆]](#toc) <a name='creating_automated'>Автоматизация Vagrant + Ansible</a>

##### Структура
![image](https://github.com/user-attachments/assets/009642cf-efb4-4e2d-8f44-ce1eab7822d9)

##### Запускаем командой `vagrant up`
```
vagrant up
```



#### 4. [[⬆]](#toc) <a name='recommended_sources'>Дополнительные источники</a>

1. Весь Linux Для тех, кто хочет стать профессионалом, стр.697

