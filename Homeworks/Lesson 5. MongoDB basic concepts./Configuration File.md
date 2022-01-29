# Configuration File.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
    - [Основные опции конфигурации для командной строки.](#основные-опции-конфигурации-для-командной-строки)
    - [Основные опции конфигурации для конфигурационного файла.](#основные-опции-конфигурации-для-конфигурационного-файла)
   

# Задание.
```
[x] Основные опции конфигурации для командной строки.
[x] Основные опции конфигурации для конфигурационного файла.
[] Доп. конфигурации:
  - [] Для mongos
  - [] Для config servers
  - [] Для шардированного кластера (если есть)
```
# Выполнение. 

## Основные опции конфигурации для командной строки.
```js
mongod --dbpath /data/db --logpath /data/log/mongod.log --fork --replSet "M103" --keyFile /data/keyfile --bind_ip "127.0.0.1,192.168.103.100" --tlsMode requireTLS --tlsCAFile "/etc/tls/TLSCA.pem" --tlsCertificateKeyFile "/etc/tls/tls.pem"
```

## Основные опции конфигурации для конфигурационного файла.
```yaml
storage:
  dbPath: "/data/db"

systemLog:
  path: "/data/log/mongod.log"
  destination: "file"
  logRotate: rename
  verbosity: 0
  # можно указывать verbosity более гранулярно
  # component: 
  #     query:
  #        verbosity: 2
  #     storage:
  #        verbosity: 2
  #        journal:
  #           verbosity: 1

operationProfiling:
   mode: 1
   slowOpThresholdMs: 1000

replication:
  replSetName: ReplicaSetName

net:
  bindIp: "127.0.0.1,192.168.103.100"
  port: 27017
  tls:
    mode: "requireTLS"
    certificateKeyFile: "/etc/tls/tls.pem"
    CAFile: "/etc/tls/TLSCA.pem"

security:
  keyFile: "/data/keyfile"

processManagement:
  fork: false
```