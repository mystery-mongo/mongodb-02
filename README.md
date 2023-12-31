# Домашняя работа по теме "Варианты установки MongoDB"

## Создание виртуальной машины в YandexCloud
Была создана виртуальная машина с ОС Debian через веб-интерфейс. Ключ для ssh добавлен на этапе создания.
![YC](./01.png?raw=true)

## Установка и настройка MongoDB
### Соединение с виртуальной машиной
```
ssh mongo@51.250.101.161
```
![VM](./02.png?raw=true)
### Обновим список пакетов и установим пакеты для импорта ключей
```
sudo apt-get update && sudo apt-get install gnupg curl
```
### Импорт ключей
```
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
### Добавление сведений о репозитории с монго в sources.list
```
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] http://repo.mongodb.org/apt/debian bullseye/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```
### Обновление списка пакетов
```
sudo apt-get update
```
### Установка mongo из репозитория
```
sudo apt-get install -y mongodb-org=7.0.2 mongodb-org-database=7.0.2 mongodb-org-server=7.0.2 mongodb-mongosh=2.0.2 mongodb-org-mongos=7.0.2 mongodb-org-tools=7.0.2
```
### Создание необходимых каталогов и установка прав на запись
```
sudo mkdir /home/mongo-test && sudo mkdir /home/mongo-test/db1 && sudo chmod 777 /home/mongo-test/db1
```
### Запуск экземпляра mongo на порту 48658
```
mongod --dbpath /home/mongo-test/db1 --port 48658 --fork --logpath /home/mongo-test/db1/db1.log --pidfilepath /home/mongo-test/db1/db1.pid
```
![Mongo Start](./03.png?raw=true)
### Локальное соединение
```
mongosh --port 48658
```
![Mongo Connect](./04.png?raw=true)
### Перейти в базу данных
```
use example
```
### Вставить первую запись в документ и просмотреть результат
```
db.clients.insert({'name':'Mongo Company', 'inn': 6600221234124})
```
```
db.clients.find()
```
![Mongo Insert](./05.png?raw=true)
### Проверить список баз данных
```
show databases
```
![Mongo Show](./06.png?raw=true)
### Создать пользователя только для новой базы данных
```
db.createUser( { user: "mystery", pwd: "otus\$123", roles: [ { role: "readWrite", db: "example" } ] } )
```
![Mongo User](./07.png?raw=true)
### Запуск Mongo с внешним ip для возможности подключения извне 
```
mongod --dbpath /home/mongo-test/db1 --port 48658 --fork --logpath /home/mongo-test/db1/db1.log --pidfilepath /home/mongo-test/db1/db1.pid --bind_ip 10.129.0.14
```
Для работы в YandexCloud потребовалось указать ip-адрес машины в локальной сети.

## Удаленное подключение
### Через терминал
```
mongosh --host 51.250.101.161 --port 48658
```
![Mongo Shell](./08.png?raw=true)
### MongoDB Compass
![Mongo Compass](./09.png?raw=true)
### DBeaver
![DBeaver](./10.png?raw=true)
