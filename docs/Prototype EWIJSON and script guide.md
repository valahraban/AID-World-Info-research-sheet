# EWIJSON and Script Guide


## Preamble
This guide focuses on Zynj's AI Dungeon script EWIJSON and primarily on using it for manipulating World Info and the context. This guide is in the prototyping stage and is intended to motivate someone more dedicated to write an advanced guide on how to better utilize it's many useful features. It is assumed that you've already read most of the AID research sheet and understand how most of AID's context works. To recap:
```
- Max. input character length: 2772
- Max. Summary length: 500
- If (Summary + WI + Remember) < 50%, then extra story aka history will fill up the empty space
- If (Story + A/N + Input + frontMemory) < 50%, the unused space will not be allocated to (Summary + WI + Remember)
- GPT3 has a hard cap of 1024 tokens for AID users, 2048 for the devs
```
EWIJSON and the wiki written by Zynj can be found at:

https://github.com/Zynj-git/AIDungeon/tree/master/AID-Script-Examples/EWIJSON/release

https://github.com/Zynj-git/AIDungeon/wiki/EWIJSON

Other useful scripts exist as well and will be mentioned. STARSTRUCK has written a userscript for Tampermonkey/Greasemoney that can be combined with any AID specific script for the purpose of modifying the UI and adding artwork from https://www.artbreeder.com/ . The script can be found at:

https://gitlab.com/STARSTRUCK/ai-dungeon-userscripts


## Installing EWIJSON
To use a script in AID you must first write or install it. Either download the 4 json files from Zynj's github or acquire a release zip through the official AID Discord channel #scripting. As of writing it is on version 1.1.7. From inside the AID UI go to the menu > my stuff > make scenario > scripts. Usually this is where you write/develop your own scripts. For a release of EWIJSON we press the upwards arrow and import the zip or the json files. After this is done and you see the code inside shared library, input modifier, context modifier and output modifier the script(s) is ready to be used only within this scenario. You must repeat this for every scenario you wish to use scripts with.


## Using EWIJSON
Reading Zynj's wiki will get you set up on the basics and how to write WI using EWIJSON. Usually this is done inside the scenario you're playing. The main purpose of EWIJSON is introducing regular expression (regexp) functionality inside AID and by default pulling any key written using EWIJSON recognized formats right next to the key in the context towards the front of the fresh context. With the use of attributes you can place your triggered WI wherever you want in the context. Any regular style WI won't be called by EWIJSON and will function identically to how it would inside vanilla AID. These functionalities make it the swiss-army-knife of scripts, allowing you to achieve any kind of text editing that can be done using JavaScript standard ECMAScript regexp. For checking your regexp syntax the author recommends using the following site:

https://regex101.com

Zynj's wiki contains many examples and ideas on how to start using the script. Some of them may be even more advanced than the methods discussed in this document. For now we will discuss two tricks rolled into one example. It is possible to use any other WI format together with EWIJSON. Thanks to EWIJSON WI being close to your inputs, it's also the best way to define `you` inside AID at the moment. Example on how to achieve this alongside synonyms for your name:
```
KEYS		|		ENTRY
yourname.character		|		yourname:[Zaltys or Neanderthal format stuff using you as the noun.]
yourname._synonyms		|		(your|full name|nicknames|titles)
REMEMBER		|		Your name is yourname. The rest of the remember continues...
```

## Credits
Zynj for writing EWIJSON, updating it frequently and educating people on how to use it. STARSTRUCK for his artdungeon.user.js. Latitude for making scripting free. birb for writing this. 