# Домашнее задание

<a id='contents'></a>
## Оглавление:
### [Задание 1](#task1)
Напишите функцию, которая возвращает название валюты (поле ‘Name’) с максимальным значением курса с помощью сервиса https://www.cbr-xml-daily.ru/daily_json.js
### [Задание 2](#task2)
Добавьте в класс Rate параметр diff (со значениями True или False), который в случае значения True в методах курсов валют (eur, usd итд) будет возвращать не курс валюты, а изменение по сравнению в прошлым значением. Считайте, self.diff будет принимать значение True только при возврате значения курса. При отображении всей информации о валюте он не используется.
### [Задание 3](#task3)
Напишите класс Designer, который учитывает количество международных премий. Подсказки в коде занятия в разделе “Домашнее задание задача 3”.  
__Комментарий по классу Designer такой:__
Напишите класс Designer, который учитывает количество международных премий для дизайнеров (из презентации: “Повышение на 1 грейд за каждые 7 баллов. Получение международной премии – это +2 балла”). Считайте, что при выходе на работу сотрудник уже имеет две премии и их количество не меняется со стажем (конечно если хотите это можно вручную менять).

Класс Designer пишется по аналогии с классом Developer из материалов занятия. Комментарий про его условия Вика написала выше: “Повышение на 1 грейд за каждые 7 баллов. Получение международной премии – это +2 балла”

<a id='task1'></a>
## Задание 1


```python
def max_currency():
    
    import requests
    
    max_value = 0
    max_name = ''
    
    '''
    Функция, которая возвращает название валюты
    с максимальным значением курса с помощью сервиса
    https://www.cbr-xml-daily.ru/daily_json.js
    '''

    in_data = requests.get('https://www.cbr-xml-daily.ru/daily_json.js')
    for curent_data in in_data.json()['Valute'].values():
        if curent_data.get('Value') >= max_value:
            max_name = curent_data.get('Name')
            max_value = curent_data.get('Value')
    return max_name
```


```python
max_currency()
```




    'СДР (специальные права заимствования)'



[к оглавлению](#contents)

<a id='task2'></a>
## Задание 2


```python
from libs.exchange import Rate
```


```python
class DiffCurrency(Rate):
    def __init__(self, format_ = 'value', diff = False):
        super().__init__(format_)
        self.format = format_
        self.diff = diff
        
    
    def check_in(self):
        """
        Проверяет вводные параметры; если format_ = value, а diff = True генерирует True для
        последующих методов, позволяющих вернуть изменение курса соответствующей валюты по сравнению
        с прошлым значением
        """
        if self.format == 'value' and self.diff:
            return True
        else:
            return False
    
    def prev_value(self, currency):
        """
        Возвращает информацию о курсе валюты currency на предыдущую дату
        """
        response = self.exchange_rates()
        
        if currency in response:
            return response[currency]['Previous']
        return 'Error'
               
    def eur(self):
        """
        Возвращает курс евро на сегодня в формате self.format или
        его изменение по сравнению с прошлым значением, если параметр
        format_ = value, а diff = True
        """
        if self.check_in():
            return round(self.make_format('EUR') - self.prev_value('EUR'), 3)
        else:
            return self.make_format('EUR')
    
    def usd(self):
        """
        Возвращает курс евро на сегодня в формате self.format или
        его изменение по сравнению с прошлым значением, если параметр
        format_ = value, а diff = True
        """
        if self.check_in():
            return round(self.make_format('USD') - self.prev_value('USD'), 3)        
        else:
            return self.make_format('USD')

    def AZN(self):
        """
        Возвращает курс евро на сегодня в формате self.format или
        его изменение по сравнению с прошлым значением, если параметр
        format_ = value, а diff = True
        """
        if self.check_in():
            return round(self.make_format('AZN') - self.prev_value('AZN'), 3)       
        else:
            return self.make_format('AZN')
```

включаем режим выгружки полной информации по курсам валют


```python
a = DiffCurrency('full', True)
```


```python
a.usd()
```




    {'ID': 'R01235',
     'NumCode': '840',
     'CharCode': 'USD',
     'Nominal': 1,
     'Name': 'Доллар США',
     'Value': 73.4979,
     'Previous': 73.2411}




```python
a = DiffCurrency('value', True)
```

включаем режим выгрузки именения курса валюты по сравнению с прошлым периодом


```python
a.usd()
```




    0.257



[к оглавлению](#contents)

<a id='task3'></a>
## Задание 3

Копипаст из практики, для выполнения ДЗ:


```python
class Employee:
    def __init__(self, name, seniority):
        self.name = name
        # стаж работы
        self.seniority = seniority
        # грейд (прием на работу начинается с 1-го грейда)
        self.grade = 1
    
    def grade_up(self):
        """Повышает грейд сотрудника"""
        self.grade += 1
    
    def publish_grade(self):
        """Публикация результатов аккредитации сотрудников"""
        print(self.name, self.grade)
```

Тело домашнего задания:


```python
class Designer(Employee):
    def __init__(self, name, seniority, awards = 0):
        # формально нам не требуется name, однако, чтобы воспользоваться переменной 
        # grade, нам необходимо описать все родительские переменные или явно задав им значения в
        # super (через = ...) или указав, что переменная из родительской функции будет задана в дочерней (перечисляем ее в init дочерней);
        # существует третий вариант - задавать переменную grade, однако в этом случае нам вообще не
        # нужен тогда дочерний класс Designer и его можно задать как второй родительский
        super().__init__(name, seniority)
        self.name = name
        # принимаем указанное число международных премий awards, добавляет к ним
        # 2 премии, которые имеет любой принимаемый на работу дизайнер;
        self.awards = awards + 2
    
    def check_if_it_is_time_for_upgrade(self):
        '''
        метод считает баллы:
        - условно принимаем, что каждый год дизайнер проходит одну аккредитацию (= 1 балл);
        - каждая премия равна 2 баллам
        далее баллы используются для расчета возможности повышения грейда через self.grade_up()
        '''
        self.seniority += 1
        
        # условие повышения дизайнера
        if (self.awards * 2 + self.seniority) % 7 == 0:
            self.grade_up()
        
        # публикация результатов
        return self.publish_grade()
```

[к оглавлению](#contents)


```python
a = Designer('Николай', 0, 0)
```


```python
print (f'имя: {a.name}, премий: {a.awards}, грейд: {a.grade}, стаж: {a.seniority}')
```

    имя: Николай, премий: 2, грейд: 1, стаж: 0
    


```python
for i in range(20):
    a.check_if_it_is_time_for_upgrade()
```

    Николай 1
    Николай 1
    Николай 2
    Николай 2
    Николай 2
    Николай 2
    Николай 2
    Николай 2
    Николай 2
    Николай 3
    Николай 3
    Николай 3
    Николай 3
    Николай 3
    Николай 3
    Николай 3
    Николай 4
    Николай 4
    Николай 4
    Николай 4
    


```python
print (f'имя: {a.name}, премий: {a.awards}, грейд: {a.grade}, стаж: {a.seniority}')
```

    имя: Николай, премий: 2, грейд: 4, стаж: 20
    
