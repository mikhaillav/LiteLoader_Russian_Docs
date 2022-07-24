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

## 🔌 WebSocket Client Object API

In LLSE, use "WebSocket objects" to manipulate the connection and work of a WebSocket client.

### Create a New WebSocket Client Object 

[JS] `new WSClient()`  
[Lua] `WSClient()`

- Return value: A new WebSocket client object.
- Return value type: `WSClient`

<br>

### WebSocket Client Object - Properties

Every WS client object contains some fixed object properties. for a specific file object `wsc`, has the following properties:

| Attributes | Meaning        | Data Type   |
| ---------- | -------------- | ------ |
| wsc.status | Current Connection Status | `Enum` |

These object properties are read-only and cannot be modified.

Among them, the wsc.status enumeration has the following situations:

`wsc.Open` - In normal connection.  
`wsc.Closing` - Disconnecting.  
`wsc.Closed` - Not connected.

<br>

### WebSocket Client Object - Function

Every WS client object contains some member functions (member methods) that can be executed. for a specific file object `wsc`, you can perform some operations on this client through the following functions.

#### Create a Connection

`wsc.connect(target)`

- Parameter: 
  - target : `String`  
    The destination address to connect to, in the form of `ws://hostname[:port][/path/path][?query=value]`
- Return value: Whether the connection is successful.
- Return value type: `Boolean` 

<br>

#### Create a Connection Asynchronously

`wsc.connectAsync(target,callback)`

- Parameters: 
  - target : `String`  
    The destination address to connect to, in the form of `ws://hostname[:port][/path/path][?query=value]`
  - callback : `Function`
    A callback function to execute when the connection succeeds or fails.
- Return value: Whether the connection attempt was started successfully or not.
- Return value type: `Boolean` 

Note: The prototype of the callback function of the parameter callback: `function(success)`  

- success : `Boolean`    
  Whether the WebSocket connection is successful 

<br>

#### Send Text/Binary Messages

`wsc.send(msg)`

- Parameter: 
  - msg : `String` / `ByteBuffer`  
    Text/Binary data to send.
- Return value: Whether it was sent successfully.
- Return value type: `Boolean` 

If the parameter type passed in is `String`, will be sent as text, if it is `ByteBuffer` will be sent as binary data.

<br>

#### Listen for WebSocket Events 

In the process of WS working, when a message is received or an error occurs, the relevant information needs to be processed. Here is the interface for listening to events.

`wsc.listen(event,callback)`

- Parameters: 

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
