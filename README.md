# procaparser_sjob.py
Extracts production calendar from https://superjob.ru  
Parsed result is represented as a dictionary in JSON file(s)  

Парсит производственный календарь разных лет с сайта https://superjob.ru  
Результат сохраняет структурой словаря в JSON файл(ы)

## Example of result dictionary structure

```python
{
    2023: {
            1: {
                'name': 'Январь',
                'total': 31,
                'restdays': 14,
                'workdays': 17,
                'days': { 
                        1: {
                            'day_num': 1,
                            'wday_num': 7,
                            'wday_str': 'Воскресенье',
                            'dtype_num': 3,
                            'dtype_str': 'Праздничный день',
                            },
                        nd: {
                            ...  # next day nd/total
                            },
                        }
                },
            nm: {
                ...  # next month nm/12
                },
        }
}
```
# proca.py module
Module contains class for working with production calendar in JSON files  
JSON files should be prepared with procaparser 
Or just get prepared JSON files stored in this repository

Модуль proca содержит класс для удобной работы с производственными календарями в JSON файлах
JSON файлы можно приготовить парсером procaparser
Можно взять готовый JSON файл данных с этого репозитория

# proca_examples.py
More python3 code examples of using proca is in proca_examples.py  
Больше примеров кода в файле proca_examples.py  

## Examples of using proca module over JSON file (2023 prod calendar data)

```python
import proca
import json


# Returns dictionary loaded from JSON file
def readJSON(fileName):
    with open(fileName) as fileJSON:
        data = json.load(fileJSON)
    return data


# Construct object 2023 year production calendar
# from source JSON file using ProcaYear
year = proca.ProcaYear(readJSON('sjob_proca_2023.json'))
print(f'Год: {year.year_num}')

# Get month January
january = year.getMonth(1)
# Print some January info
print(f'Месяц: {january.mon_name}')
print(f'Выходных: {january.days_total}, рабочих дней: {january.days_work}')

# Is 7 of January is rest day?
print('7 Января выходной') if january.isRest(7) else print('рабочий день')
# Is work day?
print('31 Января рабочий') if january.isRest(7) else print('выходной день')

# Get day 7 of January
day7 = january.getDay(7)
# Same result by using selector format month.day
day7 = year.select(1.7)
# It' the same
day7 = year.select('01.07')

# What is weekday of 7 of January?
print(f'01.07 день недели {day7["wday_str"]}')

# Print all days of January with formatted table
# field_name: column_width
january.printMe(
    {'day_num': 5, 'wday_str': 12, 'dtype_str': 40},
    gap='-',
    hide=True
)
```
## Result of execution commands above:

```
Год: 2023
Месяц: Январь
Выходных: 31, рабочих дней: 17
7 Января выходной
31 Января рабочий
01.07 день недели Суббота
                                                 
Январь                                                   
1----Воскресенье-Праздничный день. Новогодние каникулы---
2----Понедельник-Праздничный день------------------------
3----Вторник-----Праздничный день------------------------
4----Среда-------Праздничный день------------------------
5----Четверг-----Праздничный день------------------------
6----Пятница-----Праздничный день------------------------
7----Суббота-----Праздничный день. Рождество Христово----
8----Воскресенье-Праздничный день------------------------
9----Понедельник-Рабочий день----------------------------
10---Вторник-----Рабочий день----------------------------
11---Среда-------Рабочий день----------------------------
12---Четверг-----Рабочий день----------------------------
13---Пятница-----Рабочий день----------------------------
14---Суббота-----Выходной--------------------------------
15---Воскресенье-Выходной--------------------------------
16---Понедельник-Рабочий день----------------------------
17---Вторник-----Рабочий день----------------------------
18---Среда-------Рабочий день----------------------------
19---Четверг-----Рабочий день----------------------------
20---Пятница-----Рабочий день----------------------------
21---Суббота-----Выходной--------------------------------
22---Воскресенье-Выходной--------------------------------
23---Понедельник-Рабочий день----------------------------
24---Вторник-----Рабочий день----------------------------
25---Среда-------Рабочий день----------------------------
26---Четверг-----Рабочий день----------------------------
27---Пятница-----Рабочий день----------------------------
28---Суббота-----Выходной--------------------------------
29---Воскресенье-Выходной--------------------------------
30---Понедельник-Рабочий день----------------------------
31---Вторник-----Рабочий день---------------------------- 
```