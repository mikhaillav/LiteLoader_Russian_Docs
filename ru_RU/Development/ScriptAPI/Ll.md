<!-- working -->
<!-- by shishkevichd -->

## 💡 API загрузки плагинов

Некоторые интерфейсы, связанные с операциями LiteLoaderBDS, представлены здесь.

### Получить версию LiteLoaderBDS

`ll.version()`

- Возращаемое значение: объект версии LiteLoaderBDS (`Object`) 

- Тип возращаемого значения:  `Object<Integer,Integer,Integer,Boolean>`

- Для возвращаемого объекта версии LiteLoaderBDS `Ver` есть следующие значения:

  | Значение       | Обозначение                              | Тип  |
  | ------------ | ------------------------------------- | --------- |
  | ver.major    | Мажорное число версии (пример **2** в **2**.4.1)   | `Integer` |
  | ver.minor    | Минорное число версии (пример **4** in 2.**4**.1)    | `Integer` |
  | ver.revision | Патчевое число версии (пример **1** in 2.4.**1**)  | `Integer` |
  | ver.isBeta   | Является ли эта версия бетой  | `Boolean` |

<br>

### Получить версию LiteLoaderBDS (Строка)

`ll.versionString()`

- Возращаемое значение: версия LiteLoaderBDS
- Тип возращаемого значения:  `String`

<br>

### Проверить версию LiteLoaderBDS

`ll.requireVersion(major[,minor,revision])`

- Параметры: 
  - major: `Integer`  
    Проверка, является ли мажорное число версии установленного в настоящее время LL >= указанного значения.
  - minor: `Integer` (на свое усмотрение)
    Проверка, является ли минорное число версии установленного в настоящее время LL >= указанного значения.
  - revision: `Integer` (на свое усмотрение)  
    Проверка, является ли патчевое число версии установленного в настоящее время LL >= указанного значения.
- Возращаемое значение: Результат проверки
- Тип возращаемого значения:  `Boolean`

Если обнаружение обнаружит, что установленная в настоящее время версия LLSE ниже, чем передаваемое значение, оно вернется `false`. 
Вы можете сообщить об ошибке, чтобы напомнить пользователям обновить версию LiteLoaderBDS (LLSE).
<br>

### Список загруженных плагинов

`ll.listPlugins()`

- Возращаемое значение: Список имен загруженных плагинов
- Тип возращаемого значения:  `Array<String,String,...>`

<br>

### Вызов удаленной функции

Чтобы предоставить разработчикам функции, разработанные другими разработчиками, для других плагинов представлена функция вызова удаленной функции, чтобы один плагин, написанный на LLSE, мог вызвать существующие функции в другом плагине.

#### Функция экспорта 

Во-первых, для того, чтобы функции в вашем плагине были используемыми для других плагином, вы сначала экспортируете некоторые функции в своем плагине, чтобы другие могли найти ваш интерфейс по имени. Используйте эту функцию, чтобы экспортировать функции, с которыми вы хотите поделиться:

`ll.export(func,name)`

- Параметры: 
  - func : `Function`  
    Экспортируемая функция
  - name : `String`  
    Название функции при экспорте
- Возращаемое значение: Был ли экспорт успешен.
- Тип возращаемого значения:  `Boolean`

Примечание. Если при экспорте есть конфликт имени, экспорт будет неудачным. Вам может потребоваться добавить уникальный префикс или суффикс к имени экспорта, чтобы избежать возможных конфликтов с другими плагинами.

<br>

#### Функция экспорта

Чтобы предоставить разработчикам функции, разработанные другими разработчиками, для других плагинов представлена функция вызова удаленной функции, чтобы один плагин, написанный на `LLSE` или `LL`, мог вызвать существующие функции в другом плагине.

`ll.export(func,namespace,name)`

- Параметры: 
  - func : `Function`  
    Экспортируемая функция
  - namespace : `String`  
    Имя пространства имен функции, которое удобно для различения API, экспортируемого различными плагинами.
  - name : `String`  
    Название функции при экспорте
- Возращаемое значение: Был ли экспорт успешен.
- Тип возращаемого значения:  `Boolean`

Примечание. Если вы экспортируете функцию с существующим пространством имен и именем, экспорт будет неудачным. Это API в настоящее время доступен только в режиме отладки (`debugMode`).  

