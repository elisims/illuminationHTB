# Illumination Forensic Box - HTB Project

## Description
- Hack The Box (HTB) challenges have become a popular way to test new techniques, hone your skills, and create content. These labs can be quite fun, as it’s not usually a single person creating a whole course, but are generally one-off scenarios (so there’s no stress to not finishing). 
- While “Hack The Box” is a company name that hosts some, these challenges can be included in CTFs, and well as a good way of demonstrating new tooling in an environment that was set up specifically for that purpose.

## Tools Utilized
- HTB (HackTheBox)
- “Illumination” forensic box
  - https://app.hackthebox.eu/challenges/Illumination
- Git
  - Installed using the command “sudo apt install git-all” if not already automatically installed on the Linux distro.
- VMWare Workstation 16
- Kali Linux VM

# Report

## Introduction
- While researching which challenge from HTB (HackTheBox) I was going to attempt, I looked under “Forensics” because of my interest in Digital Forensics. I am still not the most experienced “hacker”, so I knew I wanted to select a medium difficulty box, therefore allowing me to gain confidence and experience to eventually branch out into harder instances in the future. The instance that caught my eye is titled “Illumination”, as it had very good reviews on the HTB website and didn’t require a connection to an instance- Just one .zip file download.

## Initial Recon
- Once the .zip file finished downloading onto my Kali Linux VM, I extracted the files into the documents folder.

![img](https://github.com/elisims/illuminationHTB/raw/main/images/1.PNG)

- I then opened a new terminal window in this folder location by right clicking the folder and selecting the option “open terminal window here”.


![img](https://github.com/elisims/illuminationHTB/raw/main/images/2.PNG)

- Once the terminal window was open, I began the initial recon process by inputting a simple “ls” command which shows the files in the current directory. After checking the output of these two files, I realized that nothing of interest would come from these current files available.


![img](https://github.com/elisims/illuminationHTB/raw/main/images/3.PNG)

- I then input the command “ls -a” which shows both regular and hidden files in the current directory, and viola- a hidden folder named “.git” has now been uncovered.


![img](https://github.com/elisims/illuminationHTB/raw/main/images/4.PNG)

## Exploring the .git hidden folder
- Realizing that there was a .git repo within this illumination.js folder made me start looking within the log files first and foremost. Using the “git log” command, the logs of the git repository are output to the terminal. 

![img](https://github.com/elisims/illuminationHTB/raw/main/images/5.PNG)
![img](https://github.com/elisims/illuminationHTB/raw/main/images/6.PNG)


## Researching each git log
- Starting with the earliest available log from the list of log entries, I copied the bottom commit hash and used the command “git show {log-commit-hash}”.

![img](https://github.com/elisims/illuminationHTB/raw/main/images/7.PNG)

- Looking through the output given by this command didn’t prove fruitful, as there was nothing suspicious given. The output of the command was as follows:


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

- I then tried the next commit hash from the previous output of the git logs using the same command “git show {log-commit-hash #2}”.

![img](https://github.com/elisims/illuminationHTB/raw/main/images/8.PNG)

- Looking through the output given by this command didn’t prove fruitful either, as there was nothing suspicious given. The output of the second command was as follows:

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

- This third log caught my eye initially when I first ran the “git log” command because the comment associated with it stated that the unique token was removed- Suggesting a static auth token would be somewhere within the code now (big security risk), however, I still wanted to go through all the previous logs just in case there could have been other vulnerabilities. Anyway, using the same command as before, we receive the following output:

![img](https://github.com/elisims/illuminationHTB/raw/main/images/9.PNG)

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

- Bingo, the red “token” looks very suspicious and will most likely lead to the flag after some decryption. I loaded up Firefox and went to the website “tunnelsup.com” which is my current favorite website to quickly identify which encryption algorithm has been used on a provided string.

![img](https://github.com/elisims/illuminationHTB/raw/main/images/10.PNG)

- The website identified that no encryption was used but the character type is base64 which can be easily reversed with a simple command using the terminal.

![img](https://github.com/elisims/illuminationHTB/raw/main/images/11.PNG)

- The output provided by the command 
“echo 'SFRCe3YzcnNpMG5fYzBudHIwbF9hbV9JX3JpZ2h0P30=' | base64 --decode” is as follows:

```python

┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ echo 'SFRCe3YzcnNpMG5fYzBudHIwbF9hbV9JX3JpZ2h0P30=' | base64 --decode
HTB{v3rsi0n_c0ntr0l_am_I_right?}                                                                                           
┌──(kali㉿kali)-[~/Documents/Illumination.JS/.git]
└─$ 

```

- The flag “HTB{****************}” had been discovered, so I input the flag into HTB to confirm it was correct, and indeed the challenge was over.

![img](https://github.com/elisims/illuminationHTB/raw/main/images/12.PNG)

## Conclusion
- This HTB challenge was another AMAZING confidence and morale booster for me- As I was able to utilize the basic knowledge of Git I had in order to figure out and solve the challenge. I don’t think it was quite as challenging as the main page for it suggested (I would have preferred a little more of a challenge), but practicing any skills is important for a beginner pentester like myself.
- I would recommend this to any beginner pentester who is interested in practicing their forensic and Git skills at the same time.

