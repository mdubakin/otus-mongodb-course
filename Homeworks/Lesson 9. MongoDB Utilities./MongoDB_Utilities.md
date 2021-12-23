# MongoDB Utilities.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
   

# Задание.
```
1. Ответить на вопросы:
  - [x] 1.1. Чем отличаются друг от друга mongorestore / mongodump от mongoimport / mongoexport соответственно?
  - [x] 1.2. Формат URI для подключения к кластеру.
  - [x] 1.3. Как не получать ошибки при восстановлении данных из бэкапа? Для mongorestore? Для mongoimport? 
2. Поработать с утилитами:
  - [x] mongotop
  - [] mongostat
  - [] mongofiles
  - [] mongodump
  - [] mongorestore
  - [] mongoexport
  - [] mongoimport
  - [] bsondump
```
# Выполнение.

## 1. Ответы на вопросы.

### 1.1. Отличия `mongorestore` / `mongodump` от `mongoimport` / `mongoexport`.
Для выгрузки (бэкапа) данных, можно использовать утилиты `mongodump` и `mongoexport`.
Эти 2 варианта отличаются тем, что утилита `mongodump` экспортирует данные в BSON формате, а утилита `mongoexport` в формате JSON.

Для загрузки (восстановления) данных, можно использовать утилиты `mongorestore` и `mongoimport`.
Эти 2 варианта отличаются тем, что утилита `mongorestore` загружает данные в BSON формате, а утилита `mongoimport` в формате JSON.

### 1.2. Формат URI для подключения к кластеру.
```js
mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/database
```

### 1.3. Как не получать ошибки при восстановлении данных из бэкапа? Для mongorestore? Для mongoimport?
Для обеих утилит использовать опцию `--drop`, которая удалит все данные из указанной базы данных / коллекции, перед тем как начать ее восстановление.
Пример:
```js
mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump

mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json
```

## 2. Утилиты.

### 2.1. mongotop
Утилита для просмотра информации о чтении и записи в коллекции.

Пример:
1. `mongotop` # Если база слушает локалхост. Информация обновляется каждые 2 секунды.
2. `mongotop 10` # Информация обновляется каждые 10 секунд.
3. `mongotop 60 --host mongodb-host.example.com -u root -p StronGPa$$word --authenticationDatabase admin` # Удаленное подключение к базе для просмотра информации о чтении и записи. Информация обновляется каждую минуту.

References:
- https://docs.mongodb.com/database-tools/mongotop/
- https://www.youtube.com/watch?v=94LmMElo0hI&list=PLWkguCWKqN9PZkdmzgS6TuwwQGNFWts2K