# Домашнее задание

<a id='contents'></a>
## Оглавление:
### [Задание 1](#task1)
Печатные газеты использовали свой формат дат для каждого выпуска.
Для каждой газеты из списка напишите формат указанной даты для перевода в объект datetime:  
``The Moscow Times - Wednesday, October 2, 2002
The Guardian - Friday, 11.10.13
Daily News - Thursday, 18 August 1977``
### [Задание 2](#task2)
Дан поток дат в формате YYYY-MM-DD, в которых встречаются некорректные значения:
`stream = [‘2018-04-02’, ‘2018-02-29’, ‘2018-19-02’]`
Напишите функцию, которая проверяет эти даты на корректность. Т. е. для каждой даты возвращает True (дата корректна) или False (некорректная дата).
### [Задание 3](#task3)
Напишите функцию date_range, которая возвращает список дат за период от start_date до end_date. Даты должны вводиться в формате YYYY-MM-DD. В случае неверного формата или при start_date > end_date должен возвращаться пустой список.
### [Задание 4 (бонусное)](#task4)
Ваш коллега прислал код функции:
```python
DEFAULT_USER_COUNT = 3
def delete_and_return_last_user(region, default_list=[‘A100’, ‘A101’, ‘A102’]):
    '''Удаляет из списка default_list последнего пользователя
    и возвращает ID нового последнего пользователя.
    '''
    element_to_delete = default_list[-1]
    default_list.remove(element_to_delete)
    return default_list[DEFAULT_USER_COUNT-2]
```
При однократном вызове этой функции все работает корректно:
```
delete_and_return_last_user(1)
```
``‘A101’``  

однако, при повторном вызове получается ошибка `IndexError: list index out of range.`

Задание:

1. Что значит ошибка list index out of range?
1. Почему при первом запуске функция работает корректно, а при втором - нет?

<a id='task1'></a>
## Задание 1


```python
from datetime import datetime as dt
```


```python
moscow_times = dt.strptime('Wednesday, October 2, 2002', '%A, %B %d, %Y')
print(moscow_times.strftime('%A, %B %d, %Y'))
the_guardian =  dt.strptime('Friday, 11.10.13', '%A, %d.%m.%y')
print(the_guardian.strftime('%A, %d.%m.%y'))
daily_news = dt.strptime('Thursday, 18 August 1977', '%A, %d %B %Y')
print(daily_news.strftime('%A, %d %B %Y'))
```

    Wednesday, October 02, 2002
    Friday, 11.10.13
    Thursday, 18 August 1977
    

[к оглавлению](#contents)

<a id='task2'></a>
## Задание 2


```python
stream = ['2018-04-02', '2018-02-29', '2018-19-02']
```


```python
from datetime import datetime as dt
```


```python
def date_check(in_date):
    '''
    По объявленному значению даты проверяет ее корректность и соответствие формату
    год-месяц-день и выдает true - если дата корректная, false - если дата не существует
    '''
    try:
        dt.strptime(in_date, '%Y-%m-%d')
        return True
    except:
        return False
```


```python
for date_ in stream:
    print(f" {in_date} - {date_check(date_)}")
```

     2018-19-02 - True
     2018-19-02 - False
     2018-19-02 - False
    

[к оглавлению](#contents)

<a id='task3'></a>
## Задание 3


```python
def date_range(start_date, end_date):
    '''
    На основании начальной и конечно даты, вводимых в функции, проверяется соответствие дат формату
    год-месяц-день, а также тому, что дата окончания позже начальной даты и выводит список дат в указанном интервале,
    в случае невыполнения проверки - выводит пустой список
    '''
    from datetime import datetime as dt
    from datetime import timedelta as td
    
    date_list = []
    
    if not date_check(start_date) or not date_check(end_date) or dt.strptime(end_date, '%Y-%m-%d') < dt.strptime(start_date, '%Y-%m-%d'):
        return f"Список всех дат интервала:\n{date_list}"
    
    cur_dt = dt.strptime(start_date, '%Y-%m-%d')
    end_dt = dt.strptime(end_date, '%Y-%m-%d')
    
    while cur_dt <= end_dt:
        date_list.append(cur_dt.strftime('%Y-%m-%d'))
        cur_dt += td(days = 1)
    return f"Список всех дат интервала:\n{date_list}"
```


```python
print(date_range('2019-1-16', '2019-1-15'))
```

    Список всех дат интервала:
    []
    


```python
print(date_range('2019-1-1', '2019-1-15'))
```

    Список всех дат интервала:
    ['2019-01-01', '2019-01-02', '2019-01-03', '2019-01-04', '2019-01-05', '2019-01-06', '2019-01-07', '2019-01-08', '2019-01-09', '2019-01-10', '2019-01-11', '2019-01-12', '2019-01-13', '2019-01-14', '2019-01-15']
    

[к оглавлению](#contents)

<a id='task4'></a>
## Задание 4

1. Что значит ошибка list index out of range?  
__Ответ:__  
Буквально означает, что индекс, по которому происходит обращение отсутствует в допустимом интервале индексов.
2. Почему при первом запуске функция работает корректно, а при втором - нет?  
__Ответ:__  
Из-за того, что при работе функции удаление элементов списка производятся непосредственно в списке заданном в по умолчанию, а не над его копией:
 1. при первом запуске из списка, состоящего из 3х элементов (индексы 0, 1 и 2) удаляется 3ий и остается 2 элемента (индексы 0 и 1). Функция находит 2ой элемент списка (с индексом 1) и возвращает его.
 1. на второй итерации из оставшихся 2х элементов удаляется последний (с индексом 1) и остается только первый элемент (с индексом 0). Соответственно, найти 2ой элемент (с индексом 1) в списке из одного элемента с индексом 0 интерпретатор уже не может и возвращает ошибку.

[к оглавлению](#contents)
