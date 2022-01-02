# Advanced CRUD.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
    - [Update operators.](#update-operators)
      - [$inc](#inc)
      - [$set](#set)
      - [$unset](#unset)
    - [Comparison operators.](#comparison-operators)
      - [$eq & $ne](#eq--ne)
      - [$gt & $lt](#gt--lt)
      - [$gte & $lte](#gte--lte)
    - [Logic operators.](#logic-operators)
      - [syntax binary operators ($and, $or, $nor)](#syntax-binary-operators-and-or-nor)
      - [$and](#and)
      - [$or](#or)
      - [$nor](#nor)
      - [$not](#not)
    - [Expressive Query operator ($expr).](#expressive-query-operator-expr)
# Задание.
```
[x] Update operators:
  - [x] $inc
  - [x] $set
  - [x] $unset
[x] Comparison operators:
  - [x] $eq & $ne
  - [x] $gt & $lt
  - [x] $gte & $lte
[x] Logic operators:
  - [x] syntax binary operators ($and, $or, $nor)
  - [x] $and
  - [x] $or
  - [x] $nor
  - [x] $not
[x] Expressive Query operator ($expr).
[] Array operators:
  - [x] $push
  - [x] $size
  - [x] $all
  - [x] $elemMatch
  - [] $in & $nin
[] Projection:
  - [] syntax
  - [] examples
```
# Выполнение.

## Update operators.
### $inc
1. Decription: `$inc` используется для увеличения или уменьшения числового значения.
2. Syntax: 
      ```js
      { $inc: { <field1>: <amount1>, <field2>: <amount2>, ... } }
      ```
3. Example:
    * Увеличим значение `stars` на 1 во всех записях.
      ```js
      db.user.updateMany({}, {"$inc": {"stars": 1}})
      ```
    * Во всех документах штата Флорида увеличим кол-во обзоров на 1 и уменьшим кол-во звезд на 1.
      ```js
      db.user.updateMany({"state": "FL"}, {"$inc": {"review_count": 10, "stars": -1}})
      ```

### $set
1. Decription: `$set` используется для назначения нового значения полю.
2. Syntax:
      ```js
      { $set: { <field1>: <value1>, ... } }
      ```
3. Example:
    * Заменим значение поля `is_open` ресторана с именем `Oskar Blues Taproom` на 1.
      ```js
      db.user.updateOne({"name": "Oskar Blues Taproom"}, {"$set": {"is_open": 0}})
      ```

### $unset
1. Decription: `$unset` используется для удаления поля.
2. Syntax:
      ```js
      { $unset: { <field1>: "", ... } }
      ```
3. Example:
    * Удалим поле `categories` ресторана с именем `Oskar Blues Taproom`.
      ```js
      db.user.updateOne({"name": "Oskar Blues Taproom"}, {"$unset": {"categories": ""}})
      ```

## Comparison operators.
### $eq & &ne
1. Decription: `$eq` и `$ne` используется для проверки значения поля.
2. Syntax:

      `$eq`
      ```js
      { <field>: { $eq: <value> } }
      ```
      `$ne`
      ```js
      { <field>: { $ne: <value> } } 
      ```
3. Example:
    * Найдем все рестораны в городе Austin.
      ```js
      db.user.find({"city": {"$eq": "Austin"}})
      // Этот запрос эквивалентен запросу ниже
      db.user.find({"city": "Austin"})
      ```
    * Найдем все рестораны вне города Austin.
      ```js
      db.user.find({"city": {"$ne": "Austin"}})
      ```

### $gt & $lt
1. Decription: `$gt` (greater that) - строго больше и `$lt` (less than) - строго меньше.
2. Syntax:

      `$gt`
      ```js
      { <field>: { $gt: <value> } }
      ```
      `$lt`
      ```js
      { <field>: { $lt: <value> } } 
      ```
3. Example:
    * Найдем рестораны более чем с 4 звездами.
      ```js
      db.user.find({"stars": {"$gt": 4}})
      ```
    * Найдем рестораны, у которых меньше 10 обзоров.
      ```js
      db.user.find({"review_count": {"$lt": 10}})

### $gte & $lte
1. Decription: `$gte` (greater that or equal) - больше или равно и `$lte` (less than or equal) - меньше или равно.
2. Syntax: такой же как и у `$gt` и `$lt`

## Logic operators.
### syntax binary operators ($and, $or, $nor)
1. Syntax:
    ```js
    { ($and | $or | $nor): [ { <expression1> }, { <expression2> } , ... , { <expressionN> } ] }
    ```

### $and
1. Decription: `$and` - логическое И.
2. Example:
    * Найдем рестораны в Ванкувере с 4 или более звездами.
      ```js
      db.user.find({"$and": [{"city": "Vancouver"}, {"stars": {"$gte": 4}}]})
      // Этот запрос эквивалентен запросу ниже
      db.user.find({"city": "Vancouver", "stars": {"$gte": 4}})
      ```

### $or
1. Decription: `$or` - логическое ИЛИ.
2. Example:
    * Найдем рестораны в Колумбусе или Орландо.
      ```js
      db.user.find({"$or": [{"city": "Columbus"}, {"city": "Orlando"}]})
      ```
  
### $nor
1. Decription: `$or` - логическое ИЛИ-НЕ. Истиной будут все значения, кроме указанных.
2. Example:
    * Найдем рестораны во всех городах кроме Колумбуса и Орландо.
      ```js
      db.user.find({"$nor": [{"city": "Columbus"}, {"city": "Orlando"}]})
      ```

### $not
1. Decription: `$not` - логическое НЕ.
2. Syntax:
      ```js
      { <field>: { $not: { <operator-expression> } } }
      ```
3. Example:
    * Найдем рестораны не более чем с 4 звездами.
      ```js
      db.user.find({"$not": {"stars": {"$gt": 4}}})
      ```

## Expressive Query operator ($expr).
1. Decription: С помощью оператора `$expr` можно выполнять операции между полями внутри документа.
2. Syntax:
      ```js
      { $expr: { <expression> } }
      ```
3. Example:
    * Найдем рестораны, у которых значения полей `stars` и `review_count` будет одинаковыми.
      ```js
      db.user.find({"$expr": {"$eq": ["$stars", "$review_count"]}})
      ```
    * Найдем рестораны, у которых значение поля `stars` будет строго больше значения `review_count`.
      ```js
      db.user.find({"$expr": {"$gt": ["$stars", "$review_count"]}})
      ```

## Array operators.
### $push
1. Decription: С помощью оператора `$push` мы добавляем элемент в массив (создавая поле с типом данных массив, если оно отсутствует).
2. Syntax:
      ```js
      { $push: { <field1>: <value1>, ... } }
      ```
3. Example:
    * Добавим одному ресторану с индексом `80302` массив `postal_code_array` с элементом `12345`.
      ```js
      db.user.updateOne({"postal_code": "80302"}, {"$push": {"postal_code_array": "12345"}})
      ```
    * Добавим одному ресторану с индексом `80302` в массив `postal_code_array` несколько элементов.
      ```js
      db.user.updateOne({"postal_code": "80302"}, {"$push": {"postal_code_array": {"$each": ["1", "2", "12"]}}})
      ```

### $size
1. Decription: Оператор `$size` находит документы с указанной длиной массива.
2. Syntax:
      ```js
      { <array>: { $size: <number> } }
      ```
3. Example:
    * Найдем ресторан с длиной массива `postal_code_array` в 8 элементов.
      ```js
      db.user.find({"postal_code_array": {"$size": 8}})
      ```

### $all
1. Decription: Оператор `$all` позволяет искать документы с указанными элементами массива в произвольном порядке и количестве.
2. Syntax:
      ```js
      { <field>: { $all: [ <value1> , <value2> ... ] } }
      ```
3. Example:
    * Найдем ресторан с элементами массива `postal_code_array` ["34567", "23456", "1"].
      ```js
      db.user.find({"postal_code_array": {"$all": ["34567", "23456", "1"]}})
      ```

### $elemMatch
1. Decription: Оператор `$elemMatch` работает с каждым элементом массива, согласно запросу.
2. Syntax:
      ```js
      { <field>: { $elemMatch: { <query1>, <query2>, ... } } }
      ```
3. Example:
    * Найдем кол-во документов, массив `postal_code_array` которых будет иметь хотя бы один элемент с номер от 19 до 20 включительно.
      ```js
      // функция для генерирования целого числа
      function getRandomInt(max) {
        return Math.floor(Math.random() * max);
      }
      // функция для добавления в документ 5 рандомных чисел
      function setRandomArray(id) {
        db.user.updateOne({"_id": id}, {"$push": {"postal_code_array": {"$each": [getRandomInt(100), getRandomInt(100), getRandomInt(100), getRandomInt(100), getRandomInt(100)]}}})
      }
      // добавим к каждому документу массив postal_code_array с рандомными значениями
      var cursor = db.user.find()
      cursor.forEach(setRandomArray(${doc._id}))

      db.user.find({"postal_code_array": {"$elemMatch": {"$gt": 18, "$lte": 20}}}).count()
      ```
      