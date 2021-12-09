# MongoDB basic concepts.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
    - [1. Установка MongoDB.](#1-установка-mongodb)
    - [2. Заполнение тестовыми данными.](#2-заполнение-тестовыми-данными)
      - [2.1. Заполнение данными с помощью утилиты `mongoimport`.](#21-заполнение-данными-с-помощью-утилиты-mongoimport)
      - [2.2. Заполнение данными с помощью GUI `MongoDB Compass`.](#22-заполнение-данными-с-помощью-gui-mongodb-compass)
    - [3. Запросы.](#3-запросы)

## Задание.
```
[x] Установить MongoDB на VM В Google Cloud Platform.
[x] Заполнить данными в какой-либо предметной области, например интернет-магазин.
[x] Написать несколько запросов на выборку, обновление и удаление данных.

Сдача ДЗ осуществляется в виде миниотчета в markdown в гите.

Критерии оценивания:
Выполнение ДЗ: 10 баллов
+2 балл за красивое решение
-2 балл за рабочее решение, и недостатки указанные преподавателем не устранены
```

## Выполнение.

### 1. Установка MongoDB.

MongoDB была установлена в домашней работе урока 3. Сейчас она доступна на сервере с **именем**: `mongodb-01-1a`; и публичным **IP-address**: `35.222.107.176` (динамический адрес).

### 2. Заполнение тестовыми данными.

#### 2.1. Заполнение данными с помощью утилиты `mongoimport`.

Используя утилиту `mongoimport` импортировал данные [покупателей торгового центра](https://www.kaggle.com/shwetabh123/mall-customers) в базу данных `mall`:
```bash
mongoimport -h=35.222.107.176:27017 -u siteRootAdmin --authenticationDatabase admin --type csv --file Mall_Customers.csv --headerline -d mall
```

#### 2.2. Заполнение данными с помощью GUI `MongoDB Compass`.

Используя GUI `MongoDB Compass` импортировал данные в базу данных `mall_gui`, порядок действий:
1. Заходим в нужную базу данных и коллекцию.
2. **Documents** --> **ADD DATA** --> **Import File**.
3. Выбираем файл, выбираем тип файла (*csv* в моем случае) и нажимаем **Import**.

### 3. Запросы.

Читаем одну рандомную запись, чтобы получить поля:
```js
db.Mall_Customers.findOne()
```

Создаем новый документ:
```js
db.Mall_Customers.insertOne( {
  CustomerID: 201,
  Genre: 'Female',
  Age: 80,
  'Annual Income (k$)': 20,
  'Spending Score (1-100)': 100 }
  )
```

Читаем созданный документ:
```js
db.Mall_Customers.findOne({ CustomerID: 201 })
```

Добавим несколько документов, при этом один из них существует:
```js
db.Mall_Customers.insertMany([
  {
  _id: ObjectId('61b1a5a384524c8f5901d58b'),
  CustomerID: 201,
  Genre: 'Female',
  Age: 80,
  'Annual Income (k$)': 20,
  'Spending Score (1-100)': 100 
    },
  {
  CustomerID: 202,
  Genre: 'Male',
  Age: 22,
  'Annual Income (k$)': 13,
  'Spending Score (1-100)': 65 
    },
  {
  CustomerID: 203,
  Genre: 'Female',
  Age: 40,
  'Annual Income (k$)': 16,
  'Spending Score (1-100)': 34 
  }]
  )
```
Получаем ошибку: `E11000 duplicate key error collection: mall.Mall_Customers index: _id_ dup key: { _id: ObjectId(\'61b1a5a384524c8f5901d58b\') }`

Чтобы добавить уникальные документы, нужно добавить опцию {ordered:false}:
```js
db.Mall_Customers.insertMany([
  {
  _id: ObjectId('61b1a5a384524c8f5901d58b'),
  CustomerID: 201,
  Genre: 'Female',
  Age: 80,
  'Annual Income (k$)': 20,
  'Spending Score (1-100)': 100 
    },
  {
  CustomerID: 202,
  Genre: 'Male',
  Age: 22,
  'Annual Income (k$)': 13,
  'Spending Score (1-100)': 65 
    },
  {
  CustomerID: 203,
  Genre: 'Female',
  Age: 40,
  'Annual Income (k$)': 16,
  'Spending Score (1-100)': 34 
    }],
  {ordered:false}
  )
```
Мы получим ошибку, но уникальные документы добавятся.

Прочитаем все новые записи:
```js
db.Mall_Customers.find({ CustomerID: {$gt: 200} })
```

Получим все записи покупаетелей женского пола:
```js
db.Mall_Customers.find({ Genre: 'Female' })
```

Получим все записи покупателей которым 20 или больше, но меньше 30:
```js
db.Mall_Customers.find({
  $and: [
    {"Age": {$gte: 20}},
    {"Age": {$lt: 30}}
    ]
  })
```

Получим все записи покупателей которым 18 или больше, но не больше 50 и отсортируем в порядке возрастания:
```js
db.Mall_Customers.find({
  $and: [
    {"Age": {$gte: 18}},
    {"Age": {$lte: 50}}
    ]
}).sort(
  {
    "Age": 1
    }
  )
```

Получим все записи покупателей женского пола, покупательский рейтинг выше или равен 80, отсортируем по возрасту в порядке возрастания и пропустим первые 5 документов:
```js
db.Mall_Customers.find({
  $and: [
    {"Genre": "Female"},
    {"Spending Score (1-100)": {$gte: 80}}
    ]
  }).sort(
    {
    "Age": 1
      }).skip(5)
```

Получим все записи покупателей мужского пола, потративших за год 80 или более тысяч долларов, отсортируем в обратном порядке по полю `CustomerID`, пропустим первые 10 документов и выведем следующие 10 документов:
```js
db.Mall_Customers.find({
  $and: [
    {
      "Genre": "Male"
      },
    {
      "Annual Income (k$)": {$gte: 80}
      }
    ]
  }).sort(
    {
    "CustomerID": -1
      }
  ).skip(10).limit(10)
```

Создадим индекс для поля `CustomerID`:
```js
db.movie.createIndex( 
  {
    "CustomerID" : 1 
    }
  )
```

Посчитаем количество документов о покупателях старше 60 лет:
```js
db.Mall_Customers.countDocuments(
  {
    "Age": {"$gt": 60}
    }
  )
```

Увеличим поле `Annual Income (k$)` для всех мужчин на 10:
```js
db.Mall_Customers.update(
  {
    "Genre": "Male"
    },
  {
    $inc: { "Annual Income (k$)": 10 }
    },
  {
    multi : true
    }
  )
```

Удалим всех покупателей младшей 18 лет:
```js
db.Mall_Customers.deleteMany(
  {
    "Age": {"$lt": 18}
    }
  )
```

Перейдем в базу данных `mall_gui`, удалим коллекцию и базу данных:
```js
use mall_gui
db.Mall_Customers.drop()
db.dropDatabase()
```