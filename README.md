plugapi
=======

A generic API for creating Plug.dj bots

'''VERSION IS IN PRE-RELEASE BECAUSE THE API IS UNDER CHANGES - THINGS CAN BREAK'''

## How to use
Due to a Plug update, the original version of PlugAPI from npm no longer works. You will have to use this fork for now.

Run the following:

```npm install https://github.com/TATDK/plugapi/tarball/master```

To connect, do this!

```
var PlugAPI = require('plugapi'),
    AUTH = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx=?_expires=xxxxxxxxxxxxxxxxxx==&user_id=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=', // Put your auth token here, it's the cookie value for usr
    ROOM = '',
    UPDATECODE = '';

var bot = new PlugAPI(AUTH, UPDATECODE);
bot.connect(ROOM);

bot.on('roomJoin', function(room) {
    console.log("Joined " + room);
});
```

##Examples
Here are some bots using this API.

* Upcoming LiteBot in Radiant Music

Have a bot that uses the API? Let me know!

## EventListener
You can listen on essentially any event that plug emits.
```
// basic chat handler to show incoming chats formatted nicely
bot.on('chat', function(data) {
    if (data.type == 'emote')
        console.log(data.from + data.message);
    else
        console.log(data.from + "> " + data.message);
});
```

Here's an example for automatic reconnecting on errors / close events!
```
var reconnect = function() { bot.connect(ROOM); };

bot.on('close', reconnect);
bot.on('error', reconnect);
```

## Events

Read about some of the events on the [wiki](https://github.com/TATDK/plugapi/wiki/events).

## Actions

Read about the actions on the [wiki](https://github.com/TATDK/plugapi/wiki/actions).

##Misc

#### setLogObject(logger)
You can set your own custom logger for the API to use when it logs important events, such as errors or stack traces from the server.

The logger object must have a function called "log" that takes any number of parameters and prints them.

```
var prompt = new Prompt();
bot.setLogObject(prompt);
```

#### Multi line chat
Since Plug.dj cuts off chat messages at 250 characters, you can choose to have your bot split up chat messages into multiple lines:

```
var bot = new PlugAPI(auth);
bot.multiLine = true; // Set to true to enable multi line chat. Default is false
bot.multiLineLimit = 5; // Set to the maximum number of lines the bot should split messages up into. Any text beyond this number will just be omitted. Default is 5.
```

#### TCP Server
You can start up a TCP server the bot will listen to, for remote administration

Example:
```
    bot.tcpListen(6666, 'localhost');
    bot.on('tcpConnect', function(socket) {
        // executed when someone telnets into localhost port 6666
    });

    bot.on('tcpMessage', function(socket, msg) {
        // Use socket.write, for example, to send output back to the telnet session
        // 'msg' is whatever was entered by the user in the telnet session
    });
```