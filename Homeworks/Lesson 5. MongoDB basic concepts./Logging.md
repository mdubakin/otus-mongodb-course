# Logging.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
   

# Задание.
```
[]
```
# Выполнение. 
### Виды сообщений логирования.
![Process log.](https://i.ibb.co/K7fcfFW/image.png)

### Настройка verbosity логирования.
1. Получить информацию об уровне логирования:
db.getLogComponents()
2. Настроить уровень логирования:
   1. Через MongoDB Shell:
      ```js
      db.setLogLevel(0, "index")
      ```
   2. В файле конфигурации:
      ```yaml
      systemLog:
        path: "/data/log/mongod.log"
        destination: "file"
        verbosity: 1
        component: 
          query:
            verbosity: 2
          storage:
            verbosity: 2
            journal:
              verbosity: 1
       ```

### Уровни verbosity.

1. `-1` - наследованно выше.
2. `0` - только информационные сообщения. Значение по-умолчанию.
3. `1 - 5` - debug уровень. Чем выше, тем больше информации.

### Уровни severity лога.

1. `F` - Fatal.
2. `E` - Error.
3. `W` - Warning.
4. `I` - Informational.
5. `D1 - D5` - Debug.

### Разбор лога.

`2022-01-05T09:32:02.140+0000` (**timestamp**) `I` (**severity**) `COMMAND` (**log component**) `[conn535]` (**connection**) `command somedb.$cmd` (**action and namespace**) `command: update { update: "account", ordered: true, $db: "somedb" }` (**operation**) `numYields:0 reslen:364 locks:{ Global: { acquireCount: { r: 4, w: 2 } }, Database: { acquireCount: { w: 2 } }, Collection: { acquireCount: { w: 2 } } } storage:{} protocol:op_query 104ms` (**operation metadata**)