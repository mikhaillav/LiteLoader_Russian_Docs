<!-- working -->
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

After the setup is complete, you can use the function here to output the log.

`logger.log(data1,data2,...)` -> Output normal text  
`logger.debug(data1,data2,...)` -> Output debugging information  
`logger.info(data1,data2,...)`  -> Output prompt information  
`logger.warn(data1,data2,...)`  -> Output warning message  
`logger.error(data1,data2,...)`  -> Output error messages  
`logger.fatal(data1,data2,...)`  -> Output critical error message

- Parameter:
  - Variable or data to be output  
    Can be of any type, and the number of parameters can be any number.
- Return value: none 

Among them, **ordinary text** will be output as it is when output, while other output interfaces will append the **current time and log type.**
For example: you call `logger.error("Fail to transport the player")`  
The result of the log output is: 

```
[2021-05-21 19:41:03 Error] Fail to transport the player
```

<br>

### Other Settings

In addition, there are other settings to change the format of the output log 

#### Set custom log message headers  

`logger.setTitle(title)`

- Parameter:
  - title : `String`  
    Set custom headers
- Return value: none 

"Header" is the text at the beginning of the log output entry, which is used to visually distinguish the output source of the log. 
By default, message headers are empty by default, i.e. output without headers. 

For example: set a custom header as `logger.setTitle("LiteLoader")`  
Then the following log output will become like: 

```
20:05:26 ERROR [LiteLoader] Fail to transport the player
```

If you want to turn off the header after setting it, do `logger.setTitle("")`

<br>

#### Unified modification log output level

`logger.setLogLevel(level)`

- Parameter:
  - level : `Integer`  
    Log output level    
- Return value: none 

Unified reset of log output levels for various output directions 

<br>
