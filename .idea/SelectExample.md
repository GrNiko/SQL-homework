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
