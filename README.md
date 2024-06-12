
Курсовая работа по теме "Разработка базы данных для энциклопедии по игре Genshin Impact"

![Erd1-диаграмма](https://github.com/321Astari123/Course_work/assets/68735994/df3e5aed-417d-4fe9-b636-ec4a904959d4)


## Типовые Запросы
**1. Вывести список всех персонажей определённого элемента**
```sql
SELECT * FROM Characters WHERE Elements_id = 1;
```

**2. Вывести список всего оружия определённой редкости**
```sql
SELECT * FROM Weapons WHERE Rarity_ID = 4;
```

**3. Вывести список всех персонажей из определённого региона**
```sql
SELECT * FROM Characters WHERE Regions_id = 4;
```
**4. Вывести список еды которая усиляет урон от атак**
```sql
SELECT * FROM wikip.foods WHERE type = 'ATK-Boosting Dishes'
```
**5.Удаление оружия из таблицы по его ID**
```sql
DELETE FROM Weapons WHERE ID = 8;
```

## Роли
1. Роль: Простой пользователь (User)
``` sql
--создание роли
CREATE ROLE IF NOT EXISTS User;
GRANT SELECT ON characters TO User;
GRANT SELECT ON elements TO User;
GRANT SELECT ON foods TO User;
GRANT SELECT ON rarity TO User;
GRANT SELECT ON regions TO User;
GRANT SELECT ON weapons TO User;
GRANT SELECT ON weapontypes TO User;
GRANT SELECT ON worldbosses TO User;
```
> Пользователи с этой ролью могут только читать таблицы

2. Роль: Администратора (Admin)
 ```sql
 --создание роли
CREATE ROLE IF NOT EXISTS Admin;
GRANT ALL PRIVILEGES ON wikip.* To Admin;
```
> Пользователи с этой ролью имеют все права на таблицы

## Хранимые процедуры
1. Процедура добавления персонажа в таблицу
``` sql
CALL AddNewCharacter('Cyno', 'Medium Male', 5, 3, 4, 5);
```
2. Процедура подсчёта общего количества персонажей в таблице
```sql
set @charactersCount = 0;
call wikip.CountCharacters(@charactersCount);
select @charactersCount;
```

## Представление
```sql
SELECT * FROM CharacterInfo
```
> Это представление выводит всю информацию по персонажам
