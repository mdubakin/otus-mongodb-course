# MongoDB Installation.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)

## Задание.
```
[x] 1. Создать новый проект в Google Cloud Platform, например mongo2021-, где yyyymmdd год, месяц и день вашего рождения (имя проекта должно быть уникально на уровне GCP).
[x] 2. Дать возможность доступа к этому проекту пользователю ifti@yandex.ru с ролью Project Editor.
[x] 3. Далее создать инстанс виртуальной машины Compute Engine с дефолтными параметрами.
[x] 4. Добавить свой ssh ключ в GCE metadata.
[x] 5. Зайти удаленным ssh (первая сессия), не забывайте про ssh-add.
[x] 6. Поставить MongoDB.
[x] 7. Зайти клиентом и создать свою БД.
[x] 8. Настроить доступ извне и проверить подключение с ноутбука.

Критерии оценивания:
Выполнение ДЗ: 10 баллов
+2 балл за красивое решение
-2 балл за рабочее решение, и недостатки указанные преподавателем не устранены
```

## Выполнение.

1. Создал проект `mongo2021-otus` в GCP, но забыв переключиться на него создал инстанс базы данных в проекте по-умолчанию `mystical-nimbus-334306` (дальше буду работать в нем из-за недостатка времени на переделываение, если требуется, то пересоздам все в другом проекте.)
2. Выдал права Project Editor для пользователя ifti@yandex.ru к проектам `mystical-nimbus-334306` и `mongo2021-otus`.
3. Создал экземпляр MongoDB с **именем**: `mongodb-01-1a`; и публичным **IP-address**: `34.134.212.20`;
4. Добавил новый ключ и подключился к серверу.
5. Установил и настроил MongoDB через конфигурационный файл `mongod.conf` и `systemd`.
   1. Содержимое mongod.conf: 
    ```
    # mongod.conf

    # for documentation of all options, see:
    #   http://docs.mongodb.org/manual/reference/configuration-options/

    # Where and how to store data.
    storage:
      dbPath: /home/mongo/db1
      journal:
        enabled: true
    #  engine:
    #  mmapv1:
    #  wiredTiger:

    # where to write logging data.
    systemLog:
      destination: file
      logAppend: true
      path: /home/mongo/db1/db1.log

    # network interfaces
    net:
      port: 27017
      bindIp: 0.0.0.0


    # how the process runs
    processManagement:
      pidFilePath: /home/mongo/db1/db1.pid
      timeZoneInfo: /usr/share/zoneinfo

    security:
      authorization: "enabled"

    #operationProfiling:

    #replication:

    #sharding:

    ## Enterprise-Only Options:

    #auditLog:

    #snmp:
    ```
6. Запустил MongoDB, создал пользователя `siteRootAdmin` с админскими правами в базе `admin`.
7. Создал базу данных `otus`, коллекцию `test` и сделал тестовую запись в коллекцию.
8. Подключился через утилиту `mongo` и утилиту `MongoDB Compass` со своего компьютера.