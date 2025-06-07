# <p align="center"> ![image](https://github.com/user-attachments/assets/19eaae88-976f-492a-b9a1-d7668771a384) </p>

# Тестовое задание (PL/SQL / Oracle) компании ЦФТ

## Общие требования

При выполнении заданий:

1. **Указать версию Oracle**, которую вы используете:
   https://onecompiler.com/plsql/
3. **Приложить скрипты для заполнения таблиц** с тестовыми данными.
4. Если вы принимаете решение **наложить ограничения на условия задачи**, обязательно **укажите причину**, по которой вы приняли это решение.

---

## 🧩 Задание №1: Анаграммы

**Условие:**  
Даны две строки. Необходимо на PL/SQL реализовать алгоритм, определяющий, являются ли эти строки анаграммами (то есть, состоят ли они из одинакового набора символов).

``` sql
CREATE OR REPLACE FUNCTION sort_string(str VARCHAR2) RETURN VARCHAR2 IS
    v_sorted VARCHAR2(4000);
BEGIN
    SELECT LISTAGG(SUBSTR(str, LEVEL, 1), '') WITHIN GROUP (ORDER BY SUBSTR(str, LEVEL, 1))
    INTO v_sorted
    FROM dual
    CONNECT BY LEVEL <= LENGTH(str);
    RETURN v_sorted;
END;
/

CREATE OR REPLACE FUNCTION is_anagram(str1 VARCHAR2, str2 VARCHAR2) RETURN VARCHAR2 IS
BEGIN
    IF LENGTH(str1) != LENGTH(str2) THEN
        RETURN 'NO';
    ELSIF sort_string(LOWER(str1)) = sort_string(LOWER(str2)) THEN
        RETURN 'YES';
    ELSE
        RETURN 'NO';
    END IF;
END;
/

SELECT is_anagram('test','test'); 
SELECT is_anagram('test','test2');
```

---

## 🧩 Задание №2: 
**Условие:**  
Даны три таблицы: Написать запрос, который вернёт для каждого поставщика три услуги, суммы оборотов по которым наиболее высоки. Выборку упорядочить по поставщику и убыванию суммы оборота.

| Поле   | Тип        | Описание                |
| ------ | ---------- | ----------------------- |
| `Id`   | `NUMBER`   | ID поставщика           |
| `Name` | `VARCHAR2` | Наименование поставщика |


| Поле      | Тип        | Описание                                     |
| --------- | ---------- | -------------------------------------------- |
| `Id`      | `NUMBER`   | ID услуги                                    |
| `Name`    | `VARCHAR2` | Наименование услуги                          |
| `Deliver` | `NUMBER`   | ID поставщика (внешний ключ к `DELIVERS.Id`) |


| Поле      | Тип           | Описание                                 |
| --------- | ------------- | ---------------------------------------- |
| `Id`      | `NUMBER`      | ID записи                                |
| `Service` | `NUMBER`      | ID услуги (внешний ключ к `SERVICES.Id`) |
| `Turn`    | `NUMBER(9,2)` | Сумма оборота                            |


