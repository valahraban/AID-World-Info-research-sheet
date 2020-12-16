# AI Dungeon World Info Reference
***


## Preamble
Turn on word wrap to read this document. Perhaps the greatest addition to the AI Dungeon for generating high quality stories has been world info. Learning and figuring out how to use world info effectively is of great interest to many users: using it one can get vastly more consistent output from the AI and define terms that the pre-trained AI might otherwise have difficulty understanding. Features like `Remember` (pins) and `A/N:` (author's notes) will also be covered as they are heavily related and connected to world info. Everything here is of use to both free and premium users. The differences between griffin and dragon will be discussed as there are some differences one should be mindful of. Good world info can make griffin almost as good as dragon, but requires more attention and effort from the user. While world info is a powerful tool there are also caveats and concerns to using it effectively. With good world info the user can create any kind of scenario and a `world` of their own. All of this info is from users sharing notes and research on AID Discord and other fan communities. The information has been confirmed and peer-reviewed through sharing outputs which will be skipped for the sake of brevity. Special credit to kimtaengsshi for help and fact checking the technical details. Recently there have been rumors of WI 2.0 launching at an unknown future date. This document will require an overhaul after this comes to pass.


## Remember, world info, Author's Notes, Worlds and how the game treats each part
Remember was the first of these features implemented in the game. Glossing over a lot of details, remember is a world info that gets constantly fed to the game after your every input. World info is like remember, but has user-defined key-words `keys` that after getting triggered make the game use them as input. Having this element of context makes world info less restricting to work with - after you're happy with it you can forget and leave it while things in remember require more frequent updating as the story progresses. History means old prompts written by you and the AI. The order the AI reads your input is in:

+ Summary										| Experimental feature to help devs
+ WI
+ Remember
+ History
+ AN if you've at least 3 newlines in history
+ Rest of history
+ Fresh Input
+ Front memory from scripting					| Advanced/premium users

All of this consumes characters or as we will soon discuss tokens from the AI's `memory`. Anything that goes over the `memory` limitations will not be read by GPT-3 for the sake of producing new output. In regular use a single WI has a limit of 500 and remember has a limit of 1000 characters. On the plus side thus far no limits have been discovered on *how many* WI one can define. It's not recommended, but it is possible to define multiple WI with the same keys, essentially increasing the space by 500 chars on every iteration. AID is smart about using WI, but not about remember and updating remember constantly is tiresome - therefore many advanced users focus on producing high quality WI while foregoing longer remember pins. The limitations of GPT-3 and AID is why understanding and researching WI is of interest.

A lot of research hasn't been done on worlds. They contain the same data structures, a lot of it being hidden from the player. Most WI enthusiasts prefer making their own custom scenarios. Some discoveries have been made to enhance the World experience. This is usually done through the `Name` field during character creation this being the main way of influencing how the World generates. You input your info into the `Name` field. This is most effective with json-like formats. For example if you put `[{"name":"Eve","theme":"pirate"}]` in the name, the World will name your character Eve, while giving the World a noticeably more pirate theme. Quoting what we know so far from a user named Gen.Alexander:
```
Prose names like "Bill the fallen paladin," "Jessica the necromancer," etc. affect World prompt generation too. But with JSON you can really cram a lot of stuff into it (and it's likely to include at least half of it in the prompt.) Like
[{"name":"Jessica","personality": "ambitious", "birthplace": "Waterdeep", "worships":{"Mystra":"godess of magic"},"relationship":{"Elminster":["teacher","boyfriend"]} }]
I just tried "theme" for that yesterday and it seems to work too.
```


## Author's Notes specifics
Let's discuss `A/N:` which is sold as a premium feature - the way it automatically works is extremely convenient. But all it really does is insert a line of text into the input the AI takes `Author's Note: Some details like mood about this story` and has a short character limit of 200. Discovering this special input and how heavily it affected output is why it was added. But even as a free user, you can manually add Author's Notes in your story. With manual A/Ns you have much more characters to work with, but it is doubtful that making a super long A/N: offers any extra benefit. Editor's Note is another interesting take on the concept by the community. For paid users there exists a script that can replace the paid `A/N:` with an `E/N:`. Research suggests that A/N: can be used to reinforce the style of well-known authors through repetition. It can also be used to change perspectives and the `focus` of the storytelling. These findings suggest that the prompt informs the AI what kind of writing to expect directly after A/N: has been invoked. The following is a longer A/N: using some tricks and reinforcing style.
`[Author's note: This novel has no plot twists. Author: H. P. Lovecraft. Writing style: narrative, in the style of H. P. Lovecraft. Genre: Lovecraftian horror. The pacing is deliberate and the writing pays vivid attention to detail.]`

Switching focus is easy, but may require extra use of alter afterwards to make sure the AI understands what you want. This style of A/N: is best used manually written into the story (although you can change your global A/N whenever you want - this being another recommended trick).
`[Author's note: The story now follows Mary's perspective.]` or `[Author's note: We now switch focus on Mary.]`
The main character can also be defined in A/N vs memory `A/N: Main character: <species>, <gender>, <job>`

A/N: likes both style-describing nouns and adjectives. `strikingly elegant` makes the AI output more elegant prose than just `elegant`. This may also work for other words predicated with strikingly. `THEME:` is a word that can get the AI to focus on concepts and their attributes such as pirates or ninjas.
An example of output generated with `strikingly elegant style`:
```
You look around.
The sun softly kisses your exposed skin with her rays. You gently bask in her glory, raising your face towards her. Below, the plants softly sway in a gentle breeze as though worshipping her as well.
```
Short list of useful A/N words: AUTHOR, WRITING STYLE (Writing style:grandiose), GENRE, THEME
A/N: understands JSON too. So it's possible to do `A\N: [{"writing style":["descriptive", "elegant", "gritty"], "wording":["archaic", "Cockney accent"], "current state":"indoors"}]


## Tokenization - understanding the true limitations as well as special characters
The character limitations mentioned in the previous section are ones imposed by the devs of AID. They are quite sensible, plenty for generating good stories, not bankrupting Latitude and not horribly far off from the true limitations of GPT-3 that both models now run on. For a machine learning transformer to read an input it must first be tokenized into a form the model understands. GPT-3 has a hard limit of 2048 tokens. It won't read more at once. Devs have access to 2048 tokens, the users are given 1024 per input. But as you've noticed, you have more words to work with that the AI clearly understands. Tokens are just the way data is encoded into the machine learning model - during training, common words became recognized and encoded into an efficient form, sometimes even a large word gets recognized as 1 token. From kim's research, depending on how a world info is defined, the counts average to 2-4 characters per token meaning you get to use at least double the characters of the tokens you really have hence why a commonly given character limit is 2048. These are rough estimates - there are other limitations going on in AID and it's updated all the time. 

A few things have been learned from experience - common words and phrases like `hello` or `25y` get treated as a single token. White spaces are special - they often get rolled into words as is grammatically correct. An uncommon word like ` Enterprise` takes 1 token with the white space while `Enterprise` takes 2 tokens. Unicode emoji like `😃` and some special symbols are expensive, they cost 2 tokens per character. As we will discuss characters like `<>` `[]` are useful for WI, but tend to have a cost ratio of 1 token. There are also rare characters that the AI hates like `ù`. Experiments have shown that longer strings of symbols like `}}}` get rolled into one token.

If you want to test out and learn more about GPT-2/3 tokens (they work the same) visit this link: https://colab.research.google.com/drive/1i1wXfc4SWDprHZuLTaMsgWPv6YwvlQ_I?usp=sharing#scrollTo=mlBq9zO67b2n
Recently, kim has also made a tokenizer scenario/script you can use inside AID: https://play.aidungeon.io/main/scenarioView?publicId=6927d610-34b7-11eb-b8e1-c185d026672d
NOTE: The information there is subject to become outdated as the devs have ways of tweaking how tokenization works. Also, scenario authors might sometimes hide their scenarios, usually as they get outdated and updated.
An important thing we learned here is if your writing is highly efficient, you can get more tokens to play with than your character limit implies. Advanced users can bypass the regular world info character limit by writing their own JSON and importing it manually into AID. This makes it so that you can only delete letters from your oversized WI though and this method is generally not recommended.


## Griffin vs Dragon
Modern versions of AID Griffin and Dragon are both GPT-3. Many forums still spread misinformation. This has been confirmed by the devs. That being said there are different sizes of GPT-3. Griffin uses a subset of GPT-3 that is closer to the original 1.542 billion parameters used by the GPT-2. The full-sized GPT-3 that Dragon is based on has a whopping 175 billion parameters. This shows in the quality, consistency and most importantly *forgiveness* of the output. While Dragon loves details at least as much as Griffin does it'll be able to produce higher quality prose with less effort from the user. This shows especially in how differently it responds to WI you feed it. Some precursory research has been made into this - you can get roughly similar WI results on both models, but Griffin has quirks you have to keep in mind. These affect what kind of WI you write and will be discussed in the following sections.
An interesting thing to mention here - Your first prompt and the response to it by the AI are relatively unimportant as they are generated in *GPT-2*. Detailed WI is more important as the AI starts outputting quality content. With AID you should focus building on success or an interesting response to your input. Also if you are experimenting with new WI formats, keywords and categories, you might want to do the experimentation after 2 regular outputs from AID. Just make sure the writing is consistently high quality and the AI output should reward you for it.


## On certain characters
Functionalities of certain characters have been found and described thanks to the development of `world info formats` but it's useful to discuss these beforehand. Their functionality can be somewhat verified through the GPT-2 tokenizer Colab. Some characters connect. The most important of these is -. `hair-color` or `red-color` are words that gets better results for colorization than other methods (changed as of December, use redcolor or redhue instead). `somecolor-hue` doesn't work as well. Other characters have been tried for connecting words, but few show as much power. AID recognizes many separators. Letters , '' "" ; / | all separate words and concepts in rough degree of power. You would want to separate members of a list with , like in common grammar, but ; might be better for separating categories. () {} [] <> : are connectors. You can put lists inside all of them or use : to create a logical implication. For example it might be preferred to do a list like `MENTAL:[happy, go-lucky, energetic];` and define logical connections like AGE(25y) or sword(polished) the AI usually recognizes that the thing you're describing is 25 years old or that the sword is polished. Enclosures can be interchanged, but these are two commonly seen and powerful methods.
ALL OF THE ABOVE IS RELATIVE AND CAN BE CHANGED IF YOU TRAIN YOUR STORY BY USING THESE CHARACTERS DIFFERENTLY IN YOUR WI


## World Info Formatting
The section most users will be most interested in. For the longest time users of AID were writing WIs in pure prose. It helped things, but things remained inconsistent especially with how fickle and air-headed Griffin tends to be. Users started researching using different letters and sentence structures. Some people even tried coding inside WI or getting AID to code for them. After some time, users shared their results and many came to the shared N=X conclusion that deliberate `world info formatting` leads to more desirable outcomes. These became known as `world info formats`. Some goals of structuring WI include:

+ Reducing randomness (or later adapted into a RNG generator)
+ Defining characters, locations, lore AND getting the AI to remember and mention these things
+ Describing those things consistently and repeatedly
+ Staying on topic: more magic and less guns in fantasy worlds, no magic and more scientific guns in sci-fi worlds
+ Preventing leakage - the AI picks up on patterns and WI can leak into each other (especially in Griffin)
+ Consistency both in how the AI receives data and how the user writes their WI (the AI loves a degree of consistency, consistent WI are easier to write)
+ Formats can be mixed and matched for different tasks; characters, new verbs/actions, locales and regions might each have an optimal format that better suits the user's needs
+ Breakdown: sloppy WI can lead to the AI 'losing focus' faster and forgetting about what you're trying to tell it with WI - this occurs with regular stories too, faster

One of the first formats commonly shared online was what the author calls the `Ben format` popularized by a user curious_nekomimi in August 2020. Every newline would have the key of the WI usually a character and every sentence would be followed by a newline. This raised public awareness of reinforcing the AI with WI. A simple prose format like this can get the AI to be mindful of every attribute assigned to said character or object. But it tends to be heavy on used-up characters and especially Griffin might start mixing up actors from different WI together.

Here it bears mentioning that many users want to define `You` inside WI. There are caveats to this, especially in a heavier format like the above one. If you use `you` as a key this WI would likely fire every turn and make the AI focus strongly on things mentioned in this WI as well as constantly eating up space. This poses the risk of burying other WI. A potential alternative is using a key in the form of `AKA: You` alongside the name of the character you play as. Users should make personal decisions on what they value of their WI given the data we have on GPT-3, AID and WI experiments.

Interest in WI formats blew up after people started sharing a version of JSON-based formatting based on Discord user Zaltys 🐍#5362 experimentation. Here we define `the pure JSON format`:
```
keys: AKA: YOU, Anon
Entry: [{"You":{"name":"Anon","age":24,"gender":"male","species":["human","man"],"eyes":{"eye-color":"brown"},"hair":{"hair-color":"dark brown","hair-length":"intermediate","hair-style":"spiky"},"relationships":{"orphan":"no relatives"},"body":{"physique":"average","height":"average-178cm","weight":"average-80kg"},"personality":["sarcastic","friendly","wishes he had a gf","plays AI dungeon"]}}]
```
+ Super consistent, the AI understands it super well and sticks to it like a hawk
+ """Simple"""
+ No reports of leaking
+ Has stood the test of time even at times when the AI was unstable 
+ If you know how to write JSON you can describe more complex relationships and have the AI use them
+ Is starting to get early writing tools like https://aid-world-builder.ey.r.appspot.com
+ Painful to write and spell-check, use a linter/editor like jsonmate.com and validate with jsonlint.com or use the above tool
+ Consumes surprisingly many tokens, space wasted on "" and other symbols
+ Motivated research into other formats because it's good but has caveats
+ You're best off writing 'uglified' json because 'beautified' json has no benefits, spends more space and looks weird when you input it to WI

Zaltys 🐍#5362 has developed many interesting formats and motivated the community to test different keywords in WI. Here's an earlier example of his kobold that worked well.
```
keys: Deekin, Scalesinger, Deekin Scalesinger
Deekin Scalesinger:[Kobold/♂/<72cm>;ORIGIN<Deekin>:Ally from Neverwinter Nights games(CRPGs);APPEAR:Reptilian/orange-scaled/horns(nubs)/snout/legs(2)/eyes(brown/2)/arms(2)/tail(scaly)/wings(2/red/scaly);LACK:hair/ears;WORN:Bard clothes;MENTAL:Pessimist/creative/polite/kind/friendly/self-conscious;TRAITS<Deekin>:Musician/poet/bard/writer/raspy voice/hero/sorcerer/plays lute/beat Mephistopheles/bard-magic/former servant(of dragon:Tymofarrar)/"Yes, Deekin very kobold, last Deekin look in mirror."]
```
Here we see many adaptations seen today. Various enclosures, removal of whitespace and other excess symbols. Having `."]` close to the end. Experiments from Zaltys and friends has shown that Griffin works the best if the character `.` is used close to the end of the WI. 

Many users enjoyed experimenting with Deekin but Zaltys has since refined his format. The following is more recent Zaltys with the the character Mike Haggar.
```
keys: Mike, Haggar, Mike Haggar
Mike Haggar:[Human/male/202cm/140kg. APPEARANCE<Haggar>:Stocky, muscular, hairy arms, hair(brown-hue), mustache(brown-hue). WORN:Eyeglasses, pants(green-color). TRAITS<Haggar>:Born in 1943, from Final Fight and Street Fighter games, former pro-wrestler, grew up on streets(of Metro City), Mayor of Metro City, just, incorruptible, fought Mad Gear gang & Skull Cross gang, fights gangs, "It's my job to keep Metro City safe!", hands-on, likes curry rice, friends(Cody, Guy), daughter(Jessica).]
```
<Haggar> is used to remind and reinforce the AI all the data has to do with Haggar. () is reserved for associations, but can hold a list of details. Longer sentences do work well in simple encapsulation like this. Phrases inside quotations seem to inform the AI how the character speaks. Works as good as pure-JSON with most keywords the AI likes. 

Based on experiments in these and similar formats, the community has found all-caps categories are good. APPEAR/BODY, WORN, MENTAL and TRAITS cover the main bases for most characters. LOOKS can also be used to replace APPEAR, especially if you're defining an object. SPEECH and "" can be used to reinforce speaking patterns, albeit with some inconsistency. `mute` in traits also works if going for a mute character. LACKS can be used to denote a thing the character doesn't have. LIKES/DISLIKES are good. Later on in the document we include a long list of CATEGORIES. Zaltys likes to write a lot of species from different series. He has written on discord that his most used categories are APPEAR, LACK, GRAB, MOV, MENTAL and TRAITS. For defining a type of character all these have purpose. Appear is self explanatory. Lack has to do what the characters can and can't do - a lamia without legs shouldn't be able to walk. Grab is their preferred way of interacting through appendages, a bird would use it's beak or talons, the lamia would use their hands or tail. Mov works for movement. Lamia would crawl/slither on top of their tails, slimes ooze other characters may do more imaginative things. Mental is traits of the mind, traits is more intrinsic physical or sensory traits.

Latest Zaltys.
```
Aarakocra:[Avian/150cm/42kg;APPEAR<Aarakocra>:Slim/feather-covered(brown-hue or white-hue/♂:colorful)/feathered head/2-eyes(keen)/tail-feathers/2-wings(big wingspan)/digitigrade legs(2/bird-feet/talons)/2-hands(clawed/5-fingers)/hooked beak(sharp);LACK<Aarakocra>: hair/ears/arms/breasts;MENTAL:Wise/honest/calm/benevolent;TRAITS<Aarakocra>:Prefer flying/lowtech tribal/hunters/scouts/adventurers(rare/rangers/druids)/no ownership/weapons(thrown)/live in mountains(isolationist)/claustrophobic.]
```

Next we take a detour to kimtaengsshi's testing before going into formats borne of experimentation. Results like these are a big motivator for being so particular with WI. He tested a bunch of WI-formats from the discord and analyzed their tokens to characters ratio with the following results:


Full prose (standard): 514 chars, 118 tokens = 4.36 ch/tk

Pure JSON: 512 chars (-2), 172 tokens (+54) = 2.98 ch/tk

Pure JSON (w/o spacing): 475 chars (-51), 138 tokens (+16) = 3.44 ch/tk

Zaltys-Basic: 393 chars (-121), tokens 122 (+4) = 3.22 ch/tk

Zaltys-Advanced: 358 chars (-156), 128 tokens (+10) = 2.8 ch/tk

Monky-Modern: 352 chars (-162), 123 tokens (+3) = 2.86 ch/tk

Monky-Caveman: 362 chars (-152), 87 tokens (-31) = 4.16 ch/tk

Pseudo-JSON-kim (no parentheses): 402 chars, 135 tokens = 2.98 ch/tk

The goal here is optimization. Full prose has the best character-ratio, but has all the caveats AID is known for. Many users love the output pure json gives, but it has some of the worst ratios and character usage. Zaltys results aren't much better although this can be attributed to heavy symbol and possibly unicode use. Kim's own minimized format is similar to Deekin without long sentences, but had similar ratio problems. At the bottom we see something interesting. A format called caveman that's far shorter than prose with similar character-ratios.

Monky's solution to optimizing is what he calls the `caveman` format. A cursory look at an example will show us why:
```
keys:Chagra, initiate, apprentice
Chagra AKA 'Tusk': Orc she 20y.
Chagra 220cm ht. 100kg hardy.
Chagra green scar skin. Long brown hair.
Chagra loyal careful outcast pranked Healing magic initiate coven.
Chagra Hergea's daughter Friends You Renni.
Chagra likes fruit veggies Hates meat cheese.
```
In this WI all fluff words have been removed. Still, testing has AI mention all the relevant details with very low risk of leaking into other characters. It is likely that the brutal efficiency of AI doesn't care about words like `is`. Users of the format feel that it sticks to the given information better than the base AI alone or untested prose. And so far no one has reported that the AI starts speaking caveman when using this. You should probably use highly descriptive inputs or an A/N: for making the AI write descriptive prose though.
There are two good resources for this, Monky's test scenario and kim's caveman generator, both linked in order:
https://play.aidungeon.io/main/scenarioView?publicId=558a8d40-2fbd-11eb-9239-8b8f17f7a2b0
https://play.aidungeon.io/main/scenarioView?publicId=3f094990-2e40-11eb-b81d-a3d32aaa3e7a

During experimentation on caveman Monky made important observations. When he used the caveman format extensively on Griffin he found if he didn't mention the character/key every 50 chars/15 tokens the AI would 'lose focus' and start describing random attributes outside of the WI. On Dragon the limit before he needed to start mentioning the key again was around 200 characters. Discoveries like these have led some users to believe the key should be reinforced at these intervals DEPENDING ON WHICH MODEL YOU USE. 

TL;DR A maxed out Griffin WI should be ~10 50-character lines long, each line starting with the key. On Dragon this could be 2-3 lines each maximum 200 characters. 

Hunter Rodrigez, Lazy and other users have been working with shortened formats motivated by JSON. Originally parentheses were removed perceived as unnecessary. Early tests suggest it works just as well as proper JSON, bearing in mind the tricks discussed above. The author would recommend pursuing variants of this format for a good balance between accurate details and token use. Originally people called this pseudo-JSON, but more accurately it is pseudo-prose. Token testing shows commas and brackets work as contextual linkers just like in regular grammar.
```
keys: Thea Smith
[{name:Thea Smith,age:25y/o,♀,BODY:[toned,muscular,athletic],skin:[tanned],HEIGHT:[tall],BREASTS:[medium],HAIR:[Purple,very short,mohawk],EYES:[glowing,blue,heavy eyeliner],PERSONALITY:[calls you shorty,Punk,adventures,kind,rebellious],LIKES:[dancing,music,working out],relationship:{Tess:[little sister],Finn:[little brother],Beth:[Mother,Mom],Bob:[father,Dad]}}].
```

`NOTES ON X` works as a WI format. As with all pseudo-prose formats it could be cavemanned. It's an effective and easy way to define fields of study like `alchemy`. While it works as a WI-format some users have reported leaking with it and it seems more useful/interesting as a test input for the AI.
```
keys: John Edgarton
NOTES ON JOHN EDGARTON: current prime minister, stressed, morose, but ethical, keeping appearances for morale. Black bowler, tan overcoat, dirty shoes. Bald, large tummy, hairy mole on broad nose, brown sleepy eyes, sallow skin. Widower, has daughter (worries about her). Touches head when nervous (subconsciously). Paces constantly.
```

Onyx has also come up with interesting and reliable tricks. With RND we can actually have pseudo-random lists. Testing so far suggests with 10 members the list is completely consistent, more members or simple words make it output more random, but still conceptually related things.
```
keys: monsters, forest
Many monsters live in the forest. [RND (monsters): slime, goblin, orc, Ent, giant carnivorous sloth, pixie, beholder.]
OR
keys: goal, objective, quest
RND (objectives): slay the dragon, rescue the princess, challenge the demon lord
```

Another one is `IF (action): THEN (things happen)` this is especially useful for creating custom spells or other conditional effects like the format suggests. 
```
keys: cursed book, (OR) open cursed book
IF (open cursed book): THEN (the pages of the cursed book form into a face, shriek loudly and place a terrible curse on you)
```

From his work we have a reliable and easy way to define locales especially when combined with data mentioned above. These get interesting when combined with other short WI.
```
keys: Rask
Rask (kingdom): TERRAIN: tundra (full of ogres and giants), fertile land (southern). FEATURES: Wolfsholme (capital, walled city), primitive gunpowder weapons. CLIMATE: cold, snowy. RESOURCES: livestock, horses, furs, lumber. CITIZENS: humans, elves, giants, hardy, fierce, wield warhammers and axes, worship Nahr. RULER: Queen Cassandra.
```
From recent experiments the community has discovered that defining locales like this may benefit from placing each category on a newline. Both methods are effective. 

Zaltys has also made discoveries on how to reliably define the structure of a building. Sometimes the AI is finicky. As a category EXIT works better than EXITS. Directions are good categories, but specifically for architecture.
```
keys: library
Library room: large room. EXIT: corridor (north), lounge (west), trophy room (southeast). FEATURES: Bookshelves (dusty tomes, esoteric, mythology, grimoires), man-eating plant (huge), spiral staircase (to second floor), chandelier (creepy).
```

Many sources confirm that in all versions of the AI newlines (pressing enter) is powerful at grouping and separating certain traits. ALL CAPS is good for defining the group, encapsulation like () is good for the reference point. Thanks to monky caveman we know the AI doesn't care about grammatical correctness in the WI either. As of current version of this document `red-color` or `hair-color` for example do not seem to work as well anymore. Trying to fix this, the community discovered that AI groups words strongly together, yet knows to separate them if you smash them together into compound words even if it's against English grammar for example `verylonghair` or `platinumblondehair`. birb calls this smashing words. Combining these discoveries birb and Zaltys have had great results even on griffin with the following format (and it's subsequent update).

```
keys: Rick, angry wizard
[TITLE(Rick):The angriest wizard that ever lived.
RACE(Rick):human, wizard.
APPEAR(Rick):old, tall, lanky, wrinkly, mullethair, verylonghair, grayhair, graybeard, messybeard.
EYES(Rick):orangeeyes, flamingorangeeyes.
WORN(Rick):tatteredgrayrobes, pointywizardhat.
LACK(Rick):anyshoes.
MENTAL(Rick):furious, loud, irritated, unstable, complaining.
HOBBIES(Rick):shoutingatkids, breakingwands, misusingmagic.]
```
```
[Ritz[SPECIES:elf.]
Ritz[BODY:female, 24y, shortstack, soft.]
Ritz[EYES:aquamarine, sparkling.]
Ritz[HAIR:platinumblonde, long, ponytail, bangs.]
Ritz[WORN:white hooded druid robes, white lace garterbelt.]
Ritz[MENTAL:soothingsmile, clumsy, knowledgeable, airheaded, bashful.]
Ritz[TRAITS:easily embarrassed, innocent, druid, stutteryspeech.]]
```

There are still caveats and unknowns to this format. Original testing was done without [] but they are added here to prevent WI leaking especially on Griffin. It may be possible to remove `.` from the end of each line.`Flamingorangeeyes` and `tatteredgrayrobes` produced consistently desired results, but there is ALWAYS a risk of the AI misinterpreting the words depending on tokenization when you smash words together. For example `catears` gets tokenized as ca|tear|s. Smashed words may be used with any format even Caveman (potentially leading to the greatest character and token savings). If this becomes a thing please keep making references to Rick, angriest wizards or smashing words.


## Useful Testing Prompt
The following is the testing scenario prompt birb has had some of his best WI testing results on. Before you reference the WI, you have to produce at least two outputs from the AI so you get GPT-3 results. You may alter these outputs for higher quality writing. Please do modify and form your own version of it. The trick is removing 'you' the main character from the story. God is only used as an easy stand-in. 
```
You are a God. You are omniscient and omnipotent. You don't interact with the world you have created, but you love watching it from afar. You use a crystal ball to view different characters and events unfold in the world below. As usual, you spend your time viewing your creation. You gaze through your crystal ball.
```
It's also particularly entertaining to make the AI generate inputs for many WI this way using the format `you created X this way because (Y reasons)`. Do bear in mind this is repetitive. 


## Useful Categories
With methods like JSON, pseudo-JSON, Zaltys and its relatives we've noticed a capitalized `CATEGORY:` followed by a list of attributes is very effective and commonly used among different formats. Here we share some categories that have proven very effective. The useful words will be provided as a list as their use is self-explanatory. We recommend experimentation, but these words have been chosen because they felt more effective than their synonyms. There's also some peculiarity here: the AI seems to prefer plural s to the 'do-s'. APPEAR is better than APPEARS and LACK is better than LACKS. But HOBBIES and POWERS are as good if not better than their singular forms.

BODY, APPEAR (APPE), LOOKS, WORN (STYLE/FASHION), MENTAL, LIKES, DISLIKES, HATE, LACK, RELATIONS, FRIENDS, ENEMIES, TALENTS, HOBBIES, POWERS, THEME

Not all details need their own category. RACE, GENDER, AGE can be condensed to `elf/female/25y` then followed by the bigger CATEGORIES. The AI picks up common traits just fine by their lonesome. TRAITS is a catch-all for when you don't need a big list on a single topic.

The AI understands the concept of speech patterns and accent to some degree. There is no consistent method of activating it yet, but `SPEECH:` is one working category. `WORDING:` seems to work too. Writing style theme in author's notes can also give characters accent. Known working accents: pirates, sailors, Shakespearean accent, archaic, Cockney accent, valley girl (valley girl is the best method for getting a Southern accent on both sexes).


## Current Recommendation
Here I try to suggest what WI format is most stable recently. This section is subject to heavy and frequent changes. The community is still experimenting with all of the above data. Most recent Zaltys which can always be found on the official AID Discord is considered stable. birb's Ritz example also provides very stable results. Caveman is especially good for published scenarios where players may want to add their own WI. Recent research on JSON shows it may be a red herring - the AI forms outputs based on grammatical associations. Meaning key immediately followed by category and traits is optimal for any format. JSON doesn't do anything 'different' to produce better results. Continue writing JSON if you like it, but don't feel compelled to invest in it. Alternatively, use the online worldbuilder to automatically output Zaltys or JSON for you. The dev is consistently pushing new features and taking feedback/requests on his project too.


## Other scenarios and scripts
Since this document shares some scenarios for testing purposes more bear mentioning. This scenario uses a modified Zaltys-like format. You give it names of characters or even concepts and test how the AI model generates information for it. This can be used to check how well the AI understands interesting words by it's lonesome. While designed for characters you can use it to have the AI describe ANY word. If this does not meet your needs, you can always use this as a base and create your own scenario with your own format.
https://play.aidungeon.io/main/scenarioView?publicId=6a344070-2a58-11eb-9973-ef0c0f08ab62

Scripts fall outside of the scope of this document as they can only be inserted into scenarios by premium users at the time of writing. Creating or editing a script also requires knowledge on coding. A user named Zynj is working on some of the biggest scripts that may help WI-users. Below is his public git. If you go on the official Discord you can also find his JSONEWI script that is designed to make WI more convenient and powerful to use.
https://github.com/Zynj-git/AIDungeon


## Special Inputs | Commands
What are world infos if you can't utilize them? Here are some interesting and recommended `inputs` discovered by the community. A lot of them work in conjunction with objects you've defined into WI. If an input has a field surrounded by <> you don't need to type those container characters into AID. `Author's Note:` can also be used as an input to have the AI generate something interesting for you.
List of useful A/N: words, link may go down in the future https://justpaste.it/9ofj1

- Name:"Text"
- List of X:
- POV [character]: or [character]'s POV:
- 2nd Person POV:
- Your POV:
- Description of x:
- X described by Y:
- Current scene as described by [author]/[writing style]
- [character]'s secrets:
- If only [character] knew
- Available X scenes for [character]:
- [character]'s recollection of this scene X years later, [year]
- [character]'s opinion on this story
- current scene as a X
- [character]'s thoughts (on/about subject)
- [character]'s X thoughts
- [character]'s Personality
- Open X.file
- Your actions:
- You write a poem about X:
- X, a Y poem:
- Poetry written by X:
- X performing on open-stage (narrative):
- [character]'s psychological profile:
- [character]'s Brain Root Directory/Folder:
- Your sins:
- List of story actions:
- Description of [new character]:
- You take a look at the form, it reads:
- X gives a lengthy monologue about Y
- Advice from the voices in your head:
- Rational:
- Hours later...
- You decide to view the bulletin board, where you see a number of requests.
- Audience poll results:
- You think to yourself,
- You look around and take in the scene.
- Debug mode activated. List of X available cheats:
- Credits:
- Epilogue:
- In loving memory of:
- Score:
- Deaths:
- Inventory:
- Items obtained:
- Imagine the smell:
- Summary:
- Achievement unlocked:
- Sponsored By:
- Notes on X:
- The moral of the story is
- This scene had the following effects on you for the following reasons:
- Detailed/Verbose/Poetic Description ():
- P.S./P.S.A.

You will come to see following the advice of this document that () and : are really useful characters when utilized correctly.


## Further Discussion
As mentioned before AID is in constant development. Recently WI testing was disturbed, because updates to safe mode and tackling a long-standing caching bug caused issues with the AI especially on Griffin. As of current revision the dev team says this particular bug is now confined to server side and shouldn't affect new games the same way. Regardless, in the future situations might present where development makes experimentation slower. Make sure to export and catalog your WI so you can test them out when the AI feels stable enough to your liking.

CREDITS: Everyone whose name has been mentioned here (you guys rock) and birb for compiling this. Everyone who shares their experiments and results to the general public.
4chan has threads and guides too. Go there if you want their guides and sources, some of it is good especially for NSFW. The special inputs section especially benefited from their work. Happy Holidays.
All this information is free, community provided and now AGPLv3 licensed due to being hosted on github.