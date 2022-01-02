# Performance optimization.

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
   

# Задание.
``` 
[x] Добавить индексы в свой проект.
[x] Сравнить производительность запросов.
```
# Выполнение.

## Пример документа в коллекции.
Коллекция содержит записи магазинов, ресторанов, и других общественных мест.

```js
db.user.findOne()
{ _id: ObjectId("61cc65d220951eab5320ae22"),
  business_id: '6iYb2HFDywm3zjuRg0shjw',
  name: 'Oskar Blues Taproom',
  address: '921 Pearl St',
  city: 'Boulder',
  state: 'CO',
  postal_code: '80302',
  latitude: 40.0175444,
  longitude: -105.2833481,
  stars: 5,
  review_count: 86,
  is_open: 0,
  attributes: 
   { RestaurantsTableService: 'True',
     WiFi: 'u\'free\'',
     BikeParking: 'True',
     BusinessParking: '{\'garage\': False, \'street\': True, \'validated\': False, \'lot\': False, \'valet\': False}',
     BusinessAcceptsCreditCards: 'True',
     RestaurantsReservations: 'False',
     WheelchairAccessible: 'True',
     Caters: 'True',
     OutdoorSeating: 'True',
     RestaurantsGoodForGroups: 'True',
     HappyHour: 'True',
     BusinessAcceptsBitcoin: 'False',
     RestaurantsPriceRange2: '2',
     Ambience: '{\'touristy\': False, \'hipster\': False, \'romantic\': False, \'divey\': False, \'intimate\': False, \'trendy\': False, \'upscale\': False, \'classy\': False, \'casual\': True}',
     HasTV: 'True',
     Alcohol: '\'beer_and_wine\'',
     GoodForMeal: '{\'dessert\': False, \'latenight\': False, \'lunch\': False, \'dinner\': False, \'brunch\': False, \'breakfast\': False}',
     DogsAllowed: 'False',
     RestaurantsTakeOut: 'True',
     NoiseLevel: 'u\'average\'',
     RestaurantsAttire: '\'casual\'',
     RestaurantsDelivery: 'None' },
  hours: 
   { Monday: '11:0-23:0',
     Tuesday: '11:0-23:0',
     Wednesday: '11:0-23:0',
     Thursday: '11:0-23:0',
     Friday: '11:0-23:0',
     Saturday: '11:0-23:0',
     Sunday: '11:0-23:0' },
  postal_code_array: 
   [ '12345',
     '23456',
     '34567',
     [ '1', '2', '3', '4', 5 ],
     { elemMatch: [ '1', '2', '3', '4', 5 ] },
     '1',
     '2',
     '12',
     28,
     3,
     89,
     3,
     21,
     38,
     3,
     80,
     49,
     43,
     92,
     55,
     97 ] }
```

## Запросы для проверки производительности:
1. Поиск определенного места.
```js
db.user.find({"name": "Hilton Boston/Woburn"})
```

2. Поиск всех мест с бесплатным WiFi и с 2 или более звездами.
```js
db.user.find({"attributes.WiFi": "'free'", "stars": {"$lte": 2}})
```

3. Поиск места в штате Флорида от 4 и более звезд, кол-вом отзывов более 100 и разрешением собак. Отсортировать по кол-ву отзывов по убыванию.
```js
db.user.find(
  {"state": "FL", "review_count": {"$gt": 100}, "stars": {"$gte": 4}, "attributes.DogsAllowed": "True"}
).sort({"review_count": -1})
```

## Добавление индексов.
Добавим индексы, для увеличения производительности запросов выше.

1. 
```js
db.user.createIndex({"name": 1})
```

2.
```js
db.user.createIndex({"attributes.WiFi": 1, "stars": 1})
```

3.
```js
db.user.createIndex({"state": 1, "review_count": -1})
```

## Сравнение производительности запросов.
В качестве сравнения производительности будем рассматривать поля метода explain():
1. `nReturned` - Количество документов, возвращаемых по запросу.
2. `totalDocsExamined` - Общее кол-во просмотренных документов.
3. `totalKeysExamined` - Количество просмотренных индексных записей, если индекс использовался.
4. `executionTimeMillis` - Количество миллисекунд, которое потребовалось базе данных для выполнения запроса. Чем ниже это число, тем лучше.

------------------------------------------------
1.1. Без индекса:
```js
db.user.find(
  {"name": "Hilton Boston/Woburn"}
  ).explain("executionStats")
// Результаты:
{ executionStats: 
  { executionSuccess: true,
    nReturned: 2,
    executionTimeMillis: 136,
    totalKeysExamined: 0,
    totalDocsExamined: 160585,
    ...
  }
}
```

1.2. С использованием индекса:
```js
db.user.find(
  {"name": "Hilton Boston/Woburn"}
  ).explain("executionStats")
// Результаты:
{ executionStats: 
  { executionSuccess: true,
    nReturned: 2,
    executionTimeMillis: 1,
    totalKeysExamined: 2,
    totalDocsExamined: 2,
    ...
  }
}
```
------------------------------------------------
2.1. Без индекса:
```js
db.user.find(
  {"attributes.WiFi": "'free'", "stars": {"$lte": 2}}
  ).explain("executionStats")
// Результаты:
{ executionStats: 
  { executionSuccess: true,
    nReturned: 216,
    executionTimeMillis: 158,
    totalKeysExamined: 0,
    totalDocsExamined: 160585,
    ...
  }
}
```

2.2. С использованием индекса:
```js
db.user.find(
  {"attributes.WiFi": "'free'", "stars": {"$lte": 2}}
  ).explain("executionStats")
// Результаты:
{ executionStats: 
  { executionSuccess: true,
    nReturned: 216,
    executionTimeMillis: 2,
    totalKeysExamined: 216,
    totalDocsExamined: 216,
    ...
  }
}
```
------------------------------------------------
3.1. Без индекса:
```js
db.user.find(
  {"state": "FL", "review_count": {"$gt": 100}, "stars": {"$gte": 4}, "attributes.DogsAllowed": "True"}
  ).sort({"review_count": -1}
  ).explain("executionStats")
// Результаты:
{ executionStats: 
  { executionSuccess: true,
    nReturned: 252,
    executionTimeMillis: 168,
    totalKeysExamined: 0,
    totalDocsExamined: 160585,
    ...
  }
}
```

3.2. С использованием индекса:
```js
db.user.find(
  {"state": "FL", "review_count": {"$gt": 100}, "stars": {"$gte": 4}, "attributes.DogsAllowed": "True"}
  ).sort({"review_count": -1}
  ).explain("executionStats")
// Результаты:
{ executionStats: 
  { executionSuccess: true,
    nReturned: 252,
    executionTimeMillis: 14,
    totalKeysExamined: 2727,
    totalDocsExamined: 2727,
    ...
  }
}
```
Судя по результатам выбран был не лучший индекс, так как разница между `totalDocsExamined` и `nReturned` довольно велика, попробуем использовать индекс:
```js
db.user.createIndex({"state": 1, "review_count": -1, "stars": 1, "attributes.DogsAllowed": 1})
```

```js
db.user.find(
  {"state": "FL", "review_count": {"$gt": 100}, "stars": {"$gte": 4}, "attributes.DogsAllowed": "True"}
  ).sort({"review_count": -1}
  ).explain("executionStats")
// Результаты:
{ executionStats: 
  { executionSuccess: true,
    nReturned: 252,
    executionTimeMillis: 9,
    totalKeysExamined: 1045,
    totalDocsExamined: 252,
    ...
  }
}
```