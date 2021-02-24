# EWIJSON and Script Guide


## Preamble
This guide focuses on Zynj's AI Dungeon script EWIJSON and primarily on using it for manipulating World Info and the context. This guide is intended to motivate someone more dedicated to write an advanced guide on how to better utilize it's many useful features. This guide works as an introductory bootcamp to get you started. It is assumed that you've already read most of the AID research sheet and understand how most of AID's context works. To recap:
```
- Max. input character length: 2772
- Max. Summary length: 500
- If (Summary + WI + Remember) < 50%, then extra story aka history will fill up the empty space
- If (Story + A/N + Input + frontMemory) < 50%, the unused space will not be allocated to (Summary + WI + Remember)
- GPT-3 has a hard cap of 1024 tokens for AID users, 2048 for the devs
```

EWIJSON and the wiki written by Zynj can be found at:  
https://github.com/Zynj-git/AIDungeon/tree/master/AID-Script-Examples/EWIJSON/release  
https://github.com/Zynj-git/AIDungeon/wiki/EWIJSON

Other useful scripts exist as well and will be mentioned. STARSTRUCK has written a userscript for Tampermonkey/Greasemoney that can be combined with any AID specific script for the purpose of modifying the UI and adding artwork from https://www.artbreeder.com/ . The script can be found at:  
https://gitlab.com/STARSTRUCK/ai-dungeon-userscripts  

There are other skilled scripters like Gnurro. Scripts may be merged, although the process requires some coding knowledge. A guide on script merging must be written and provided by someone with more knowledge on the topic. We provide a link to Gnurro's useful scripts:  
https://github.com/Gnurro/AIDscripts  


## Installing EWIJSON
To use a script in AID you must first write or install it. Either download the 4 json files from Zynj's github or acquire a release zip through the official AID Discord channel #scripting. As of writing it is on version 1.1.9. From inside the AID UI go to the menu > my stuff > make scenario > scripts. Usually this is where you write/develop your own scripts. For a release of EWIJSON we press the upwards arrow and import the zip or the json files. After this is done and you see the code inside shared library, input modifier, context modifier and output modifier the script(s) is ready to be used only within this scenario. You must repeat this for every scenario you wish to use scripts with.


## Using EWIJSON
Reading Zynj's wiki will get you set up on the basics and how to write WI using EWIJSON. Usually this is done inside the scenario you're playing. The main purpose of EWIJSON is introducing regular expression (regexp) functionality inside AID and by default pulling any key written using EWIJSON recognized formats right next to the key in the context towards the front of the fresh context. With the use of attributes you can place your triggered WI wherever you want in the context. Any regular style WI won't be called by EWIJSON and will function identically to how it would inside vanilla AID. These functionalities make it the swiss-army-knife of scripts, allowing you to achieve any kind of text editing that can be done using JavaScript standard ECMAScript regexp. For checking your regexp syntax the author recommends using the following site:  
https://regex101.com

Zynj's wiki contains many examples and ideas on how to start using the script. Some of them may be even more advanced than the methods discussed in this document. As of current version EWIJSON entries are bad with newlines so avoid using those. For now we will discuss two tricks rolled into one example. It is possible to use any other WI format together with EWIJSON. Thanks to EWIJSON WI being close to your inputs, it's also the best way to define `you` inside AID at the moment. Example on how to achieve this alongside synonyms for your name:
```
KEYS				|	ENTRY
yourname.character		|	yourname:[Zaltys format stuff using you as the noun.]
yourname._synonyms		|	(your|full name|nicknames|titles)
REMEMBER			|	Your name is yourname. The rest of the remember continues...
```
The rest of the guide is dedicated to a more thorough analysis and use of EWIJSON also teaching the tricks birb uses for it. First we provide the condensed TL;DR workflow that you can skip or keep reading if you're here for finer details or advanced uses. Keep in mind that future updates to EWIJSON may have bugs, introduce new features or change things that may change the functionality of methods discussed here.


