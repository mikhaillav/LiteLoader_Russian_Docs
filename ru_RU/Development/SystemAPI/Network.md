<!-- working -->

## 🌏 API веб интерфейса

Следущие API предоставляют базовые веб функции для ваших скриптов.
Если есть более сложные потребности, для выполнения задачи можно использовать сетевую библиотеку соответствующей языковой платформы.

### Отправить асинхронный httpGet запрос 

`network.httpGet(url,callback)`

- Параметры: 
  - url : `String`  
    Целевой адресс (включая параметры для создания Get запроса).
  - callback : `Function`  
    Функция обратного вызова, выполняется что бы получить результат запроса.
- Возвращаемое значение: Был ли запрос успешно отправлен.
- Тип возвращаемого значения: `Boolean`.

Примечание. Прототип функции обратного вызова: `function(exitcode,output)`  

- status : `Integer`    
  Возвращает код ответа HTTP, 200 значит что запрос выполнен успешно.
- result : `String`  
  Возвращаемые данные.

Если запрос не удался, `status` будет равен `-1`. 

<br>

### Отправить асинхронный httpPost запрос 

`network.httpPost(url,data,type,callback)`

- Параметры: 
  - url : `String`  
    Адресс назначения запроса.
  - data : `String`  
    Данные для отправки.
  - type : `String`  
    Данные Post запроса могут быть типа `text/plain` или `application/x-www-form-urlencoded`.
  - callback : `Function`  
    Функция обратного вызова, выполняется что бы получить результат запроса.
- Возвращаемое значение: Был ли запрос успешно отправлен.
- Тип возвращаемого значения: `Boolean`.

Примечание. Прототип функции обратного вызова: `function(exitcode,output)`  

- status : `Integer`    
  Возвращает код ответа HTTP, 200 значит что запрос выполнен успешно.
- result : `String`  
  Возвращаемые данные.

Если запрос не удался, `status` будет равен `-1`. 

<br>

## 🔌 API WebSocket клиент 

В LLSE, используйте "обьект WebSocket" для подключения и работы с клиентом WebSocket.

### Создание обьекта WebSocket Client Object 

[JS] `new WSClient()`  
[Lua] `WSClient()`

- Возвращаемое значение: Обьект нового клиента WebSocket.
- Тип возвращаемого значения: `WSClient`

<br>

### Объект клиента WebSocket — свойства

Каждый объект клиента WS содержит некоторые фиксированные свойства. Для определенного объекта `wsc` имеет следующие свойства:

| Атрибут | Значение        | Тип  |
| ---------- | -------------- | ------ |
| wsc.status | Статус подключения | `Enum` |

Эти свойства доступны только для чтения и не могут быть изменены

Среди них wsc.status имеет следующие значения:

`wsc.Open` - Стабльное соединение.  
`wsc.Closing` - Отключение.  
`wsc.Closed` - Отключен.

<br>

### Объект клиента WebSocket — функции

Каждый клиентский объект WS содержит некоторые функции (методы), которые могут быть выполнены. для определенного объекта `wsc` вы можете выполнять некоторые операции с помощью следующих функций.

#### Подключиться

`wsc.connect(target)`

- Параметры: 
  - target : `String`  
    Адрес назначения в следущем формате `ws://hostname[:port][/path/path][?query=value]`.
- Возвращаемое значение: Было ли подключение успешно.
- Тип возвращаемого значения: `Boolean` 

<br>

#### Асинхронно подключиться

`wsc.connectAsync(target,callback)`

- Параметры: 
  - target : `String`  
    Адрес назначения в следущем формате `ws://hostname[:port][/path/path][?query=value]`.
  - callback : `Function`
    Функция обратного вызова, выполняется в случае выполнения (или не выполнения) подключения.
- Возвращаемое значение: Была ли попытка подключиться успешна или нет.
- Тип возвращаемого значения: `Boolean` 

Примечание. Прототип функции обратного вызова: `function(success)`  

- success : `Boolean`    
  Было ли подключение выполнено успешно

<br>

#### Send Text/Binary Messages

`wsc.send(msg)`

- Параметр: 
  - msg : `String` / `ByteBuffer`  
    Text/Binary data to send.
- Return value: Whether it was sent successfully.
- Return value type: `Boolean` 

If the parameter type passed in is `String`, will be sent as text, if it is `ByteBuffer` will be sent as binary data.

<br>

#### Listen for WebSocket Events 

In the process of WS working, when a message is received or an error occurs, the relevant information needs to be processed. Here is the interface for listening to events.

`wsc.listen(event,callback)`

- Параметры: 

  - event : `String`  
    The name of the event to listen for (see the list of listening events below).

  - callback : `Functon`  
    Registered listener function (see below for function-related parameters)
    When the specified event occurs, LLSE will call the listener function you gave and pass in the corresponding parameters.
- Return value: Whether the event was successfully monitored.
- Return value type: `Boolean` 

<br>

#### List of Listening Events

##### `"onTextReceived"` - Listen for string messages.

- Listener function prototype
  `function(msg)`
- Parameter: 
  - msg : `String`  
    Received string message.

##### `"onBinaryReceived"` - Listen for binary messages.

- Listener function prototype 
  `function(data)`
- Parameter: 
  - data : `ByteBuffer`  
    Received binary message.

##### `"onError"` - Listen for errors.

- Listener function prototype 
  `function(msg)`
- Parameter: 
  - msg : `String`  
    Error message.

##### `"onLostConnection"` - Listen for lost connections.

- Listener function prototype 
  `function(code)`
- Parameter: 
  - code : `Integer`  
    Error code.

<br>

#### Close the Connection

`wsc.close()`

- Return value: Whether the connection was successfully closed.
- Return value type: `Boolean` 

Do not continue to use this client object while it is closed!

<br>

#### Force Disconnect

`wsc.shutdown()`

- Return value: Whether the connection was successfully disconnected.
- Return value type: `Boolean` 

Do not continue to use this client object while it is disconnected!

<br>

#### Get Error Code

`wsc.errorCode()`

- Return value: The error code generated by the last error.
- Return value type: `Integer`

If you encounter a failure in the use of the above interface, you can get the last error code from here.
