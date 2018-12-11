# node-red-contrib-alexa-remote2

This is a collection of node-red nodes for interacting with alexa.
All functionality is from the great [alexa-remote2](https://www.npmjs.com/package/alexa-remote2).
The goal is to expose all of [alexa-remote2](https://www.npmjs.com/package/alexa-remote2)s functionality in node-red nodes.

[Changelog](CHANGELOG.md)

### Logging in with Email and Password
   - **works with node version 10 but not with node version 8!**
   - will not work if Captcha or 2 Factor Authentication is needed

### Setup
1. Drag a **alexa remote sequence** node into your flow.
2. Create a new Account by pressing the edit button at the right side of the *Account* field.
3. Enter the **Cookie** or the **Email** and **Password** of your Amazon Alexa Account.
   - [How do i get my cookie?](get_cookie.md)
   - [Logging in with Email and Password](#logging-in-with-email-and-password)
4. Choose a **Service Host** and **Page** depending on your location. For example:

   ||Service Host|Page
   |---|---|---
   |USA|pitangui.amazon.com|amazon.com
   |UK|alexa.amazon.co.uk|amazon.co.uk
   |GER|layla.amazon.de|amazon.de
   
5. *Add* the new Account.
6. Enter the **Device** name (or Serial Number) of the target Alexa Device that is connected to your account.

Now trigger the Alexa Sequence Node with any message and your Alexa will say "Hello World!". (Hopefully!)

### Advanced
- **alexa remote sequence**: you can set the sequence by message too, see node info
- you can override the account for each node by defining `msg.alexaRemoteAccount` with the required properties: `cookie` or `email` and `password`, `alexaServiceHost`, `amazonPage`, and optional properties: `bluetooth`*(true/false)*, `useWsMqtt`*(=events true/false)*, `acceptLanguage` and `userAgent`.

### Examples
To make these examples work you need to select your Account for each of the alexa remote nodes.  
All following examples depend on node-red-dashboard.

#### 1. Alexa Speak Simple Dashboard
Enter a phrase for Alexa to say on a chosen device.
```
[{"id":"495c1858.223988","type":"tab","label":"Alexa Speak Basic","disabled":false,"info":""},{"id":"9aaa747f.77dd58","type":"ui_text_input","z":"495c1858.223988","name":"","label":"","group":"f5f6c00c.16c28","order":3,"width":0,"height":0,"passthru":true,"mode":"text","delay":"1","topic":"","x":140,"y":220,"wires":[["e02c877b.86a0d8"]]},{"id":"e02c877b.86a0d8","type":"change","z":"495c1858.223988","name":"","rules":[{"t":"set","p":"speakText","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":350,"y":220,"wires":[[]]},{"id":"61b3b44b.f9fe7c","type":"ui_button","z":"495c1858.223988","name":"","group":"f5f6c00c.16c28","order":4,"width":0,"height":0,"passthru":false,"label":"Speak","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":130,"y":260,"wires":[["a9588a39.ada738"]]},{"id":"a9588a39.ada738","type":"alexa-remote-sequence","z":"495c1858.223988","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","sequenceInputs":[{"command":"speak","value_type":"flow","value_value":"speakText"}],"x":360,"y":260,"wires":[[]]},{"id":"b47979d7.fa6d98","type":"change","z":"495c1858.223988","name":"","rules":[{"t":"set","p":"device","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":500,"y":160,"wires":[[]]},{"id":"393eea6d.9c5506","type":"alexa-remote-get","z":"495c1858.223988","name":"","account":"fb0d8688.41ff08","target_type":"select","target_value":"devices","serialOrName_type":"str","serialOrName_value":"","options":{"devices":{},"cards":{"limit":{"type":"num","value":"10"},"beforeCreationTime":{"type":"str","value":"%t"}},"media":{},"playerInfo":{},"list":{"size":{"type":"num","value":"100"},"startTime":{"type":"str","value":""},"endTime":{"type":"str","value":""},"completed":{"type":"bool","value":"false"},"listType":{"type":"select","value":"TASK"}},"wakewords":{},"notifications":{"cached":{"type":"bool","value":"true"}},"doNotDisturb":{},"deviceNotificationState":{},"bluetooth":{"cached":{"type":"bool","value":"true"}},"activities":{"startTime":{"type":"str","value":""},"size":{"type":"num","value":"1"},"offset":{"type":"num","value":"1"}},"account":{},"conversations":{"latest":{"type":"bool","value":"true"},"includeHomegroup":{"type":"bool","value":"true"},"unread":{"type":"bool","value":"false"},"modifiedSinceDate":{"type":"str","value":"1970-01-01T00:00:00.000Z"},"includeUserName":{"type":"bool","value":"true"}},"automationRoutines":{"limit":{"type":"num","value":"2000"}},"musicProviders":{},"homeGroup":{},"devicePreferences":{},"smarthomeDevices":{},"smarthomeGroups":{},"smarthomeEntities":{},"smarthomeBehaviourActionDefinitions":{}},"x":350,"y":100,"wires":[["c1c89249.16e6f"]]},{"id":"1bdbe539.4ea10b","type":"inject","z":"495c1858.223988","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":true,"onceDelay":"0","x":150,"y":60,"wires":[["393eea6d.9c5506"]]},{"id":"c1c89249.16e6f","type":"change","z":"495c1858.223988","name":"","rules":[{"t":"set","p":"options","pt":"msg","to":"payload.devices.accountName[]","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":160,"y":160,"wires":[["51dc169b.0cc3e8"]]},{"id":"51dc169b.0cc3e8","type":"ui_dropdown","z":"495c1858.223988","name":"","label":"Device","place":"","group":"f5f6c00c.16c28","order":1,"width":0,"height":0,"passthru":true,"options":[{"label":"","value":"","type":"str"}],"payload":"","topic":"","x":330,"y":160,"wires":[["b47979d7.fa6d98"]]},{"id":"229465fa.2a93ca","type":"ui_button","z":"495c1858.223988","name":"","group":"f5f6c00c.16c28","order":2,"width":0,"height":0,"passthru":false,"label":"Refresh","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":140,"y":100,"wires":[["393eea6d.9c5506"]]},{"id":"f5f6c00c.16c28","type":"ui_group","z":"","name":"Alexa Speak","tab":"e44a6ba2.3e2b08","order":1,"disp":true,"width":"6","collapse":false},{"id":"e44a6ba2.3e2b08","type":"ui_tab","z":"","name":"Alexa Speak","icon":"speaker","order":2}]
```

#### 2. Alexa Speak List Dashboard:
In this example you can save phrases to be said (to a file).
And send them by clicking a button. You need to press the inject node under Create File before using it.
```
[{"id":"90d91043.6c66","type":"tab","label":"Alexa Speak","disabled":false,"info":""},{"id":"8aa37acc.b3ea58","type":"ui_text_input","z":"90d91043.6c66","name":"","label":"","group":"bcfecefe.1c642","order":1,"width":0,"height":0,"passthru":true,"mode":"text","delay":"1","topic":"","x":360,"y":500,"wires":[["1d5e5135.032e6f"]]},{"id":"1d5e5135.032e6f","type":"change","z":"90d91043.6c66","name":"","rules":[{"t":"set","p":"speakText","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":530,"y":500,"wires":[[]]},{"id":"39f3cd05.bc2572","type":"ui_button","z":"90d91043.6c66","name":"","group":"bcfecefe.1c642","order":3,"width":"3","height":"1","passthru":false,"label":"Test","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":370,"y":540,"wires":[["2b55050a.62da0a"]]},{"id":"5eace07f.382d2","type":"ui_template","z":"90d91043.6c66","group":"ef8fa9de.e65278","name":"","order":4,"width":"6","height":"10","format":"<div ng-repeat=\"text in msg.payload track by $index\" layout=\"row\">\n    <md-button style=\"background:transparent\"><ng-md-icon ng-click=\"send({indexToDelete:$index})\" style=\"stroke:#FFF;\" icon=\"delete\"></ng-md-icon></md-button>\n    <md-button flex style=\"padding:200px; margin: 5px;\" class=\"nr-dashboard-button\" ng-click=\"send({payload:text})\">{{text}}</md-button>\n</div>","storeOutMessages":false,"fwdInMessages":false,"templateScope":"local","x":400,"y":620,"wires":[["6951223c.dfa9bc"]]},{"id":"aa5edcff.8f334","type":"inject","z":"90d91043.6c66","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":true,"onceDelay":0.1,"x":110,"y":300,"wires":[["cbad945b.d1e2f8"]]},{"id":"6951223c.dfa9bc","type":"switch","z":"90d91043.6c66","name":"","property":"indexToDelete","propertyType":"msg","rules":[{"t":"istype","v":"undefined","vt":"undefined"},{"t":"istype","v":"number","vt":"number"}],"checkall":"true","repair":false,"outputs":2,"x":530,"y":620,"wires":[["f0aac8f7.d22188","375accd5.8c8a64"],["f20b78b0.cfab78","8be37ece.78e06"]]},{"id":"f20b78b0.cfab78","type":"debug","z":"90d91043.6c66","name":"","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"indexToDelete","x":750,"y":640,"wires":[]},{"id":"375accd5.8c8a64","type":"alexa-remote-sequence","z":"90d91043.6c66","name":"","account":"2c18b673.cbdbda","serialOrName_type":"flow","serialOrName_value":"device","sequenceInputs":[{"command":"speak","value_type":"msg","value_value":"payload"}],"x":740,"y":540,"wires":[["35e7094a.5b1116"]]},{"id":"64df71ce.c7fe1","type":"ui_button","z":"90d91043.6c66","name":"","group":"bcfecefe.1c642","order":2,"width":"3","height":"1","passthru":false,"label":"Add","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":90,"y":500,"wires":[["76d20ff2.b7c01"]]},{"id":"8be37ece.78e06","type":"file in","z":"90d91043.6c66","name":"","filename":"alexa-remote-speak.json","format":"utf8","chunk":false,"sendError":false,"x":150,"y":780,"wires":[["bf35e6b8.403908"]]},{"id":"bf35e6b8.403908","type":"json","z":"90d91043.6c66","name":"","property":"payload","action":"","pretty":false,"x":90,"y":820,"wires":[["569f6dc0.348034"]]},{"id":"569f6dc0.348034","type":"function","z":"90d91043.6c66","name":"remove at indexToDelete","func":"if(!Array.isArray(msg.payload))\n    return node.error('payload is not array');\n\nif(!Number.isInteger(msg.indexToDelete))\n    return node.error('index is no integer');\n\nmsg.payload.splice(msg.indexToDelete, 1);\nreturn msg;","outputs":1,"noerr":0,"x":150,"y":860,"wires":[["7e90741c.dfad2c"]]},{"id":"cbad945b.d1e2f8","type":"file in","z":"90d91043.6c66","name":"","filename":"alexa-remote-speak.json","format":"utf8","chunk":false,"sendError":false,"x":150,"y":340,"wires":[["ada47cb2.fb7dc"]]},{"id":"ada47cb2.fb7dc","type":"json","z":"90d91043.6c66","name":"","property":"payload","action":"","pretty":false,"x":90,"y":380,"wires":[["5eace07f.382d2"]]},{"id":"194744b2.deab0b","type":"comment","z":"90d91043.6c66","name":"Removing Item","info":"","x":120,"y":740,"wires":[]},{"id":"7e90741c.dfad2c","type":"file","z":"90d91043.6c66","name":"","filename":"alexa-remote-speak.json","appendNewline":false,"createDir":false,"overwriteFile":"true","x":150,"y":900,"wires":[["5eace07f.382d2"]]},{"id":"f0aac8f7.d22188","type":"debug","z":"90d91043.6c66","name":"","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","x":730,"y":600,"wires":[]},{"id":"76d20ff2.b7c01","type":"file in","z":"90d91043.6c66","name":"","filename":"alexa-remote-speak.json","format":"utf8","chunk":false,"sendError":false,"x":150,"y":540,"wires":[["6e35ebcc.b68694"]]},{"id":"6e35ebcc.b68694","type":"json","z":"90d91043.6c66","name":"","property":"payload","action":"","pretty":false,"x":90,"y":580,"wires":[["4f791c91.cdeee4"]]},{"id":"4f791c91.cdeee4","type":"function","z":"90d91043.6c66","name":"push flow.speakText","func":"if(!Array.isArray(msg.payload))\n    return node.error('payload is not array');\n\nmsg.payload.push(flow.get('speakText'));\nreturn msg;","outputs":1,"noerr":0,"x":140,"y":620,"wires":[["e8a555f4.7415d8"]]},{"id":"e8a555f4.7415d8","type":"file","z":"90d91043.6c66","name":"","filename":"alexa-remote-speak.json","appendNewline":false,"createDir":false,"overwriteFile":"true","x":150,"y":660,"wires":[["5eace07f.382d2"]]},{"id":"b1a2722f.4247a","type":"comment","z":"90d91043.6c66","name":"Adding Item","info":"","x":110,"y":460,"wires":[]},{"id":"ebf528b3.ec1638","type":"comment","z":"90d91043.6c66","name":"Loading list","info":"","x":110,"y":260,"wires":[]},{"id":"624579cf.108458","type":"change","z":"90d91043.6c66","name":"","rules":[{"t":"set","p":"device","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":880,"y":40,"wires":[[]]},{"id":"903715e.0873ae8","type":"alexa-remote-get","z":"90d91043.6c66","name":"","account":"2c18b673.cbdbda","target_type":"str","target_value":"devices","serialOrName_type":"str","serialOrName_value":"","options":{"devices":{},"cards":{"limit":{"type":"num","value":"10"},"beforeCreationTime":{"type":"str","value":"%t"}},"media":{},"playerInfo":{},"list":{"size":{"type":"num","value":"100"},"startTime":{"type":"str","value":""},"endTime":{"type":"str","value":""},"completed":{"type":"bool","value":"false"},"listType":{"type":"select","value":"TASK"}},"wakewords":{},"notifications":{"cached":{"type":"bool","value":"true"}},"doNotDisturb":{},"deviceNotificationState":{},"bluetooth":{"cached":{"type":"bool","value":"true"}},"activities":{"startTime":{"type":"str","value":""},"size":{"type":"num","value":"1"},"offset":{"type":"num","value":"1"}},"account":{},"conversations":{"latest":{"type":"bool","value":"true"},"includeHomegroup":{"type":"bool","value":"true"},"unread":{"type":"bool","value":"false"},"modifiedSinceDate":{"type":"str","value":"1970-01-01T00:00:00.000Z"},"includeUserName":{"type":"bool","value":"true"}},"automationRoutines":{"limit":{"type":"num","value":"2000"}},"musicProviders":{},"homeGroup":{},"devicePreferences":{},"smarthomeDevices":{},"smarthomeGroups":{},"smarthomeEntities":{},"smarthomeBehaviourActionDefinitions":{}},"x":310,"y":40,"wires":[["e9c0e046.49ebf","35e7094a.5b1116"]]},{"id":"b916f31.a05161","type":"inject","z":"90d91043.6c66","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":true,"onceDelay":"0","x":110,"y":80,"wires":[["903715e.0873ae8"]]},{"id":"e9c0e046.49ebf","type":"change","z":"90d91043.6c66","name":"","rules":[{"t":"set","p":"options","pt":"msg","to":"payload.devices[deviceFamily='ECHO'].accountName[]","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":540,"y":40,"wires":[["93639aa2.af2d58"]]},{"id":"93639aa2.af2d58","type":"ui_dropdown","z":"90d91043.6c66","name":"","label":"Device","place":"","group":"b0381784.1dcb58","order":1,"width":"5","height":"1","passthru":true,"options":[{"label":"","value":"","type":"str"}],"payload":"","topic":"","x":710,"y":40,"wires":[["624579cf.108458"]]},{"id":"2b55050a.62da0a","type":"change","z":"90d91043.6c66","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"speakText","tot":"flow"}],"action":"","property":"","from":"","to":"","reg":false,"x":520,"y":540,"wires":[["375accd5.8c8a64"]]},{"id":"2a155c.d0213aa4","type":"ui_template","z":"90d91043.6c66","group":"fc58b56a.8fb978","name":"Status","order":0,"width":0,"height":0,"format":"<div style=\"margin:auto; margin-top:0;\" ng-bind-html=\"msg.payload\"></div>","storeOutMessages":true,"fwdInMessages":true,"templateScope":"local","x":1090,"y":260,"wires":[[]]},{"id":"35e7094a.5b1116","type":"switch","z":"90d91043.6c66","name":"","property":"error","propertyType":"msg","rules":[{"t":"istype","v":"object","vt":"object"},{"t":"istype","v":"undefined","vt":"undefined"}],"checkall":"true","repair":false,"outputs":2,"x":760,"y":260,"wires":[["df38294f.3c4f28"],["b2e2bfe8.fe5a6"]]},{"id":"df38294f.3c4f28","type":"change","z":"90d91043.6c66","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"error.message","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":920,"y":240,"wires":[["2a155c.d0213aa4"]]},{"id":"b2e2bfe8.fe5a6","type":"change","z":"90d91043.6c66","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"Success","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":920,"y":280,"wires":[["2a155c.d0213aa4"]]},{"id":"9a8ae965.d518d8","type":"ui_button","z":"90d91043.6c66","name":"Refresh Device","group":"b0381784.1dcb58","order":4,"width":"1","height":"1","passthru":false,"label":"","color":"","bgcolor":"","icon":"refresh","payload":"","payloadType":"str","topic":"","x":120,"y":40,"wires":[["903715e.0873ae8"]]},{"id":"cb3b79b6.a46af8","type":"inject","z":"90d91043.6c66","name":"","topic":"","payload":"[]","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":450,"y":300,"wires":[["eb0e2ba8.7f8168"]]},{"id":"eb0e2ba8.7f8168","type":"file","z":"90d91043.6c66","name":"","filename":"alexa-remote-speak.json","appendNewline":false,"createDir":false,"overwriteFile":"true","x":510,"y":340,"wires":[[]]},{"id":"4cd07c52.fde7e4","type":"comment","z":"90d91043.6c66","name":"Create File","info":"","x":460,"y":260,"wires":[]},{"id":"bcfecefe.1c642","type":"ui_group","z":"","name":"Add Phrase","tab":"ad97b13c.6b6b4","order":2,"disp":true,"width":"6","collapse":false},{"id":"ef8fa9de.e65278","type":"ui_group","z":"","name":"List","tab":"ad97b13c.6b6b4","order":3,"disp":true,"width":"6","collapse":false},{"id":"b0381784.1dcb58","type":"ui_group","z":"","name":"Choose Device","tab":"ad97b13c.6b6b4","order":1,"disp":true,"width":"6","collapse":false},{"id":"fc58b56a.8fb978","type":"ui_group","z":"","name":"Status","tab":"ad97b13c.6b6b4","order":4,"disp":true,"width":"6","collapse":false},{"id":"ad97b13c.6b6b4","type":"ui_tab","z":"","name":"Alexa Speak","icon":"speaker","order":1}]
```

#### 3. Alexa Music Dashboard:
Dashboard in which you can enter a query for music to be played from a chosen provider on a chosen device.
You can also pause, resume and change the volume, toggle repeat and toggle shuffle.
```
[{"id":"d88ff07e.1817a","type":"tab","label":"Alexa Music","disabled":false,"info":""},{"id":"85ce383c.682918","type":"ui_dropdown","z":"d88ff07e.1817a","name":"","label":"Provider","place":" ","group":"eda16006.f7366","order":3,"width":"5","height":"1","passthru":true,"options":[{"label":"","value":"","type":"str"}],"payload":"","topic":"","x":320,"y":320,"wires":[["af5a1f7f.cb1a7"]]},{"id":"f0015e69.1e2c","type":"alexa-remote-action","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","target_type":"select","target_value":"playMusicProvider","serialOrName_type":"flow","serialOrName_value":"device","options":{"checkAuthentication":{},"createNotification":{"type":{"type":"str","value":"Alarm"},"label":{"type":"str","value":""},"value":{"type":"str","value":"2019-01-01T00:00:00"},"status":{"type":"str","value":"ON"},"sound":{"type":"str","value":""}},"deleteNotification":{"id":{"type":"str","value":""}},"tuneinSearch":{"query":{"type":"str","value":""}},"findDevice":{},"renameDevice":{"newName":{"type":"str","value":""}},"deleteDevice":{},"executeAutomationRoutine":{"automationId":{"type":"str","value":""}},"playMusicProvider":{"providerId":{"type":"flow","value":"musicProvider"},"searchPhrase":{"type":"flow","value":"musicQuery"}},"sendTextMessage":{"conversationId":{"type":"str","value":""},"text":{"type":"str","value":""}},"deleteSmarthomeDevice":{"smarthomeDevice":{"type":"str","value":""}},"deleteSmarthomeGroup":{"smarthomeGroup":{"type":"str","value":""}},"deleteAllSmarthomeDevices":{},"discoverSmarthomeDevice":{},"querySmarthomeDevices":{"applicanceIds":{"type":"str","value":""},"entityType":{"type":"str","value":""}},"connectBluetooth":{"address":{"type":"str","value":""}},"unpaireBluetooth":{"address":{"type":"str","value":""}},"disconnectBluetooth":{"address":{"type":"str","value":""}}},"x":330,"y":480,"wires":[["6c8f611b.fd599"]]},{"id":"af5a1f7f.cb1a7","type":"change","z":"d88ff07e.1817a","name":"","rules":[{"t":"set","p":"musicProvider","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":360,"y":360,"wires":[[]]},{"id":"eb4d6274.292cb","type":"ui_button","z":"d88ff07e.1817a","name":"","group":"eda16006.f7366","order":6,"width":"0","height":"0","passthru":false,"label":"Play","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":110,"y":480,"wires":[["f0015e69.1e2c"]]},{"id":"ae0a80f1.6d86a","type":"ui_text_input","z":"d88ff07e.1817a","name":"","label":"Query","group":"eda16006.f7366","order":5,"width":0,"height":0,"passthru":true,"mode":"text","delay":300,"topic":"","x":110,"y":420,"wires":[["19948c4d.a11ff4"]]},{"id":"19948c4d.a11ff4","type":"change","z":"d88ff07e.1817a","name":"","rules":[{"t":"set","p":"musicQuery","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":320,"y":420,"wires":[[]]},{"id":"b8261ee9.c0496","type":"alexa-remote-command","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","command_type":"select","command_value":"pause","options":{"play":{},"pause":{},"next":{},"previous":{},"forward":{},"rewind":{},"volume":{"value":{"type":"num","value":"50"}},"shuffle":{"value":{"type":"bool","value":"true"}},"repeat":{"value":{"type":"bool","value":"true"}}},"x":350,"y":520,"wires":[["6c8f611b.fd599"]]},{"id":"15d80731.539739","type":"change","z":"d88ff07e.1817a","name":"","rules":[{"t":"set","p":"device","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":340,"y":160,"wires":[["16d267af.07a6f8"]]},{"id":"dbaf8f0a.6aecd","type":"ui_button","z":"d88ff07e.1817a","name":"","group":"247a1c57.1e0984","order":1,"width":"3","height":"1","passthru":false,"label":"Pause","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":110,"y":520,"wires":[["b8261ee9.c0496"]]},{"id":"725b6451.bf97cc","type":"alexa-remote-command","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","command_type":"select","command_value":"play","options":{"play":{},"pause":{},"next":{},"previous":{},"forward":{},"rewind":{},"volume":{"value":{"type":"num","value":"50"}},"shuffle":{"value":{"type":"bool","value":"true"}},"repeat":{"value":{"type":"bool","value":"true"}}},"x":350,"y":560,"wires":[["6c8f611b.fd599"]]},{"id":"3cc8c4be.a0111c","type":"ui_button","z":"d88ff07e.1817a","name":"","group":"247a1c57.1e0984","order":2,"width":"3","height":"1","passthru":false,"label":"Resume","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":120,"y":560,"wires":[["725b6451.bf97cc"]]},{"id":"6aa7e650.889c48","type":"ui_slider","z":"d88ff07e.1817a","name":"","label":"Volume","group":"247a1c57.1e0984","order":3,"width":0,"height":0,"passthru":true,"outs":"end","topic":"","min":0,"max":"100","step":1,"x":120,"y":600,"wires":[["16704a39.b4a426"]]},{"id":"16704a39.b4a426","type":"alexa-remote-command","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","command_type":"select","command_value":"volume","options":{"play":{},"pause":{},"next":{},"previous":{},"forward":{},"rewind":{},"volume":{"value":{"type":"msg","value":"payload"}},"shuffle":{"value":{"type":"bool","value":"true"}},"repeat":{"value":{"type":"bool","value":"true"}}},"x":350,"y":600,"wires":[["6c8f611b.fd599"]]},{"id":"9abd9be4.9e36f8","type":"alexa-remote-get","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","target_type":"select","target_value":"devices","serialOrName_type":"str","serialOrName_value":"","options":{"devices":{},"cards":{"limit":{"type":"num","value":"10"},"beforeCreationTime":{"type":"str","value":"%t"}},"media":{},"playerInfo":{},"list":{"size":{"type":"num","value":"100"},"startTime":{"type":"str","value":""},"endTime":{"type":"str","value":""},"completed":{"type":"bool","value":"false"},"listType":{"type":"select","value":"TASK"}},"wakewords":{},"notifications":{"cached":{"type":"bool","value":"true"}},"doNotDisturb":{},"deviceNotificationState":{},"bluetooth":{"cached":{"type":"bool","value":"true"}},"activities":{"startTime":{"type":"str","value":""},"size":{"type":"num","value":"1"},"offset":{"type":"num","value":"1"}},"account":{},"conversations":{"latest":{"type":"bool","value":"true"},"includeHomegroup":{"type":"bool","value":"true"},"unread":{"type":"bool","value":"false"},"modifiedSinceDate":{"type":"str","value":"1970-01-01T00:00:00.000Z"},"includeUserName":{"type":"bool","value":"true"}},"automationRoutines":{"limit":{"type":"num","value":"2000"}},"musicProviders":{},"homeGroup":{},"devicePreferences":{},"smarthomeDevices":{},"smarthomeGroups":{},"smarthomeEntities":{},"smarthomeBehaviourActionDefinitions":{}},"x":350,"y":40,"wires":[["a7349248.3b95c","6c8f611b.fd599","e010d69f.88cbd8"]]},{"id":"a7349248.3b95c","type":"change","z":"d88ff07e.1817a","name":"","rules":[{"t":"set","p":"options","pt":"msg","to":"payload.devices[capabilities[$ in \t['TUNE_IN', 'AMAZON_MUSIC', 'SPOTIFY']\t]].accountName[]","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":340,"y":80,"wires":[["25ee5eb3.b1c912","a8001117.5c831"]]},{"id":"25ee5eb3.b1c912","type":"ui_dropdown","z":"d88ff07e.1817a","name":"","label":"Device","place":" ","group":"eda16006.f7366","order":1,"width":"5","height":"1","passthru":true,"options":[{"label":"","value":"","type":"str"}],"payload":"","topic":"","x":310,"y":120,"wires":[["15d80731.539739"]]},{"id":"16d267af.07a6f8","type":"alexa-remote-get","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","target_type":"select","target_value":"musicProviders","serialOrName_type":"str","serialOrName_value":"","options":{"devices":{},"cards":{"limit":{"type":"num","value":"10"},"beforeCreationTime":{"type":"str","value":"%t"}},"media":{},"playerInfo":{},"list":{"size":{"type":"num","value":"100"},"startTime":{"type":"str","value":""},"endTime":{"type":"str","value":""},"completed":{"type":"bool","value":"false"},"listType":{"type":"select","value":"TASK"}},"wakewords":{},"notifications":{"cached":{"type":"bool","value":"true"}},"doNotDisturb":{},"deviceNotificationState":{},"bluetooth":{"cached":{"type":"bool","value":"true"}},"activities":{"startTime":{"type":"str","value":""},"size":{"type":"num","value":"1"},"offset":{"type":"num","value":"1"}},"account":{},"conversations":{"latest":{"type":"bool","value":"true"},"includeHomegroup":{"type":"bool","value":"true"},"unread":{"type":"bool","value":"false"},"modifiedSinceDate":{"type":"str","value":"1970-01-01T00:00:00.000Z"},"includeUserName":{"type":"bool","value":"true"}},"automationRoutines":{"limit":{"type":"num","value":"2000"}},"musicProviders":{},"homeGroup":{},"devicePreferences":{},"smarthomeDevices":{},"smarthomeGroups":{},"smarthomeEntities":{},"smarthomeBehaviourActionDefinitions":{}},"x":370,"y":200,"wires":[["ee47f9ff.882368","6c8f611b.fd599"]]},{"id":"ee47f9ff.882368","type":"change","z":"d88ff07e.1817a","name":"","rules":[{"t":"set","p":"options","pt":"msg","to":"payload.id","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":340,"y":240,"wires":[["7ac72874.165a98"]]},{"id":"7ac72874.165a98","type":"function","z":"d88ff07e.1817a","name":"pretty label","func":"let prettify = s => {\n\ts = s.replace('_', ' ');\n\ts = s.toLowerCase();\n\ts = s.split(' ').map(w => w[0].toUpperCase() + w.slice(1)).join(' ');\n\treturn s;\n};\n\nmsg.options = msg.options.map(o => {\n    let obj = {};\n    obj[prettify(o)] = o;\n    return obj;\n});\n\nreturn msg;","outputs":1,"noerr":0,"x":330,"y":280,"wires":[["85ce383c.682918"]]},{"id":"dae02564.bc9618","type":"ui_template","z":"d88ff07e.1817a","group":"247a1c57.1e0984","name":"Repeat","order":4,"width":"2","height":"1","format":"<div style=\"margin:auto;\">Repeat</div>","storeOutMessages":true,"fwdInMessages":true,"templateScope":"local","x":120,"y":660,"wires":[[]]},{"id":"8ab17a1e.075228","type":"ui_button","z":"d88ff07e.1817a","name":"Repeat On","group":"247a1c57.1e0984","order":5,"width":"2","height":"1","passthru":false,"label":"On","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":290,"y":660,"wires":[["f7ddb56.71ecd48"]]},{"id":"b2e81bab.3af268","type":"ui_button","z":"d88ff07e.1817a","name":"Repeat Off","group":"247a1c57.1e0984","order":6,"width":"2","height":"1","passthru":false,"label":"Off","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":290,"y":700,"wires":[["bcfb38bc.b67868"]]},{"id":"f7ddb56.71ecd48","type":"alexa-remote-command","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","command_type":"select","command_value":"repeat","options":{"play":{},"pause":{},"next":{},"previous":{},"forward":{},"rewind":{},"volume":{"value":{"type":"msg","value":"payload"}},"shuffle":{"value":{"type":"bool","value":"true"}},"repeat":{"value":{"type":"bool","value":"true"}}},"x":490,"y":660,"wires":[["6c8f611b.fd599"]]},{"id":"bcfb38bc.b67868","type":"alexa-remote-command","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","command_type":"select","command_value":"repeat","options":{"play":{},"pause":{},"next":{},"previous":{},"forward":{},"rewind":{},"volume":{"value":{"type":"msg","value":"payload"}},"shuffle":{"value":{"type":"bool","value":"false"}},"repeat":{"value":{"type":"bool","value":"true"}}},"x":490,"y":700,"wires":[["6c8f611b.fd599"]]},{"id":"788d4f1b.3e80d","type":"ui_template","z":"d88ff07e.1817a","group":"247a1c57.1e0984","name":"Shuffle","order":7,"width":"2","height":"1","format":"<div style=\"margin:auto;\">Shuffle</div>","storeOutMessages":true,"fwdInMessages":true,"templateScope":"local","x":110,"y":740,"wires":[[]]},{"id":"69951260.496b7c","type":"ui_button","z":"d88ff07e.1817a","name":"Shuffle On","group":"247a1c57.1e0984","order":8,"width":"2","height":"1","passthru":false,"label":"On","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":290,"y":740,"wires":[["47cd16df.99aba8"]]},{"id":"4324031b.ed1a8c","type":"ui_button","z":"d88ff07e.1817a","name":"Shuffle Off","group":"247a1c57.1e0984","order":9,"width":"2","height":"1","passthru":false,"label":"Off","color":"","bgcolor":"","icon":"","payload":"","payloadType":"str","topic":"","x":290,"y":780,"wires":[["c2ec8480.993518"]]},{"id":"47cd16df.99aba8","type":"alexa-remote-command","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","command_type":"select","command_value":"shuffle","options":{"play":{},"pause":{},"next":{},"previous":{},"forward":{},"rewind":{},"volume":{"value":{"type":"msg","value":"payload"}},"shuffle":{"value":{"type":"bool","value":"true"}},"repeat":{"value":{"type":"bool","value":"true"}}},"x":490,"y":740,"wires":[["6c8f611b.fd599"]]},{"id":"c2ec8480.993518","type":"alexa-remote-command","z":"d88ff07e.1817a","name":"","account":"fb0d8688.41ff08","serialOrName_type":"flow","serialOrName_value":"device","command_type":"select","command_value":"shuffle","options":{"play":{},"pause":{},"next":{},"previous":{},"forward":{},"rewind":{},"volume":{"value":{"type":"msg","value":"payload"}},"shuffle":{"value":{"type":"bool","value":"false"}},"repeat":{"value":{"type":"bool","value":"true"}}},"x":490,"y":780,"wires":[["6c8f611b.fd599"]]},{"id":"bf4c6368.dc95f","type":"ui_template","z":"d88ff07e.1817a","group":"3975ddef.f5f2c2","name":"Status","order":0,"width":0,"height":0,"format":"<div style=\"margin:auto; margin-top:0;\" ng-bind-html=\"msg.payload\"></div>","storeOutMessages":true,"fwdInMessages":true,"templateScope":"local","x":990,"y":440,"wires":[[]]},{"id":"6c8f611b.fd599","type":"switch","z":"d88ff07e.1817a","name":"","property":"error","propertyType":"msg","rules":[{"t":"istype","v":"object","vt":"object"},{"t":"istype","v":"undefined","vt":"undefined"}],"checkall":"true","repair":false,"outputs":2,"x":660,"y":440,"wires":[["4ecf8aba.a11784"],["7c548b33.567704"]]},{"id":"4ecf8aba.a11784","type":"change","z":"d88ff07e.1817a","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"error.message","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":820,"y":420,"wires":[["bf4c6368.dc95f"]]},{"id":"7c548b33.567704","type":"change","z":"d88ff07e.1817a","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"Success","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":820,"y":460,"wires":[["bf4c6368.dc95f"]]},{"id":"4a2d7db9.758794","type":"ui_button","z":"d88ff07e.1817a","name":"Refresh Device","group":"eda16006.f7366","order":2,"width":"1","height":"1","passthru":false,"label":"","color":"","bgcolor":"","icon":"refresh","payload":"","payloadType":"str","topic":"","x":120,"y":40,"wires":[["9abd9be4.9e36f8"]]},{"id":"d9d1ff9b.e2bd3","type":"ui_button","z":"d88ff07e.1817a","name":"Refresh Provider","group":"eda16006.f7366","order":4,"width":"1","height":"1","passthru":false,"label":"","color":"","bgcolor":"","icon":"refresh","payload":"","payloadType":"str","topic":"","x":130,"y":200,"wires":[["16d267af.07a6f8"]]},{"id":"ada7ca3b.c65238","type":"inject","z":"d88ff07e.1817a","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":80,"wires":[["9abd9be4.9e36f8"]]},{"id":"e010d69f.88cbd8","type":"debug","z":"d88ff07e.1817a","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":800,"y":100,"wires":[]},{"id":"a8001117.5c831","type":"debug","z":"d88ff07e.1817a","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"true","x":800,"y":180,"wires":[]},{"id":"eda16006.f7366","type":"ui_group","z":"","name":"Choose","tab":"594e577e.fe59b8","order":1,"disp":true,"width":"6","collapse":false},{"id":"247a1c57.1e0984","type":"ui_group","z":"","name":"Controls","tab":"594e577e.fe59b8","order":2,"disp":true,"width":"6","collapse":false},{"id":"3975ddef.f5f2c2","type":"ui_group","z":"","name":"Status","tab":"594e577e.fe59b8","disp":true,"width":"6","collapse":false},{"id":"594e577e.fe59b8","type":"ui_tab","z":"","name":"Alexa Music","icon":"music_note","order":2}]
```
