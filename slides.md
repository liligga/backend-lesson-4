---
theme: seriph
background: https://cover.sli.dev
title: Python Class Methods & Static Methods
info: |
  ## Python Class Methods, Static Methods & Class Variables
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
seoMeta:
  ogImage: auto
---

# Добро пожаловать

---

### Git

Ветви в Git - это независимые линии разработки, которые могут развиваться независимо друг от друга.
Как в мультивселенной отдельные вселенные. Часто в проектах используются 

<v-clicks>

- основная ветка (`main` или `master`)
- ветка разработки (`develop`)
- ветки для задач (`feature/issue`)

</v-clicks>

---

### Git

Следующие команды часто используются:

<v-clicks>

- `git branch` - список веток
- `git checkout -b branch_name` - создание новой ветки branch_name и переход на неё
- `git checkout branch_name` - переход на уже существующую ветку branch_name
- `git switch -c branch_name` - создание новой ветки branch_name и переход на неё
- `git switch branch_name` - переход на уже существующую ветку branch_name
- `git merge branch_name` - слияние ветки(соединение одной ветки с другой)
- `git push origin branch_name` - отправка ветки на удаленный репозиторий(если нужно, чтобы ваша ветка была видна другим)

</v-clicks>

<v-clicks>

> Детали:

- Переход с одной ветки на другую **возможен**, когда все изменения сохранены в коммите
- git merge используется при завершении задачи(обычно) или частичном завершении задачи

</v-clicks>

---

# Методы и переменные класса в Python

Class Methods, Static Methods, Class Variables

---
transition: slide-up
---

### Переменные класса vs Переменные экземпляра

**Переменные класса** - общие для всех экземпляров класса

**Переменные экземпляра** - уникальные для каждого объекта

```python
class Car:
    # Переменная класса - общая для всех автомобилей
    total_cars = 0
    wheels = 4

    def __init__(self, brand, model):
        # Переменные экземпляра - уникальны для каждого автомобиля
        self.brand = brand
        self.model = model
        Car.total_cars += 1  # Изменяем переменную класса

# Создание экземпляров
car1 = Car("Toyota", "Camry")
car2 = Car("Honda", "Civic")

print(Car.total_cars)  # 2 - доступ через класс
print(car1.wheels)     # 4 - доступ через экземпляр
print(car1.brand)      # "Toyota" - уникальная для car1
print(car2.brand)      # "Honda" - уникальная для car2
```

---

### Переменные класса - особенности

```python
class Counter:
    count = 0  # Переменная класса

    def __init__(self):
        Counter.count += 1

c1 = Counter()
c2 = Counter()
print(Counter.count)  # 2

# Можно изменить через класс
Counter.count = 10
print(c1.count)  # 10

# Но будьте осторожны с присваиванием через экземпляр!
c1.count = 5  # Создает НОВУЮ переменную экземпляра!
print(c1.count)      # 5 - переменная экземпляра
print(Counter.count) # 10 - переменная класса не изменилась
print(c2.count)      # 10 - у c2 все еще переменная класса
```

---
transition: slide-up
layout: two-cols
---

### Методы класса (Class Methods)

- Используют декоратор `@classmethod`
- Принимают `cls` как первый параметр
- Могут изменять переменные класса
- Могут вызывать другие методы класса
- Не могут изменять экземпляры класса
- Атрибуты и методы экземпляров не доступны
::right::
```python
class BankAccount:
    interest_rate = 0.05
    total_accounts = 0

    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance
        BankAccount.total_accounts += 1

    @classmethod
    def get_interest_rate(cls):
        """Метод класса"""
        return cls.interest_rate

    @classmethod
    def set_interest_rate(cls, new_rate):
        """Изменение переменной класса"""
        cls.interest_rate = new_rate
    @classmethod
    def get_total_accounts(cls):
        return cls.total_accounts

# Вызов через класс
BankAccount.set_interest_rate(0.07)
print(BankAccount.get_interest_rate())  # 0.07
```

---
layout: two-cols
---

### Class Methods - Альтернативные конструкторы

