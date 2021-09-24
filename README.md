# Illumination Forensic Box - HTB Project

```python

┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ git show 335d6cfe3cdc25b89cae81c50ffb957b86bf5a4a
commit 335d6cfe3cdc25b89cae81c50ffb957b86bf5a4a
Author: SherlockSec <dan@lights.htb>
Date:   Thu May 30 22:16:02 2019 +0100

    Moving to Git, first time using it. First Commit!

diff --git a/bot.js b/bot.js
new file mode 100644
index 0000000..7eb834a
--- /dev/null
+++ b/bot.js
@@ -0,0 +1,90 @@
+//Discord.JS
+const Discord = require("discord.js");
+const client = new Discord.Client();
+const fs = require("fs");
+var config = JSON.parse(fs.readFileSync("./config.json"));
+
+
+//Node-Hue-API
+var hue = require("node-hue-api"),
+       HueApi = hue.HueApi,
+       lightState = hue.lightState;
+
+//Display command results in console
+var displayResult = function(result) {
+       console.log(JSON.stringify(result, null, 2));
+};
+
+//Function taken from campushippo.com
+var rgbToHex = function (rgb) { 
+  var hex = Number(rgb).toString(16);
+  if (hex.length < 2) {
+       hex = "0" + hex;
+  }
+  return hex;
+};
+
+//Function taken from campushippo.com
+var fullColorHex = function(r,g,b) {   
+  var header = "0x"
+  var red = rgbToHex(r);
+  var green = rgbToHex(g);
+  var blue = rgbToHex(b);
+  return header+red+green+blue;

```

```python

┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ git show ddc606f8fa05c363ea4de20f31834e97dd527381
commit ddc606f8fa05c363ea4de20f31834e97dd527381
Author: SherlockSec <dan@lights.htb>
Date:   Fri May 31 09:14:04 2019 +0100

    Added some more comments for the lovely contributors! Thanks for helping out!

diff --git a/bot.js b/bot.js
index 7eb834a..e582ba9 100644
--- a/bot.js
+++ b/bot.js
@@ -54,19 +54,19 @@ client.on("message", message => {
        const command = args.shift().toLowerCase(); 
 
        switch (command) {
-               case "light.off" :
+               case "light.off" : //Turn light off
                        api.setLightState(lightNum, state.off())
                       .then(displayResult)
                       .done();
                        message.channel.send("Light Off!");
                        break;
-               case "light.on" :
+               case "light.on" : //Turn light on
                        api.setLightState(lightNum, state.on())
                           .then(displayResult)
                           .done();
                        message.channel.send("Light On!");
                        break;
-               case "light.rgb" :
+               case "light.rgb" : //Change light colour
                        let r = args[0];
                        let g = args[1];
                        let b = args[2];
@@ -79,7 +79,7 @@ client.on("message", message => {
                                .setDescription(`Red Value: ${r}. Green Value: ${g}. Blue Value: ${b}`);
                        message.channel.send(embed);
                        break;
-               case "light.switch" :
+               case "light.switch" : //Switch to different light
                        let num = args[0];
                        lightNum = num;
                        //fs.writeFile("./config.json", JSON.stringify(config))
@@ -87,4 +87,4 @@ client.on("message", message => {
        }
```

```python

┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ git show 47241a47f62ada864ec74bd6dedc4d33f4374699
commit 47241a47f62ada864ec74bd6dedc4d33f4374699
Author: SherlockSec <dan@lights.htb>
Date:   Fri May 31 12:00:54 2019 +0100

    Thanks to contributors, I removed the unique token as it was a security risk. Thanks for reporting responsibly!

diff --git a/config.json b/config.json
index 316dc21..6735aa6 100644
--- a/config.json
+++ b/config.json
@@ -1,6 +1,6 @@
 {
 
-       "token": "SFRCe3YzcnNpMG5fYzBudHIwbF9hbV9JX3JpZ2h0P30=",
+       "token": "Replace me with token when in use! Security Risk!",
        "prefix": "~",
        "lightNum": "1337",
        "username": "UmVkIEhlcnJpbmcsIHJlYWQgdGhlIEpTIGNhcmVmdWxseQ==",
                                                                                           
┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ 

```

```python

┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ echo 'SFRCe3YzcnNpMG5fYzBudHIwbF9hbV9JX3JpZ2h0P30=' | base64 --decode
HTB{v3rsi0n_c0ntr0l_am_I_right?}                                                                                           
┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ 

```