## The Ultra Condensed Numbered Point Edition
If this doesn't help you, I can't help you. This is the complete standard workflow for defining things in EWIJSON without using heavy branching.
1. Decide on a WI. Character or location for starters. Decide on initial attribute for your characters. `.character` is good. We will now create everything though the input field, or `>`
2. Set your first character. Let's go with John. `/set John.character John`. Here, .character denotes the name `John` as a JSON-object.
3. John needs more traits. Let's go with clothes and mental first. `/set John.worn business suit`. He now wears a business suit defined under a new entry. We repeat for `/set John.mental workaholic` because John loves his job. Now we could repeat this for what his workplace is...
4. John has a full name. We want the AI to understand this. We do `_synonyms` which also means `_conditions`. We do `/set John._synonyms (John Doe|The Workaholic|John The Workaholic)`. Now the AI recognizes his full name and titles. John Doe implies => John.<object> and calls the entry for that WI.
5. Manually edit your whitelist or do `/set _whitelist. character,worn,mental`. Every time you edit white whitelist this way you must set every desired object. Alternatively you can `/whitelist word` one at a time to either add the word to the whitelist or remove it if its included already. Objects/traits must be whitelisted to automatically trigger.
6. We want a WI that triggers with a condition and ISN'T whitelisted. John's secret. We do the regexp `/set _synonyms.John.secret (John).*(secrets?)|(secrets?).*(John)` that will call the entry we also create `/set John.secret John used to wet his bed until he was 8`
	1. Whenever `John` or the synonyms is mentioned in the context EWIJSON will insert everything under John.(character|worn|mental) in the history. The secret will only get inserted with the other WI entries if we mention `John's secret` or John followed by a short sequence of letters then the word `secret`.
7. We want an example of how to use EWI attributes and we want an Author's Notes for our scenario (without using the standard AN UI). We do `/set .#[p=3] [Author's Note: John the Workaholic just wants to live a quiet life. But in a fight he wouldn't lose to anyone.]` Explanation: In regexp just `.` matches everything. It's always active. The EWI function #[p=x] sets the entry x lines up in context. Here we use p=3 lines, literally copying the standard Author's Notes used in AID.


## Complete beginner's guide to getting started
First consider if you even need or want the script. Every little bit of the tool is consistently formed around it's designed purpose - to dynamically and with relatively low effort manage your WI on the fly through your input field. As long as you have good memory and have understood how regexp works you can easily manage all your WI related data structures from the input field. Its designed to be object-oriented for the purpose of being pseudo-dynamic. If you use a lot of WI and you tweak them often EWIJSON is perfect for you, if this isn't you, you might even prefer using regular old WI. While it can't automatically change the objects for now, you can change any object whenever you want and get immediately changed results from the output. 

Before we continue keep in mind that the AI reads strings in the context and matches them with the keys inside your WI definitions. Then, the entry of the matched key gets inserted in a certain place which is as far back down the context as possible in vanilla. In EWIJSON the match is by default inserted immediately above the matching key in context. This can further be manipulated with EWI attributes.

First you must pick how deep you want to go in the tree of JSON objects. Because at every level you are given an unlimited amount of objects, the least amount you need is \<root\>.\<object\>. For our example we have `John.character`. If we don't want to get more specific we can write an entry for `John.character` and to add a different category/object with different entries we can write `John.traits`. For our example we use valid objects at different depth levels.

Say you have `John.character` and want to add branching attributes to this group. We pick `mental` and `traits`. Mentally John is docile, moderate and agreeable so we do `/set John.character.mental docile, moderate, agreeable`. We want some matching traits and to make John a workaholic. So we do `/set John.character.traits workaholic, boring, average`. `John.character` doesn't get any entries because the AI wouldn't be able to see them anyway.

Next we decide to put John's clothes or worn elsewhere because it's not as related to his characterization. He might want to change his wardrobe later right? So first inside your WI you define `key:John.worn entry: pajamas`. After John wakes up and gets ready to work he will get dressed. After this is done we update with `/set John.worn business suit`. Following this example you've updated the entry for the former object to be `business suit`. If you ask the AI what John is wearing, it will be a description of some type of business suit. Since `John.worn` doesn't have sub-objects, the AI will see this entry alongside with all the subgroups from before as long as you've properly set your `keys:_whitelist.` as `mental,traits,worn`

