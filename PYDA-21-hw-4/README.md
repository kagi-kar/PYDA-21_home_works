# Домашнее задание к лекции «Управляющие конструкции и коллекции» часть 2

# Задание 1

Дана переменная, в которой хранится словарь, содержащий гео-метки для каждого пользователя (пример структуры данных приведен ниже). Вам необходимо написать программу, которая выведет на экран множество уникальных гео-меток всех пользователей.


```python
ids = {'user1': [213, 213, 213, 15, 213],
'user2': [54, 54, 119, 119, 119],
'user3': [213, 98, 98, 35]}
```


```python
geo_lable = set()

for element in ids.keys():
    geo_lable.update(ids.get(element))
print(geo_lable)
```

    {98, 35, 15, 213, 54, 119}
    

# Задание 2

Дана переменная, в которой хранится список поисковых запросов пользователя (пример структуры данных приведен ниже). Вам необходимо написать программу, которая выведет на экран распределение количества слов в запросах в требуемом виде.


```python
queries = [
'смотреть сериалы онлайн',
'новости спорта',
'афиша кино',
'курс доллара',
'сериалы этим летом',
'курс по питону',
'сериалы про спорт',
]
```


```python
distribution = {}

for element in queries:
    if len(element.split(' ')) in distribution.keys():
        distribution[len(element.split(' '))] += 1
    else:
        distribution[len(element.split(' '))] = 1
for dic_elem in distribution.keys():
    print(f"Поисковых запросов, содержащих {dic_elem} слов(а): {round(distribution.get(dic_elem)/sum(distribution.values())*100, 2)}%")      
```

    Поисковых запросов, содержащих 3 слов(а): 57.14%
    Поисковых запросов, содержащих 2 слов(а): 42.86%
    

# Задание 3

Дана переменная, в которой хранится информация о затратах и доходе рекламных кампаний по различным источникам. Необходимо дополнить исходную структуру показателем ROI, который рассчитаем по формуле: (revenue / cost - 1) * 100


```python
results = {
'vk': {'revenue': 103, 'cost': 98},
'yandex': {'revenue': 179, 'cost': 153},
'facebook': {'revenue': 103, 'cost': 110},
'adwords': {'revenue': 35, 'cost': 34},
'twitter': {'revenue': 11, 'cost': 24},
}
```


```python
for dic_elem in results.keys():
    results[dic_elem].update({'ROI':round((((results[dic_elem]['revenue'] /
                                             results[dic_elem]['cost']) - 1) * 100), 2)
                             }
                            )
print(results)
```

    {'vk': {'revenue': 103, 'cost': 98, 'ROI': 5.1}, 'yandex': {'revenue': 179, 'cost': 153, 'ROI': 16.99}, 'facebook': {'revenue': 103, 'cost': 110, 'ROI': -6.36}, 'adwords': {'revenue': 35, 'cost': 34, 'ROI': 2.94}, 'twitter': {'revenue': 11, 'cost': 24, 'ROI': -54.17}}
    

# Задание 4

Дана переменная, в которой хранится статистика рекламных каналов по объемам продаж (пример структуры данных приведен ниже). Напишите программу, которая возвращает название канала с максимальным объемом продаж.


```python
stats = {'facebook': 55, 'yandex': 115, 'vk': 120, 'google': 99, 'email': 42, 'ok': 98}
```


```python
for key_stats in stats.keys():
    if stats[key_stats] == max(stats.values()):
        print(f"Результат:\nмаксимальный объем продаж на рекламном канале: {key_stats}")
        break    
```

    Результат:
    максимальный объем продаж на рекламном канале: vk
    

# Задание 5

Дан список произвольной длины. Необходимо написать код, который на основе исходного списка составит словарь такого уровня вложенности, какова длина исхондого списка.


```python
my_list = ['2018-01-01', 'yandex', 'cpc', 100]
```


```python
dic_1 = {}
# записываем в словарь последние 2 значения из списка
dic_2 = {my_list[len(my_list)-2]:
         my_list[len(my_list)-1]}
i = 0 # обнуление счетчика

# перебираем все значения списка с конца
for key_list in reversed(my_list):
    i += 1 # счетчик, для определения момента, когда цикл пропустит 2 последних значения (т.к. они уже внесены в список)
    if len(my_list) == 2:
        break # если в списке только 2 значения - дальнешие операции не нужны, мы уже их записали в dic_2
    elif i <= 2:
        continue # пропускаем без действий последние 2 значения списка
    else:    
        dic_1.update({key_list:dic_2.copy()})   # добавляем в пустой dic_1 вложением копию dic_2 (если приравнять - словари 1 и 2
                                                # станут ссылаться на одно значение в памяти, а значит все действия над одним
                                                # отразятся и на другом словаре)
        dic_2 = dic_1.copy()                    # итоговый словарь с вложением копируем для дальнейших итераций в 1
        dic_1.clear()                           # стрираем словарь 1 
print(dic_2)
```

    {'2018-01-01': {'yandex': {'cpc': 100}}}
    


