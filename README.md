### NFS-FUSE

#### <a name='toc'>Содержание</a>
1. [Создаём виртуальные машины](#create_vm)
2. [Настраиваем сервер NFS](#setting_server)
3. [Настраиваем клиент NFS](#setting_client)
4. [Рекомендуемые источники](#recommended_sources) ???????????

#### 1. [[⬆]](#toc) <a name='create_vm'>Создаём виртуальные машины</a>
![image](https://github.com/user-attachments/assets/728e5599-fca5-4c2f-9c74-dceae04d6914)

#### Результат выполнения команды `vagrant up`
![image](https://github.com/user-attachments/assets/afdd54d7-0c98-4d4b-ba07-2ecc96d83612)

#### 2. [[⬆]](#toc) <a name='setting_server'>Настраиваем сервер NFS</a>
```
sudo apt update
sudo apt install nfs-kernel-server
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
##### Cоздаём в файле /etc/exports структуру, которая позволит экспортировать ранее созданную директорию:
```
cat << EOF > /etc/exports 
/srv/share 192.168.50.11/32(rw,sync,root_squash)
EOF
# Экспортируем ранее созданную директорию:
exportfs -r
# Проверяем экспортированную директорию
exportfs -s
```
![image](https://github.com/user-attachments/assets/0b263e92-221e-4fba-8b7a-fc0806fa7bb2)

#### 3. [[⬆]](#toc) <a name='setting_client'>Настраиваем клиент NFS</a>

#### 4. [[⬆]](#toc) <a name='????'>????????????</a>

#### 5. [[⬆]](#toc) <a name='????'>????????????</a>

#### 6. [[⬆]](#toc) <a name='????'>????????????</a>

#### 7. [[⬆]](#toc) <a name='????'>????????????</a>

#### 8. [[⬆]](#toc) <a name='compression'>????????????</a>