`._synonyms` is combined with regexp to easily define multiple synonyms for your root. From a WI the author has defined we have `faun girl._synonyms` to define every key the AI recognizes for this root. In entry we have the regexp `(faun|satyr).*(girls?|females?|woman|lady)`. This makes every faun or satyr followed by the female nouns synonymous with `faun girl` from earlier. Then you can describe how you want this species to appear like inside your game with a key like `faun girl.species` with the entry containing your desired description.

`_synonyms` at the front is the same concept, but for objects you haven't whitelisted. The author has thus far noticed this is the best way to define sexual dimorphism, secrets or other boolean type things. Binaries or configurations of a singular concept that are drastically different. Species is whitelisted, but female isn't then we define key `_synonyms.draph.species.female` with entry `(draph).*(females?|woman|women|lady)|(female|woman|lady).*(draphs?)`. When the AI recognizes that the word draph is preceded or followed by the female nouns, the WI containing the information will be triggered which we have defined in `draph.species.female`. If we want males to have their own definitions we repeat the above process for `draph.species.male`.

This isn't even going into the EWI attributes. For those you have to think what you want in terms of manipulating the position or visibility of your WI inside the context. #[d] is used to match the key then place the entry in front of or after the match. #[p=x] tells the AI how many lines *down* in the context (visually up on your screen) you want the matched entry. The other attributes you can research on Zynj's wiki. You'll figure out how to use them if you need them.


## Specialized EWIJSON use
Here we describe methods the author has adopted for working with EWIJSON. World info can use just EWI attributes or be made into JSON-objects or use both. For simplicity, combining WI formats and highly granular control the author has started mixing both. This relies mostly on the use of the trailing positional [t] attribute. You can also use the weight [w] attribute which hasn't been documented in the wiki yet. When you use attributes with # in EWIJSON, the entry becomes an EWI-format WI. This means regexp is used within the key for inserting regular text from the entry. Note: whenever using EWI-format WI, never separate keys with comma `,` otherwise the former part gets treated as a regular WI while the EWI attribute applies only to the latter key. Separate keys with `|` which is the regexp for OR.

