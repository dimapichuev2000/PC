# **Статический анализ кода**

**Стати́ческий ана́лиз ко́да** (англ. static code analysis) — анализ программного обеспечения, производимый (в отличие от динамического анализа) 
без реального выполнения исследуемых программ. В большинстве случаев анализ производится над какой-либо версией исходного кода, хотя иногда анализу 
подвергается какой-нибудь вид объектного кода, например P-код или код на MSIL. Термин обычно применяют к анализу, производимому специальным программным обеспечением (ПО),
тогда как ручной анализ называют «program understanding», «program comprehension» (пониманием или постижением программы).

### Список рассматриваемых инструментов для анализа кода Python

- pycodestyle
- pydocstyle
- pyflakes
- vulture

### Pycodestyle
**https://pypi.org/project/pycodestyle/**

Pycodestyle — простая консольная утилита для анализа кода Python, а именно для проверки кода на соответствие PEP8. Один из старейших анализаторов кода, до 2016 года носил название pep8, но был переименован по просьбе создателя языка Python Гвидо ван Россума.

Запустим проверку на нашем коде:

![image](https://user-images.githubusercontent.com/55379930/111757985-487a6f00-88df-11eb-89c6-0143b39317fe.png)

Лаконичный вывод показывает нам строки, в которых, по мнению анализатора, есть нарушение соглашений PEP8. Формат вывода прост и содержит только необходимую информацию:
**имя файла>: <номер строки> :<положение символа>: <код и короткая расшифровка ошибки>**

### Pydocstyle
**https://github.com/PyCQA/pydocstyle**

Утилиту pydocstyle мы уже упоминали в статье Работа с документацией в Python: поиск информации и соглашения. Pydocstyle проверяет наличие docstring у модулей, классов, функций и их соответствие официальному соглашению PEP257.

![image](https://user-images.githubusercontent.com/55379930/111758459-e66e3980-88df-11eb-8e7f-1b7226003d10.png)

Как мы видим из листинга, программа указала нам на отсутствие документации в определениях функции, методов класса и ошибки оформления в docstring класса. Вывод можно сделать более информативным, если использовать ключи --explain и --source при вызове программы. Функционал pydocstyle практически идентичен описанному выше для pycodestyle, различия касаются лишь названий ключей.Как мы видим из листинга, программа указала нам на отсутствие документации в определениях функции, методов класса и ошибки оформления в docstring класса. Вывод можно сделать более информативным, если использовать ключи --explain и --source при вызове программы. Функционал pydocstyle практически идентичен описанному выше для pycodestyle, различия касаются лишь названий ключей.

### Pyflakes
**https://github.com/PyCQA/pyflakeshttps://github.com/PyCQA/pyflakes**

В отличие от уже рассмотренных инструментов для анализа кода Python pyflakes не делает проверок стиля. Цель этого анализатора кода — поиск логических и синтаксических ошибок. Разработчики pyflakes сделали упор на скорость работы программы, безопасность и простоту. Несмотря на то, что данная утилита не импортирует проверяемый файл, она прекрасно справляется c поиском синтаксических ошибок и делает это быстро. С другой стороны, такой подход сильно сужает область проверок.
Функциональность pyflakes — “нулевая”, все что он умеет делать — это выводить результаты анализа в консоль:

![image](https://user-images.githubusercontent.com/55379930/111758793-3baa4b00-88e0-11eb-8381-e2980614246d.png)

В нашем тестовом скрипте, он нашел только импорт не используемого модуля os. Вы можете самостоятельно поэкспериментировать с запуском программы и передачей ей в качестве параметра командной строки Python файла, содержащего синтаксические ошибки. Данная утилита имеет еще одну особенность — если вы используете обе версии Python, вам придется установить отдельные утилиты для каждой из версий.

### Vulture
**https://github.com/jendrikseipp/vulture**

Vulture — небольшая утилита для поиска “мертвого” кода в программах Python. Она использует модуль ast стандартной библиотеки и создает абстрактные синтаксические деревья для всех файлов исходного кода в проекте. Далее осуществляется поиск всех объектов, которые были определены, но не используются. Vulture полезно применять для очистки и нахождения ошибок в больших базовых кодах.


# **Источники**

- https://proglib.io/p/python-code-analysis/
- https://ru.wikipedia.org/wiki/Статический_анализ_кода

