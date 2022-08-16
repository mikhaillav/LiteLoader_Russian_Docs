<!-- translated -->
# LLSE - Документация Вспомогательного Интерфейса

> Большое колличество **вспомогательных функций** предоставлены здесь, включая функции логирования, интерфейсы функций загрузчика и т.д.

Они делают более простой и естественной для вас разработку скриптовых плагинов, а так-же помогают избегать ненужные детали

## 📅 Общее API логгера

В прошлом вывод логов в опледелённом формате и в указанное место был трудным.
Сегодня же LLSE предоставляет вам удобный общий интерфейс логирования.

### Концепция: о уровнях вывода логов

Чтобы оценить приоритет и важность логов, мы ввели концепцию **уровня вывода лога**.
Чем выше уровень вывода лога, тем более подробное содержание лога, но и больше объем информации выводимой в лог.
См. Таблицу ниже для получения более подробной информации:

| Уровень Вывода Лога | Важность Лога | Описание Лога                                       |
| ------------------- | ------------- | --------------------------------------------------- |
| 0                   | Slient        | Нет вывода лога.                                    |
| 1                   | Fatal         | Только сообщения о критических ошибках.             |
| 2                   | Error         | Только ошибки и критические ошибки.                 |
| 3                   | Warn          | Только предупреждения, ошибки и критические ошибки. |
| 4                   | Info          | Всё, кроме отладочной информации.                   |
| 5                   | Debug         | Вся информация.                                     |

С настройкой **уровня вывода лога** вы можете легко отфильтровать ненужную информацию в рабочей среде.

Значение уровня вывода лога по умолчанию составляет `4`, то есть все виды сообщений, кроме информации отладки.
С некоторыми API, приведенными ниже, вы можете настроить уровень вывода лога до желаемого значения.

<br>

### Установка Конфигурации Вывода

Перед использованием общего интерфейса логирования вам необходимо изменить некоторые настройки конфигурации вывода лога в соответствии с вашими потребностями.
Вы можете свободно выбрать отправку журнала в консоль, файл или даже игроку, изменяя настройки.
Например, эти настройки могут существовать одновременно, вы можете отправить лог в консоль и файл одновременно.
Если вы не измените настройки, по **по умолчанию** лог будет выходиться только в консоль.

#### Установить вывод лога в консоль

`logger.setConsole(isOpen[,logLevel])`

- Параметр:
  - isOpen : `Boolean`  
    Установите, будет ли выводиться лог в консоль.
    По умолчанию лог будет выводиться в консоль.  
  - logLevel : `Integer`  
    (опциональный параметр) уровень вывода лога в консоль, по умолчанию `4` 
- Возвращаемое значение: нету 

<br>

#### Установить вывод лога в файл

`logger.setFile(filepath[,logLevel])`

- Параметр:
  - filepath : `String`  
    Установить путь к файлу, куда будет выводиться лог.. 
    Если строка пуста, или значение `Null`, то лог в файл выводиться не будет.
    По умолчанию лог в файл выводиться не будет.
  - logLevel : `Integer`  
    (опциональный параметр) уровень вывода лога в консоль, по умолчанию `4`  
- Возвращаемое значение: нету 

Если вы хотите вывести лог в файл, то мы рекомендуем выводить его в папку `корневая_папка_сервера/logs/` для лёгкой организации и проверки.

<br>

#### Установить вывод лога игроку

`logger.setPlayer(player[,logLevel])`

- Параметр:
  - player : `Player`  
    Установить объект игрока, которому будет выводиться лог.
    Если возвращаемое значение `Null`, вывод лога игроку будет прекращён.
    По умолчанию лог игроку выводиться не будет.
  - logLevel : `Integer`  
    (опциональный параметр) уровень вывода лога в консоль, по умолчанию `4`  
- Возвращаемое значение: нету  

Эта функция разработана для облегчения отладки плагинов. Она выводит лог прямо игроку.

<br>

 ### Функция вывода Логов

После завершения настройки вы можете использовать эту функцию для вывода лога.

`logger.log(data1,data2,...)` -> Вывести обычный текст
`logger.debug(data1,data2,...)` -> Вывести информацию отладки
`logger.info(data1,data2,...)`  -> Вывести общую информацию 
`logger.warn(data1,data2,...)`  -> Вывести предупреждения
`logger.error(data1,data2,...)`  -> Вывести сообщения об ошибках
`logger.fatal(data1,data2,...)`  -> Вывести сообщения об критических ошибках

- Параметр:
  - Переменная или данные для вывода  
    Может быть любого типа, и количество параметров может быть любым.
- Возвращаемое значение: нету

Среди них **обычный текст** будет выводиться таким, какой он есть при выводе, в то время как другие интерфейсы вывода будут добавлять **текущее время и тип лога.**
Например вы используете `logger.error('Не удалось телепортировать игрока')`
В консоли вы получите:

```
[2077-07-07 07:07:07 Error] Не удалось телепортировать игрока
```

<br>

### Другие настройки

Кроме того, есть другие настройки для изменения формата выводимого лога. 

#### Установить пользовательские заголовки логов

`logger.setTitle(title)`

- Параметр:
  - title : `String`  
    Установить пользовательский заголовок
- Возвращаемое значение: нету

«Заголовок» — это текст в начале записи выводимого лога, который используется для визуального определения источника выводимого лога.
По умолчанию заголовки сообщений пусты, т.е. сообщения выводятся без заголовков

Например: установим пользовательский заголовок `logger.setTitle("LiteLoader")`  
В консоли мы получим: 

```
07:07:07 ERROR [LiteLoader] Не удалось телепортировать игрока
```

Если вы хотите отключить заголовок после его установки, используйте `logger.setTitle("")`

<br>

#### Унифицированный уровень вывода лога

`logger.setLogLevel(level)`

- Параметр:
  - level : `Integer`  
    Устанавливаемый уровень вывода лога
- Возвращаемое значение: нету

Унифицированный сброс уровней вывода лога

<br>