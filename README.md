
Курсовая работа по теме "Разработка базы данных для энциклопедии по игре Genshin Impact"

![Erd1-диаграмма](https://github.com/321Astari123/Course_work/assets/68735994/df3e5aed-417d-4fe9-b636-ec4a904959d4)


## Типовые Запросы
**1. Вывести список всех персонажей определённого элемента**
```sql
SELECT * FROM Characters
join elements ON characters.Elements_id = elements.id
WHERE Elements.name = 'Anemo';
```

**2. Вывести список всего оружия определённого типа**
```sql
SELECT * FROM weapons
join weapontypes ON weapons.WeaponTypes_id = weapontypes.id
WHERE weapontypes.name  = 'Polearm';

```

**3. Вывести список всех персонажей из определённого региона**
```sql
SELECT * FROM Characters
join regions ON characters.regions_id = regions.id
WHERE regions.name = 'Inazuma';
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
> Процедура добавит в таблицу characters имя `Cyno`, тип модели `Medium Male`, id типа используемого оружия `5`, id используемого элемента `3`, id региона `4` и id редкости `5`
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

## Триггеры
1. **`characters_insert`** осуществляет свою деятельность после добавления новой записи в таблицу `characters`, а именно, создание новой записи в столбец `date` с указанием времени и даты создания информационной записи
2. **`characters_update`** осуществляет свою деятельность после обновления уже существующих данных в таблице `characters`, а именно, создание новой записи в столбец `date` с указанием времени и даты обновления информационной записи
## Пользовательская функция 
1. **`FindMostCommonElement `** принимает строку Elements и возвращает наиболее популярный элемент из представленных с его количеством
