# Установка и использование

## 💻 Установка

### Для Windows

1. Загрузите последнюю <code>LiteLoader-<i>version</i>.zip</code> из [Релиза](https://github.com/LiteLDev/LiteLoader/releases) или [Действий](https://github.com/LiteLDev/LiteLoader/actions), 
2. Распакуйте содержимое в каталог с `bedrock_server.exe`. В случае чего, выберите `Заменить`.
3. Убедитесь что файл `bedrock_server.pdb` присутствует.  
   Запустите `LLPeEditor.exe` что-бы сгенерировать модифицированный BDS (`bedrock_server_mod.exe`).
4. Когда увидите `Press any key to continue. . . ` , нажмите любую кнопку что бы закрыть окно.
5. Запустите `bedrock_server_mod.exe` и радуйтесь жизни!

### Для Linux

#### Загрузочный скрипт(Ubuntu)

```
wget https://github.com/LiteLDev/LiteLoaderBDS/raw/beta/Scripts/install.sh
chmod +x install.sh
./install.sh
```

#### Docker

Введите следущие строки в терминал: 
```
docker pull shrbox/liteloaderbds
docker create --name liteloader -p 19132:19132/udp -i -t shrbox/liteloaderbds
```
Запустить сервер: `docker container start liteloader`  
Остановить сервер: `docker container stop -t 30 liteloader`  
Открыть консоль: `docker attach liteloader`  
Закрыть консоль: Нажмите `Ctrl + P + Q`. Если нажать `Ctrl + C`, сервер выключиться.  
Если вам нужно получить доступ к файлами, введите `docker volume --help` для подробностей.
<br>
Серверные файлы лежат по пути `/var/lib/docker/volume/volume_name/data/` (замените volume_name на ваше название).

Все готово! Теперь вы можете установить плагины на **LiteLoader**!

<br>

## 🎯 Find & Install plugins

### Plugin downloads

`LiteLoader` main plugin distribution channels.

- [Official Forum](https://forum.litebds.com/)
- [MineBBS](https://www.minebbs.com/resources/?prefix_id=59)

### Plugin installation

1. If you downloaded a zip file, unzip it
2. Place all the obtained contents directly into the `plugins` directory
3. Run `bedrock_server_mod.exe` to start the service

For more **installation and usage guides**,  come to 👉[LiteLoader documentation](https://docs.litebds.com/#/en/Usage/)👈 to view

## Installation ResourcePacks/Addon
Copy `.mcpack`, `.mcaddon` or `.zip` to `plugins/AddonsHelper` and restart server  
You can manage ResourcePacks and Addons by using `addons` command

## 🔌 Plugins hot management

Don't need to close server, you can manage plugins, we provided these console commands:

- `ll list`  
  **List** plugins
- `ll load ./plugins/xxxx.js`  
  **Hot load** plugin which locate in target path. The path is relative to the BDS root directory.
- `ll unload xxxx.lua`  
  **Hot unload** plugin which called xxxx.lua
- `ll reload xxxx.dll`  
  **Reload** plugin which called xxxx.dll
- `ll reload`  
  **Reload** all plugins
- `ll version`  
  Print version of LiteLoaderBDS
- `ll upgrade`  
  Check for updates

#### Hot management common problem

- After a plugin is hot unloaded, the commands registered by this plugin are not removed. When the player uses those commands, it will prompt that the command does not exist
- If your plugin has exported functions imported by other plugins, when you unload/reload this plugin, the corresponding Import of other plugins will be invalid.  
- Do not unload or reload plugins when the server has not started, or when there are a lot of players on the server! Otherwise the server may crash
- After hot reloading/hot reloading a plugin, the `onServerStarted` event registered by the plugin will be called immediately, and the player join event `onPlayerJoin` will be called one by one (because the server has been started at this time)

>[!WARNING]
>
> Plugin hot management is only used when debugging plugins. Avoid using **in production environments**

## 📡 ScriptEngine real time debug mode

- `jsdebug`  
  Enter JS real time debug mode
- `luadebug`  
  Enter Lua real time debug mode

In real-time debugging mode, the standard input will be executed as scripting language, and the results will be output in real time.  
If an error occurs, the engine will output an error message and a stack trace.  
Entering the `jsdebug` or `luadebug` again will exit the real time debugg mode.