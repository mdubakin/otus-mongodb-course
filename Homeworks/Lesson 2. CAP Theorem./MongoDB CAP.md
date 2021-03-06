# CAP Theorem

### Table of contents:
  - [Задание.](#задание)
  - [Выполнение.](#выполнение)
    - [1. MongoDB и CAP теорема.](#1-mongodb-и-cap-теорема)
    - [2. MongoDB и PACELC теорема.](#2-mongodb-и-pacelc-теорема)
  - [Ссылки на источники.](#ссылки-на-источники)

## Задание.
```
Необходимо написать к каким системам по CAP теореме относится MongoDB.

ДЗ сдается ссылкой на гит, где расположен миниотчет в маркдауне.

Критерии оценки:
- задание выполнено - 10 баллов
- предложено красивое решение - плюс 2 балла
- предложено рабочее решение, но не устранены недостатки, указанные преподавателем - минус 2 балла
```

## Выполнение.

### 1. MongoDB и CAP теорема.

MongoDB — это популярная система управления базами данных NoSQL, в которой данные хранятся в виде документов BSON (бинарный JSON). Она часто используется для больших данных и динамических приложений, работающих в нескольких разных расположениях. В контексте теоремы CAP база данных MongoDB представляет собой хранилище данных CP — она отличается устойчивостью к разделению и обеспечивает согласованность, но не гарантирует доступность.

MongoDB — это система с одним главным узлом, в которой каждый набор реплик (внешняя ссылка) может иметь только один главный узел, принимающий все операции записи. Все остальные узлы в том же наборе реплик выполняют роль вспомогательных узлов — они получают от главного узла копию журнала операций и применяют ее к своим наборам данных. По умолчанию клиенты отправляют запросы на чтение главному узлу, однако они могут настроить параметры предпочтения чтения (внешняя ссылка), позволяющие обращаться для этой цели к вспомогательным узлам.

В случае потери доступа к главному узлу вспомогательный узел с самой последней версией журнала операций выбирается в качестве нового главного узла. После того как все остальные вспомогательные узлы зарегистрируются на новом главном узле, доступ к кластеру восстанавливается. Поскольку в течение этого времени запросы на запись от клиентов не принимаются, данные остаются согласованными во всей сети.


![CAP Theorem](https://miro.medium.com/max/732/1*7mDBUO-j0yws52wZlSxbAg.png)

Рисунок 1. CAP Theorem.

### 2. MongoDB и PACELC теорема.

Согласно теореме PACELC, MongoDB относится к типу PA/EC систем. В основных случаях система гарантирует, что операции чтения и записи будут согласованными.

---------------------------------------------
| DDBS            | P+A |  P+C |	E+L |	E+C |
|-----------------|-----|------|------|-----|
| DynamoDB        | Yes |	     |  Yes |     |	
| Cassandra       | Yes |	     |  Yes |	    |
| Cosmos DB       |     |  Yes |  Yes |     |
| Couchbase       |     |  Yes |  Yes | Yes |
| Riak            | Yes |	     |  Yes |     |	
| VoltDB/H-Store  |     |  Yes |	  	| Yes |
| Megastore       |     |  Yes |	  	| Yes |
| BigTable/HBase  |     |  Yes |	  	| Yes |
| MySQL Cluster   |     |  Yes |	  	| Yes |
| **MongoDB** 	  | Yes |	     |	  	| Yes |
| PNUTS 		      |     |  Yes |	Yes |	    |
| Hazelcast IMDG  | Yes |  Yes |  Yes | Yes |
| FaunaDB         |     |  Yes |	Yes | Yes |
---------------------------------------------
Таблица 1. Популярные базы данных и теорема PACELC.

## Ссылки на источники.
- [**IBM** | Теорема CAP](https://www.ibm.com/ru-ru/cloud/learn/cap-theorem#toc--cap-cp----oVh3rgM9)
- [**Habr** | Всё, что вы не знали о CAP теореме](https://habr.com/ru/post/328792/)
- [**Wikipedia** | PACELC theorem](https://en.wikipedia.org/wiki/PACELC_theorem)
- [**Medium** | What is the CAP Theorem? MongoDB vs Cassandra vs RDBMS, where do they stand in the CAP theorem?](https://bikas-katwal.medium.com/mongodb-vs-cassandra-vs-rdbms-where-do-they-stand-in-the-cap-theorem-1bae779a7a15)