Один из самых популярных паттернов использования `@classmethod`
::right::
```python
class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    @classmethod
    def from_string(cls, date_string):
        """Создание объекта из строки 'YYYY-MM-DD'"""
        year, month, day = map(int, date_string.split('-'))
        return cls(year, month, day)  # Возвращает новый экземпляр

    @classmethod
    def today(cls):
        """Создание объекта с текущей датой"""
        import datetime
        today = datetime.date.today()
        return cls(today.year, today.month, today.day)

# Разные способы создания объекта
date1 = Date(2024, 3, 15)           # Обычный конструктор
date2 = Date.from_string("2024-03-15")  # Из строки
date3 = Date.today()                 # Текущая дата

print(date1)  # 2024-03-15
print(date2)  # 2024-03-15
```

---
transition: slide-up
layout: two-cols
---

### Static Methods - Статические методы

- Используют декоратор `@staticmethod`
- **НЕ** принимают ни `self`, ни `cls`
- Не имеют доступа к переменным экземпляра или класса
- Не имеют доступа к методам экземпляра
- Чаще всего используются для вспомогательных функций:
  - проверки, валидации
  - преобразования
  - вычислений
::right::
```python
class MathHelper:
    PI = 3.14159

    @staticmethod
    def add(x, y):
        """Простая функция без доступа к классу, для вычисления суммы"""
        return x + y

    @staticmethod
    def is_even(number):
        """Проверка на четность"""
        return number % 2 == 0

    @staticmethod
    def validate_email(email):
        """Валидация/проверка правильности написания email"""
        return '@' in email and '.' in email.split('@')[1]

# Использование статических методов
print(MathUtils.add(5, 3))           # 8
print(MathUtils.is_even(10))         # True
print(MathUtils.validate_email("test@example.com"))  # True

# Можно вызывать и через экземпляр, но это редко делается
utils = MathUtils()
print(utils.add(2, 3))  # 5
```

---

### Когда использовать каждый тип метода?

| Тип метода | Когда использовать | Доступ |
|------------|-------------------|---------|
| **Instance Method** | Работа с данными конкретного объекта | `self` - экземпляр |
| **Class Method** | Альтернативные конструкторы, работа с переменными класса | `cls` - класс |
| **Static Method** | Вспомогательные функции, логически связанные с классом | Нет доступа к классу и экземпляру |

---

### Когда использовать каждый тип метода?

```python
class Pizza:
    def __init__(self, ingredients):
        self.ingredients = ingredients

    def __str__(self):
        # Метод экземпляра - использует self
        return f"Pizza with {', '.join(self.ingredients)}"

    @classmethod
    def margherita(cls):
        # Метод класса - альтернативный конструктор
        return cls(['mozzarella', 'tomatoes', 'basil'])

    @classmethod
    def pepperoni(cls):
        return cls(['mozzarella', 'tomatoes', 'pepperoni'])

    @staticmethod
    def validate_ingredient(ingredient):
        # Статический метод - вспомогательная функция для проверки
        return isinstance(ingredient, str) and len(ingredient) > 0
```

---

### Наследование и методы класса

- Методы класса учитывают наследование через `cls`
- __class__ - ссылка на класс
- При использовании `Vehicle.vehicle_count += 1` в `__init__` приведет к тому, что своя переменная `vehicle_count` класса Car не будет изменяться
```python
class Vehicle:
    vehicle_count = 0

    def __init__(self):
        Vehicle.vehicle_count += 1

class Car(Vehicle):
    vehicle_count = 0  # Своя переменная класса

car1 = Car()
car2 = Car()

print(Vehicle.vehicle_count)  # 2
print(Car.vehicle_count)      # 0 !!! не меняется
```

---

### Наследование и методы класса

```python
class Vehicle:
    vehicle_count = 0

    def __init__(self, brand):
        self.brand = brand
        self.__class__.vehicle_count += 1  # Использует класс потомка!

    @classmethod
    def get_count(cls):
        return cls.vehicle_count

class Car(Vehicle):
    vehicle_count = 0  # Своя переменная класса

# Создание объектов
car1 = Car("Toyota")
car2 = Car("Honda")

print(Car.get_count())     # 2
print(Bike.get_count())    # 1
print(Vehicle.get_count()) # 0 - родительский счетчик не изменился
```