```python
my_list = ['a', 'b', 'c', 'd', 'e', 'f']
```


```python
dic_1 = {}
dic_2 = {my_list[len(my_list)-2]:
         my_list[len(my_list)-1]}
i = 0

for key_list in reversed(my_list):
    i += 1
    if len(my_list) == 2:
        break
    elif i <= 2:
        continue
    else:    
        dic_1.update({key_list:dic_2.copy()})
        dic_2 = dic_1.copy()
        dic_1.clear()
print(dic_2)
```

    {'a': {'b': {'c': {'d': {'e': 'f'}}}}}
    

# Задание 6

Дана книга рецептов с информацией о том, сколько ингредиентов нужно для приготовления блюда в расчете на одну порцию (пример данных представлен ниже).
Напишите программу, которая будет запрашивать у пользователя количество порций для приготовления этих блюд и отображать информацию о суммарном количестве требуемых ингредиентов в указанном виде.


```python
cook_book = {
'салат': [
{'ingridient_name': 'сыр', 'quantity': 50, 'measure': 'гр'},
{'ingridient_name': 'томаты', 'quantity': 2, 'measure': 'шт'},
{'ingridient_name': 'огурцы', 'quantity': 20, 'measure': 'гр'},
{'ingridient_name': 'маслины', 'quantity': 10, 'measure': 'гр'},
{'ingridient_name': 'оливковое масло', 'quantity': 20, 'measure': 'мл'},
{'ingridient_name': 'салат', 'quantity': 10, 'measure': 'гр'},
{'ingridient_name': 'перец', 'quantity': 20, 'measure': 'гр'}
],
'пицца': [
{'ingridient_name': 'сыр', 'quantity': 20, 'measure': 'гр'},
{'ingridient_name': 'колбаса', 'quantity': 30, 'measure': 'гр'},
{'ingridient_name': 'бекон', 'quantity': 30, 'measure': 'гр'},
{'ingridient_name': 'оливки', 'quantity': 10, 'measure': 'гр'},
{'ingridient_name': 'томаты', 'quantity': 20, 'measure': 'гр'},
{'ingridient_name': 'тесто', 'quantity': 100, 'measure': 'гр'},
],
'лимонад': [
{'ingridient_name': 'лимон', 'quantity': 1, 'measure': 'шт'},
{'ingridient_name': 'вода', 'quantity': 200, 'measure': 'мл'},
{'ingridient_name': 'сахар', 'quantity': 10, 'measure': 'гр'},
{'ingridient_name': 'лайм', 'quantity': 20, 'measure': 'гр'},
]
}
```


```python
# создаем словарь для того, чтобы в нем агрегировать все ингридиенты
ingrids = {}

persons = input('Введите пожалуйста количество персон:\n')
# последовательно перебираем каждое блюдо
for dish in cook_book:
    # последовательно перебираем каждый словарь, вложенный в список,
    # являющийся значением для соответствующего блюда
    for ingr_name in cook_book[dish]:
        # проверяем, что наименование продукта еще не записано в качестве ключа в словарь ingrids
        if ingr_name.get('ingridient_name') not in ingrids.keys():
            # если такой продукт еще не записывался - записываем его наименование как ключ,
            # а единицы измерения и необходимое количество - как вложенный словарь (ключ/значение соответственно)
            ingrids.update({ingr_name.get('ingridient_name'):
                            {ingr_name.get('measure'):ingr_name.get('quantity')}
                           }
                          )
        # проверка, что продукт добавлен, но единицы измерения отличаются от уже добавленного, в этом случае для
        # продукта создаем в словаре-значении второе значение
        elif ingr_name.get('measure') not in ingrids[ingr_name.get('ingridient_name')].keys():
            ingrids[ingr_name.get('ingridient_name')].update({ingr_name.get('measure'):ingr_name.get('quantity')})
        # если продукт уже добавлялся и единицы измерения совпадают - суммируем добавленное и новое значение количества продукта
        else:         
            ingrids[ingr_name.get('ingridient_name')][ingr_name.get('measure')] += ingr_name['quantity']
print('Требуемый объем ингридиентов:\n')
for elem in ingrids.keys():
    for vol_key in ingrids.get(elem).keys():
        vol = ingrids[elem][vol_key] * 3
        print(f"{elem}: {vol} {vol_key}")
```

    Введите пожалуйста количество персон:
    3
    Требуемый объем ингридиентов:
    
    сыр: 210 гр
    томаты: 6 шт
    томаты: 60 гр
    огурцы: 60 гр
    маслины: 30 гр
    оливковое масло: 60 мл
    салат: 30 гр
    перец: 60 гр
    колбаса: 90 гр
    бекон: 90 гр
    оливки: 30 гр
    тесто: 300 гр
    лимон: 3 шт
    вода: 600 мл
    сахар: 30 гр
    лайм: 60 гр
    