Let's use WI for the character `you` named John as an example. A Neanderthal format WI can only be done in EWI, because JSON has problems with newlines.
```
keys: (you(rself)?|John|John Doe)#[t=1]
entry: <John>:[ You are John Doe, a royal knight of Larion. Your title comes with prestige but also many burdens and responsibilities.]
<John>:[ You are a handsome and dapper dandy.]
```
The details included in that WI we assume to be permanent or something you rarely want to edit through the WI interface so we don't need to tweak it through the `/set` command. Since conditionals are defined in regexp we also set one of those using the [t] attribute since it's easier than maintaining many JSON-objects for synonyms.
```
keys: (your?).(secrets?)#[t] | if AID detects nearby input with you(r) followed by random text and secret(s), it will add the entry to the context
entry: <John>:[ You're secretly bald and wear a wig.]
```
We want to define changing things about John. These may require the use of `you._synonyms` unless you're content with only words containing `you` triggering them. We do `/set you.appear You have slick brown beard and hair.` `/set you.status You're in perfect health.` and `/set you.worn You wear a trusty suit of shiny steel plate armor.` We also set the whitelist `/set _whitelist. appear,status,worn`.

Now we need a prompt to show what the point of all of the above was. John is a knight of Larion tasked with things like hunting dragons. But he must check himself out before heading out for his duties. We define the context and give the AI an input that triggers John's secret. Then we look at what the LMI(Last Model Input) looks like.
```
Context: You are John, a knight of Larion. You are tasked with hunting the dragon that has been terrorizing the realm. Before you head out, you check yourself out and make sure you're ready.
```
We give the AI the input `You have a hairy secret` to trigger every WI mentioned above including John's secret. This is what the LMI will look like.
```
You are John, a knight of Larion. You are tasked with hunting the dragon that has been terrorizing the realm. Before you head out, you check yourself out and make sure you're ready.
<John>:[ You are John Doe, a royal knight of Larion. Your title comes with prestige but also many burdens and responsibilities.]
<John>:[ You are a handsome and dapper dandy.]
[{"appear":"You have slick brown beard and hair.","status":"You're in perfect health.","worn":"You wear a trusty suit of shiny steel plate armor."}]
<John>: [ You're secretly bald and wear a wig.]
You have a hairy secret
```
What the heck happened there? We exploited EWIJSON's design on how the trailing attribute and JSON-object default positioning works. By default, they get placed above the inputs with the keys that trigger the WI. This is true even with the [t=1] position, but it gives the associated WI less priority putting it behind the others or trailing it 1 action back. The default JSON-objects go where they're supposed to. The default trailing [t=0] makes the secret trail 0 actions back or right above the key mentioning it. Now as long as the AI is behaving well, it might output how your slick brown hair is actually a fake wig. This example was chosen for being easy to understand and is not a recommendation on what WI to use in practice.  


## EWIJSON Exploits
Using the power of EWIJSON, regex and understanding the memory load, it is entirely possible to 'hack' AID to do things previously thought impossible. This includes breaking past the dreaded Levenshtein limit. The following is a trick birb developed together with Mr.Accountant who did most of the testing to get it working. The idea is to push the entire old context out and replace it whatever you desire to completely wipe the memory of the AI and replace it with anything. This way you can summarize the story in the middle of your adventure without restarting or having to worry about exporting WIs. The result might as well as be called lobotomizing or brainwashing the AI.

First you must replace your Remember Pin. You are actually allowed to go over the "1000 character limit". Make it as long as you can, ideally near 1432 but longer than 1336. Then write or import entries with your 'reset#[t]' keyword using the #[t] attribute. An EWI using the [t] attribute is loaded in story Context, so it doesn't count to the world info limit and with a full back memory doesn't trigger the 85% context limit because only 50% is being changed. So the first half, you write inside Remember. The second half is contained in the keyword(s) fed into the AI in order. Testing this yourself, you will find that you can replace the entire LMI with whatever you want.

This is the flashforward, the flashback, King Crimson, Gold Experience Requiem, whatever meme superpower you want to call it.

Mr.Accountant's writeup on how to do to the patented (Zynj and friends) Context Nuke:  
1. Remember at 1432 characters. Don't pay attention to the 1000 limit, its only a suggestion now.  [m] should work if you load it before the world entries.
2. Have EWIJSON so you can control when this thing dissipates with the [l] attribute.
3. You need around 1300 characters in the frontMemory (Where you see your story). 
4. Have 1300 character in world entries hooked to to a VERY specific keyword that only can trigger. They need to be hooked to [t].
5. Watch as the context becomes whatever you want.


## Understanding EWI Attributes
Here we provide the reader with short, sweet and easy to understand metaphors for every major EWI attribute as of writing compliments to Mr.Accountant. You should check out the wiki to see each attribute and examples of their use. The metaphors may give some readers a more common sense understanding though.

Let's get on with the (very) informal EWIJSON attribute school:  
[d] = "I am going to insert this shit right before I find the key match. So your sword now become a stupid-ass sword."  
[l] = "How long before your entry gets bounced. [l=1] means bye bye next turn and your entry is removed from context. [l=-2] means the WI has to wait in line for 2 turns before being let in."  
[p] = "The Karen of the attributes, rude as heck, barges in exactly where it wants to go. If p says it wants to be three lines from the last input, that's where it's going."  
[m] = "[m] however is chill, tells the world entry to hang out at the remember's position in context."  
[t] = "[t] is that obxionous fan that follows you EVERYWHERE. When your name is mention, they are always right behind you or behind a bush. Imagine if your name was said [t] would be right behind you, or if you put a restraining order of [t=3] then he will be three steps behind the last mention of your name"  
[r] = "Slots! Picks a item from a group of OR conditionals. They need to be identical keys before # or [r] doesn't give a fuck."  
[x] = "This is the bouncer, world entry doesn't get in until he says enough time has passed. [x=4] means four actions must pass in the adventure before you can get this world entry to fire."


## Credits
Zynj for writing EWIJSON, updating it frequently and educating people on how to use it. STARSTRUCK for his artdungeon.user.js. Latitude for making scripting free. birb for writing this.