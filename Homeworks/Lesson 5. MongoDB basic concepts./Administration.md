# CRUD optimization.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
    - [Basic command groups.](#basic-command-groups)
    - [Basic commands.](#basic-commands)
      - [Работа с базой данных.](#работа-с-базой-данных)
   

# Задание.
```
[x] Basic command groups.
[x] Basic commands.
```
# Выполнение.

## Basic command groups.
```js
1. db.<method>() // для работы с базой данных 
   1. db.<collection>.<method>() // для работы с коллекцией в базе данных
2. rs.<method>() // для работы с ReplicaSet
3. sh.<method>() // для работы с шардированным кластером
```

## Basic commands.

### Работа с базой данных.

User management commands:
```js
db.createUser() // создание пользователя
db.dropUser() // удаление пользователя
```

Collection management commands:
```js
db.<collection>.renameCollection() // переименовать коллекцию
db.<collection>.createIndex() // создать индекс
db.<collection>.drop() // удалить коллекцию
```

Database management commands:
```js
db.dropDatabase() // удаление базы данных
db.createCollection() // создание коллекции
```

Database status command:
```js
db.serverStatus() // получение информации о сервере
```