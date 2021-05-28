# Домашнее задание

<a id='contents'></a>
## Оглавление:
### [Задание 1](#task1)
  * [Пункт 1:](#point1) Переведите содержимое файла purchase_log.txt в словарь purchases вида:
{'1840e0b9d4': 'Продукты', …}
  * [Пункт 2:](#point2) Для каждого user_id в файле visit_log.csv определите третий столбец с категорией покупки (если покупка была, сам файл visit_log.csv изменять не надо). Запишите в файл funnel.csv визиты из файла visit_log.csv, в которых были покупки с указанием категории.

__Учтите условия на данные:__
1. содержимое purchase_log.txt помещается в оперативную память компьютера
1. содержимое visit_log.csv - нет; используйте только построчную обработку этого файла

<a id='task1'></a>
## Задание 1

<a id='point1'></a>
### Пункт 1


```python
# задаем словарь в который будем позже записывать логи
purchases = {}
```


```python
# считываю в память в список весь файл целиком, в режиме для чтения, декодируя из utf-8
purchase_log = open('purchase_log.txt', 'r', encoding = 'UTF-8').readlines()
# начиная со второго элемента, т.к. в 1-ом (0-ой) содержатся заголовки, записываю циклом значения в словарь
for line in purchase_log[1:]:
    purchases.update({list(json.loads(line).values())[0]:list(json.loads(line).values())[1]})
```

[к оглавлению](#contents)

<a id='point2'></a>
### Пункт 2


```python
# передаю открытый файл переменной черех with, чтобы после его выполнения файл автоматически закрылся
with open('visit_log.csv', 'r', encoding = 'UTF-8') as purchase_log:
    # открываю файл для записи через w, т.к. мне важно, чтобы он всякий раз создавался с нуля (обнулялся)
    with open('funnel.csv', 'w', encoding = 'UTF-8') as funnel:
        temp_line = purchase_log.readline().split(',')
        temp_line.insert(1, 'category')
        funnel.write(','.join(temp_line))
        for i, line in enumerate(purchase_log):
            # ввожу промежуточную переменную для облегчения кода, в которую записываю строку
            # пробелы и переносы строки не удаляю, чтобы потом не добавлять, а иметь возможность
            # прямо записать в csv; только делю по запятой
            temp_line = line.split(',')
            if temp_line[0] in purchases:
                # добавляю к списку в середину данные о категории покупки
                temp_line.insert(1, purchases.get(temp_line[0]))
                funnel.write(','.join(temp_line))
```

[к оглавлению](#contents)
