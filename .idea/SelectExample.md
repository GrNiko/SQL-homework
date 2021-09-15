#### Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол.
SELECT model, speed, hd FROM PC WHERE price<500

#### Найдите производителей принтеров.
SELECT DISTINCT maker FROM Product WHERE type = 'Printer'

#### Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.
SELECT model, ram, screen FROM Laptop WHERE price >1000

#### Найдите все записи таблицы Printer для цветных принтеров.
SELECT * FROM Printer WHERE color = 'y'

#### Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
SELECT model, speed , hd FROM PC WHERE price <600 AND cd IN ('12x', '24x')

#### Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.
SELECT DISTINCT Product.maker , Laptop.speed
FROM Laptop
INNER JOIN Product
ON Laptop.model = Product.model
WHERE hd>=10

###upd. 1
#### Найдите производителя, выпускающего ПК, но не ПК-блокноты.
SELECT maker FROM Product WHERE type = 'PC'
EXCEPT
SELECT maker FROM Product WHERE type = 'Laptop'

#### Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker
SELECT DISTINCT maker FROM Product
INNER JOIN PC ON PC.model = Product.model
WHERE PC.speed >= 450

#### Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price
SELECT model, price FROM Printer WHERE price =(SELECT MAX(price) FROM Printer)

#### Найдите среднюю скорость ПК.
SELECT AVG(speed) FROM PC

#### Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
SELECT AVG(speed) FROM Laptop WHERE price >1000

#### Найдите среднюю скорость ПК, выпущенных производителем A.
SELECT AVG(speed) FROM PC
INNER JOIN Product ON PC.model = Product.model
WHERE Product.maker = 'A'

#### Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).
SELECT pr.model, pc.price FROM PC pc, Product pr
WHERE pc.model=pr.model AND maker = 'B'
UNION
SELECT pr.model, lt.price FROM Laptop lt, Product pr
WHERE lt.model=pr.model AND maker = 'B'
UNION
SELECT pr.model, pt.price FROM Printer pt, Product pr
WHERE pt.model=pr.model AND maker = 'B'

#### Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD
SELECT hd FROM PC
GROUP BY hd
HAVING COUNT(hd) >1

####Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
SELECT DISTINCT a.model, b.model , a.speed , a.ram FROM PC a, PC b
WHERE a.model>b.model AND a.speed = b.speed AND a.ram= b.ram

####Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК. Вывести: type, model, speed
SELECT DISTINCT pr.type, pr.model , lt.speed FROM Product pr, Laptop lt
WHERE pr.model = lt.model AND lt.speed < ALL (SELECT speed FROM PC)

####Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price
SELECT DISTINCT pr.maker, pt.price FROM Product pr, Printer pt
WHERE pr.model = pt.model AND pt.color = 'y' AND pt.price = (SELECT MIN(price) FROM Printer WHERE color = 'y')

#### Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.Вывести: maker, средний размер экрана.
SELECT pr.maker, AVG(lp.screen) FROM Product pr, Laptop lp
WHERE pr.model=lp.model
GROUP BY maker

###end of working in DB PC company, start DB Ships

#### Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.
SELECT cl.class, ship.name, cl.country FROM Classes cl, Ships ship
WHERE cl.class= ship.class AND cl.numGuns >= 10
