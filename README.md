1. Насколько похорошела Москва

```python
import json

def get_street(address):
    str_without_city = " ".join(address.split()[2:]) # уберем название "город Москва"
    resutl_str = str_without_city[0:str_without_city.find(",")] #"вытащим " название улицы
    result = resutl_str
    if ('город' in resutl_str) or ('поселение' in resutl_str): # т.к. непонятно по условиям че надо делать, допустим так
        result = 'NaN'
    return str(result)

# Load data from files
library_json = json.load(open('library.json', "r"))
cinema_json = json.load(open('cinema.json', "r"))
cultural_json = json.load(open('cultural.json', "r"))
garden_json = json.load(open('garden.json', "r"))

# Соберем все в один датасет
data_set = [get_street(rec['Address']) for rec in library_json]
[data_set.append(get_street(rec['Address'])) for rec in cinema_json]
[data_set.append(get_street(rec['Address'])) for rec in garden_json]
#[print(rec['Address']) for rec in cultural_json] -- ХЗ, тут нет улиц, только название парка, как что определять непонятно =)

# Почистим от NaN
clear_data_set = [rec for rec in data_set if rec != 'NaN']

# Посчитаем количество и сложим в словарик
street_count = [clear_data_set.count(rec) for rec in clear_data_set]
result = sorted(dict(list(zip(clear_data_set, street_count))).items(), reverse=True, key=lambda x: x[1])
print(result[0:5])
```
2. Статистика оказаний услуг
```sql
-- Генерация данных
with data as
(select 
   'Name' || trunc(dbms_random.value(1,5)) name, -- Кто
   'Place' || trunc(dbms_random.value(1,5)) place, -- Где
   sysdate - trunc(dbms_random.value(1,5)) dat,    -- Когда
   'Name' || trunc(dbms_random.value(1,20)) service_name -- Какая услуга
from 
    dual
connect by level <= 200)
-- Странный запрос конечно в условиях, но остальное не требуется по условиям задачи =)
select 
    b1.service_name,
    b1.cnt 
from 
    (select
        service_name,
        count(1) cnt
    from 
        data a1
    group by 
        a1.service_name
    order by 2 desc) b1
where rownum <= 10
```

3. Обновление Python

Если честно, то у меня Windows и я просто ставлю его в отдельную папочку и прописываю везде пути к ней, но давайте порассуждаем о вечном, как обновлять =)
  1) Сделаем sudo apt-get install python3.8
  2) Добавим его к update-alternatives
      sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
      sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 2
  3) Зададим его по умолчанию в 
   sudo update-alternatives --config python3
   и выберем там "2"
  4) Проверим python3 -V и будем надеяться, что нигде не накосячили.

4. Не знаю куда писать промокод, но вдруг прокатит "Промокод: MAMA_ANARKHIA"
