# HA-NR-Flows
Sharing of smaller Home Assistant and Node-Red Flow used in my home automation applications. ![visitors](https://visitor-badge.glitch.me/badge?page_id=anas-ivs.ha-nr-flows.visitor-badge)

| [Pre-requisites](#Pre) |

Applications | [Telegram](#Telegram) | [Frigate](#Frigate) | [Autogate](#Autogate) | [Blueiris](#Blueiris) | [Synology Telegram File Downloader](#Synology) | 

Utilities | [Washing Machine Automata](#washing-machine) | [Dryer Automata](#dryer) | [Coffee Cup Counter](#coffee-cups) |  [Power Monitoring](#Power_Monitoring}) | [Application of Counter](#Application_counter}) | [Data logging](#data_logging}) |

Ad-Deen | [Random Hadith](#Random_Hadith)| [Pre-solat Broadcast](#pre-solat-broadcast)

## <a name="Pre"> Pre-requisites </a>
1.  Home Assistant with Node-Red. Ada banyak tutorial/videos on this with difficulty level as easy. [This is one example](http://https://www.juanmtech.com/get-started-with-node-red-and-home-assistant/). Test that you have enabled and can load Node-red on side bar. Make sure to also install [Node-Red companion integration](https://github.com/zachowj/hass-node-red).
2.  Telegram bot and chat ids. I followed this [tutorial](https://www.thesmarthomebook.com/2020/10/13/a-guide-to-using-telegram-with-node-red-and-home-assistant/) which is clear and easy to follow. 
> **Tip:** Follow the steps to get botid/chatid only but you do not need to setup in Home Assistant Notify/Telegram platform. Use Node-Red fully for Telegram.
3.  In Node-red the following additional nodes may be required:
- `node-red-contrib-home-assistant-websocket` - Comes pre-installed if using default HA Node-Red Docker from Supervisor store. 
- `node-red-contrib-telegrambot` - For Telegrambot. Setup as guide above.


## <a name="Telegram">Telegram in/out flow </a>

![Telegram in out flow](https://github.com/anas-ivs/HA-NR-Flows/blob/main/images/telegram-node-out.PNG) 

1. Capitalize on the `link-in` `link-out` nodes to create links to centralize Telegram sender. Aditionally configure one each telegram `link-in` to intended parse mode type i.e. message, picture, html where this is set respectively in the function block for chatID and parse mode.

2. Create two channel groups in Telegram - one being set on mute; and another obviously not hence allowing you to decide which flows you want to be alerted with sounds (P1) and for info/loggin only (P2). 



```json
[{"id":"b340e297.11a6","type":"function","z":"c8694ea1.9678f","name":"Creating message","func":"msg.payload = {\n chatId: '##P1 CHATID HERE##',\n type: 'message',\n content: msg.payload\n}\nmsg.payload.options = {parse_mode : \"Markdown\"};\n\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":635,"y":100,"wires":[["5ae73777.c96a18"]],"l":false},{"id":"8718a1dd.5beb7","type":"link in","z":"c8694ea1.9678f","name":"TelegramP1/message","links":["1e728149.1a3d9f","6207f8c8.4c3138","c1ecc3b6.5f7a","cdc3643e.25f498","cae18fa1.d1e76","4002d28e.02073c","2902516.c1262ae","cb4f929e.988ea","8a1bef97.bc983","944d419c.39e28","78ad1c9.b685de4","ebfb39ee.32cb58","8116579c.8f76d8"],"x":340,"y":100,"wires":[["b340e297.11a6"]],"icon":"node-red-contrib-telegrambot/telegram_cmd.png","l":true},{"id":"376edfb2.2e27b","type":"function","z":"c8694ea1.9678f","name":"Creating message","func":"msg.payload = {\n chatId: '##P2 CHATID HERE##',   // P2\n type: 'message',\n content: msg.payload\n }\nmsg.payload.options = {parse_mode : \"Markdown\"};\nreturn msg;\n\n\n","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":635,"y":260,"wires":[["87b3d4a9.9bec48"]],"l":false},{"id":"3bd8aa47.d839e6","type":"link in","z":"c8694ea1.9678f","name":"TelegramP2/message","links":["b9034afa.651308","29b724d3.00034c","b21413c.c2e05f","fdff9332.a0fb2","49e62860.2e77f8","5149d08.6aaf73","8846f4e0.f224a8","41a25d68.46b2a4","6753298a.6701d8","8e895913.617178","328fd291.4fd61e","539fee51.06081","40ade167.497ed","ace026da.9734a8","308c79be.40a506","9d883125.365dc","b308e31.dee102","c8223c0b.8a11f"],"x":320,"y":260,"wires":[["376edfb2.2e27b"]],"icon":"node-red-contrib-telegrambot/telegram_cmd.png","l":true},{"id":"17c45329.bd83cd","type":"function","z":"c8694ea1.9678f","name":"","func":"\nvar picture = {\n  content: msg.payload, // <-- check msg.payload is a buffer\n  caption: msg.message,\n  type : 'photo',\n  chatId: '##P2 CHATID HERE##'   // P2\n}\nmsg.payload = picture;\nreturn msg;\n\n","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":635,"y":300,"wires":[["87b3d4a9.9bec48"]],"l":false},{"id":"f41e4773.788428","type":"link in","z":"c8694ea1.9678f","name":"TelegramP2/picture","links":["569aae54.cb52c","b54e633a.8fe92","be8dcb41.88ff98","a2cbfe4d.94d2c","d1042c24.f3d98","8f0a1e69.a7512","80f2430f.673c3","6caf3bef.b35854"],"x":330,"y":300,"wires":[["17c45329.bd83cd"]],"icon":"node-red-contrib-telegrambot/telegram_cmd.png","l":true},{"id":"951a8dba.d4051","type":"function","z":"c8694ea1.9678f","name":"","func":"\nvar picture = {\n  content: msg.payload, // <-- check msg.payload is a buffer\n  caption: msg.message,\n  type : 'photo',\n  chatId: '##P1 CHATID HERE##' // P1\n}\nmsg.payload = picture;\nreturn msg;\n\n","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":635,"y":180,"wires":[["5ae73777.c96a18"]],"l":false},{"id":"d5fed255.7f8a4","type":"link in","z":"c8694ea1.9678f","name":"TelegramP1/picture","links":["e73e172.4e0b9e8","5476ad7e.f74af4","73d4b7ce.32f438"],"x":350,"y":180,"wires":[["951a8dba.d4051"]],"icon":"node-red-contrib-telegrambot/telegram_cmd.png","l":true},{"id":"e6e1d8da.45ee98","type":"function","z":"c8694ea1.9678f","name":"","func":"msg.payload = {\n chatId: '##P1 CHATID HERE##', // P1\n type: 'message',\n content: msg.payload\n }\nmsg.payload.options = {parse_mode : \"HTML\"};\nreturn msg;\n\n\n","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":635,"y":140,"wires":[["5ae73777.c96a18"]],"l":false},{"id":"c6e0b135.77de1","type":"link in","z":"c8694ea1.9678f","name":"TelegramP1/html","links":["425ca14d.eb33c","6a8413a1.0fa4cc"],"x":360,"y":140,"wires":[["e6e1d8da.45ee98"]],"icon":"node-red-contrib-telegrambot/telegram_cmd.png","l":true},{"id":"5ae73777.c96a18","type":"telegram sender","z":"c8694ea1.9678f","name":"P1 Channel","bot":"","haserroroutput":false,"outputs":1,"x":790,"y":140,"wires":[[]]},{"id":"87b3d4a9.9bec48","type":"telegram sender","z":"c8694ea1.9678f","name":"P2 Channel","bot":"","haserroroutput":false,"outputs":1,"x":790,"y":280,"wires":[[]]}]
```


## <a name="Autogate">Autogate </a>

## <a name="Frigate">Frigate flow </a>

## <a name="Blueiris">BlueIris flow</a>

## <a name="Synology">Synology Telegram file downloader </a>

## <a name="washing-machine">Washing Machine Automata</a>

## <a name="dryer">Dryer Automata</a>

## <a name="coffee-cups">Coffee Cup Counter</a>

## <a name="Power_Monitoring">Power Monitoring </a>

## <a name="Application_counter">Application of counter </a>

## <a name="Data_logging">Data Logging </a>

## <a name="Random_Hadith">Random Hadith </a>

## <a name="pre-solat-broadcast">Pre Solat Broadcast </a>