<br>

#### Функция импорта

After you have learned that there is a plug-in exporting function, in order to use the function exported by him, you first need to import this function into your own scripting system.
LLSE provides the interface import to import functions already exported by other plugins. 

`ll.import(name)`

- Параметры: 
  - name : `String`  
    The export name used by the function to be imported.
- Возращаемое значение: the imported function
- Тип возращаемого значения:  `Function`

`ll.import` will import the target function directly into your scripting environment. Therefore, you can call an imported function as if you were using an existing function. The process of calling across plugins will be done automatically in the background, you don't need to worry about any of this.

Note: In the process of remote invocation, you cannot pass custom data objects such as player objects in the parameters. You can use player Xuid info etc as an alternative.

<br>

#### Import Function

After you have learned that there is a plug-in exporting function, in order to use the function exported by him, you first need to import this function into your own scripting system.
LLSE provides the interface import to import functions already exported by other plugins.

`ll.import(namespace,name)`

- Параметры: 
  - namespace : `String`  
    The namespace name used by the function that is being imported.
  - name : `String`  
    The name of the function that is being imported.
- Возращаемое значение: The imported function
- Тип возращаемого значения:  `Function`

`ll.import` will import the target function directly into your scripting environment. Therefore, you can call an imported function as if you were using an existing function. The process of calling across plugins will be done automatically in the background, you don't need to worry about any of this.

Note: In the process of remote invocation, you cannot pass custom data objects such as player objects in the parameters. You can use player Xuid info etc as an alternative Note: this API is only available in `debugMode`.

<br>

#### Example of Remote Calling Function 

For example, there is a plugin that exports a function, and the export name of the function is AAA_Welcome
when you use `welcome = ll.import("AAA_Welcome"); ` After the import is complete, you can execute directly below:

`welcome("hello",2,true);`   

As if the function already existed. 
The parameters of the function will be automatically forwarded to the corresponding target function for execution, and the return value of the corresponding target function will be returned after execution. The whole process is automatically completed. 

Notice! When calling a function, you need to ensure that the number and types of parameters you pass in and the parameters accepted by the target function are correct and in one-to-one correspondence. Otherwise, an error will occur. 

<br>

### Set Plugin Dependencies 

Sometimes, you need to make sure that certain plugins are loaded before your own plugins to use the frontend services provided by them. We call these frontend plugins **library dependencies**.

When using the import function mentioned above, you need to pay attention: the premise of being able to import a function is that the pre-plugin to be called has been loaded by LLSE.
Therefore, you may need to use the following function to set the dependent library, so that the pre-plugins you need are loaded first and the import is successful.

LLSE provides the following interface to preload the dependent libraries required by the plugin, download the dependent library files you need from local files, or even from remote HTTP(s) addresses.

`ll.require(path[,remotePath])`

- Параметры: 
  - path : `String`  
    Library dependency filename (Example: `addplugin.js`)
  - remotePath : `String`  
    (Optional parameter) The path to find and load dependent libraries, see below for instructions.
- Возращаемое значение: Whether the dependent library is loaded successfully 
- Тип возращаемого значения:  `Boolean`

For execution, use `ll.require`, then LLSE will perform the following series of operations:

- Search the list of loaded plugins. If the dependent library has been loaded, it will return success directly.
- Search **plugins** and **plugins/lib** directories, if the corresponding dependent library file is found, load it and return the loading result.
- If the corresponding dependent library file is not found after the search is completed, and the remotePath parameter is not passed in, it will return directly to failure.
- Use the HTTP(s) protocol remotePath to request the download address corresponding to the remotePath parameter, and download the dependent library files to the `plugins/lib` directory. If the download fails, return failure.
- Load the successfully downloaded dependent library file and return the loading result.

<br>

Authors of dependent libraries can host relevant code on stable large websites such as GitHub or Gitee, and provide external links to other developers for remote download.

<br>

### Execute a String as a Script

`ll.eval(str)`

- Параметры: 
  - str : `String`  
    String to execute as a Script
- Возращаемое значение: Execution result
- Тип возращаемого значения:  `Any Type`

Different from the above mentioned `ll.require`, the script code executed here is directly executed in the engine corresponding to the current plugin, similar to the eval mechanism of each language.