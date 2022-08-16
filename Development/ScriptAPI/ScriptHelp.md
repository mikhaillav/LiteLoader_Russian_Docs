<!-- translated -->

# LLSE - Основная документация по основному скриптовому интерфейсу

> Вот некоторые обычно используемые **вспомогательные функции**, такие как регистрация плагина, логирование, асинхронные интерфейсы и т.д.

Они делают процесс разработки более простым и удобным, и позволяют избегать множества ненужных действий.

## 🎯 API регистрации плагина

Прежде, чем начать писать код для своего плагина, вам сначала необходимо предоставить загрузчику некоторую информацию связанную с плагином.

`ll.registerPlugin(name, introduction, version, otherInformation)`

- Параметру: 

  - name : `String`  
    Название плагина.
  - introduction : `String`  
    Небольшое описание плагина.
  - version : `Array<Integer,Integer,Integer>`  
    Версия плагина
  - other : `Object<string,string>`  
    Дополнительная информация, такие как лицензия, ссылка на исходники и другое.

Среди них информация о версии представляет собой массив номеров номеров версий, например `[2,0,1]` указывает, что номер версии равен 2.0.1.
Если вы не передадите действительную информацию о версии, для номера версии плагина будет установлено значение по умолчанию «1.0.0».

Для дополнительной информации о плагине вы можете передать любую информацию, необходимую для информирования пользователя, в том же формате, что и пара ключ-значение `Object`. Конкретные данные пары ключ-значение должны иметь формат `String`.

<br>

## 💼 Скриптовый помощник

Следующие API добавляют в скрипты необходимые вспомогательные интерфейсы.

### Вывод информации в консоль

`log(data1,data2,...)`  

- Параметр:
  - data1,data2...:
    Данные для вывода, могут быть любым значением/типом.
- Возвращаемое значение: нету

<br>

### Цветной вывод информации в консоль

Это обновленная версия вышеуказанной функции. Она поддерживает цветовой вывод.

`colorLog(color,data1,data2,...)`

- Параметр: 
  - color : `String`  
    Название цвета
  - data1,data2...: 
      Данные для вывода, могут быть любым значением/типом.
- Возвращаемое значение: нету

#### Пример результатов: 

![Пример цветного лога](/assets/colorLog.png)

<br>

### Асинхронный вывод

Эта функция возвращается сразу после отправки запроса на вывод, избегая времени блокировки, вызванного синхронным чтением и записью.
Нижний слой имеет защиту от блокировки, другой `fastLog`. Между ними не будет явления строки.

`fastLog(data1,data2,...)`

- Параметры: 
  - data1,data2...: 
    Данные для вывода, могут быть любым значением/типом.
- Возвращаемое значение: нету 

<br>

### Задержать выполнение функции на определенное время

`setTimeout(func,msec)`

- Параметры: 

  - func : `Function`  
    Функция для выполнения

  - msec : `Integer`  
    Задержка (милисекунды)
- Возвращаемое значение: айди задачи.
- Тип возвращаемого значения: `Integer`
  - Если вернет `Null`, создать задачу не удалось.

<br>

### Задержка выполнения сегмента кода на период времени (Eval)

`setTimeout(code,msec)`

- Параметры: 

  - code : `String`  
    Сегмент кода для выполнения.

  - msec : `Integer`  
    Задержка (милисекунды)
- Возвращаемое значение: айди задачи.
- Тип возвращаемого значения: `Integer`
  - Если вернет `Null`, создать задачу не удалось.

<br>

### Задать переодичность выполнения функции

`setInterval(func,msec)`

- Параметры: 

  - func : `Function`  
    Функция для выполнения

  - msec : `Integer`  
    Переодичность (милисекунды)
- Возвращаемое значение: айди задачи.
- Тип возвращаемого значения: `Integer`
  - Если вернет `Null`, создать задачу не удалось.

<br>

### Задать переодичность выполнения сегмента кода

`setInterval(code,msec)`

- Параметры: 

  - code : `Function`  
    Сегмент кода для выполнения

  - msec : `Integer`  
    Переодичность (милисекунды)
- Возвращаемое значение: айди задачи.
- Тип возвращаемого значения: `Integer`
  - Если вернет `Null`, создать задачу не удалось.

<br>

### Отменить передочиность задачи

`clearInterval(taskid)`

- Параметр: 
  - taskid : `Integer`  
    Айди задачи
- Возвращаемое значение: была ли отмена задачи успешна.
- Тип возвращаемного значения:  `Boolean`
  - Если вернуло `Null`, отмена не удалась.

<br>