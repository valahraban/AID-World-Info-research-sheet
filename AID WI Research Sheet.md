# AI Dungeon World Info Research and Reference
- [AI Dungeon World Info Research and Reference](#ai-dungeon-world-info-research-and-reference)
  * [Preamble](#preamble)
  * [Recommendations or the TLDR](#recommendations-or-the-tldr)
  * [Remember WorldInfo AuthorsNotes Worlds and how the game handles them](#remember-worldinfo-authorsnotes-worlds-and-how-the-game-handles-them)
  * [Griffin vs Dragon](#griffin-vs-dragon)
  * [Tokenization - understanding limitations and special characters](#tokenization---understanding-limitations-and-special-characters)
  * [On certain characters](#on-certain-characters)
  * [Authors Notes specifics](#authors-notes-specifics)
  * [World Info and Formats](#world-info-and-formats)
    + [JSON Format](#json-format)
    + [Zaltys Formats](#zaltys-formats)
    + [Useful Categories](#useful-categories)
    + [Monky Formats](#monky-formats)
    + [Onyx Formats](#onyx-formats)
    + [birb research](#birb-research)
    + [Misc Tips](#misc-tips)
    + [character-to-token ratio tests](#character-to-token-ratio-tests)
    + [Worlds Specific Tips](#worlds-specific-tips)
  * [Other Scenarios and Scripts](#other-scenarios-and-scripts)
  * [Obsoleted Methods](#obsoleted-methods)
  * [Special Inputs Commands](#special-inputs-commands)
  * [Ending Notes](#ending-notes)


## Preamble
If you're using a text editor turn on word wrap to read this document. Perhaps the greatest addition to the AI Dungeon for generating high quality stories has been world info. Learning and figuring out how to use world info effectively is of great interest to many users: using it one can get vastly more consistent output from the AI and define terms that the pre-trained AI might otherwise have difficulty understanding. Features like `Remember` (pins) and `A/N:` (author's notes) will also be covered as they are heavily related and connected to world info. Everything here is of use to both free and premium users. The differences between griffin and dragon will be discussed as there are some differences one should be mindful of. Good world info can make griffin almost as good as dragon, but requires more attention and effort from the user. While world info is a powerful tool there are also caveats and concerns to using it effectively. With good world info the user can create any kind of scenario and a `world` of their own. 

A friendly reminder: This document isn't authoritative and doesn't pretend to be so in any section. It is a compilation of the community's collective and shared experience, primarily through the official AID Discord and burrowing publicly available material from other fan-run communities. Originally it was the author's personal notes and should be treated as such. This means the evidence and advice is mostly anecdotal. Citations are shared with permission from the users. Most of them can be found as messages on Discord. Some links may be provided, but please understand imagehosting is unreliable. Attempts have been made to explain which practices show the most promise. Everything has been repeatedly tested inside AID scenarios and the tools provided to us by developers and scripters. You will have to take the author's word on it for the peer-reviewing process. Outputs will mostly be skipped for the sake of brevity as they can be tested inside AID. 

The start of the New Year has been a time of rapid changes for the AI and Latitude's business model. There have been many changes that have affected formatting and scripting is now a free feature. In light of this, this document repository is looking into overhauls and changes. Materials from other authors and their work will be provided with permission. A new section is included where we discuss past methods that no longer work as well as they used to.

Special credit to kimtaengsshi for help with editorship and fact checking the technical details. This document will contain data kim has confirmed through monitoring LMI (Last Model Input) and other techniques which the author finds trustworthy. Much of this information comes from and is shared with the community-run wiki and Discord. A link to the wiki will be provided where you can also find an invite to the Discord and other useful content: https://wiki.aidiscord.cc/wiki/Main_Page


## Recommendations or the TLDR
**As of 07.01.2021 Scripting is free for all users, it is very recommended you scroll down to Zynj's github, check out EWIJSON and learn how to use it. Loading WI through these scripts alone makes it work more like it should.** This document will focus on scripting-free methods. At a later date a scripting-focused document will be provided.

For those of you who came here for the WI formats or have short attention spans. Click on the links contained in the table of contents above or hit CTRL+F to search for names that interest you. **Pure-JSON** is neither recommended or discouraged. If you're comfortable using it continue doing so. The **Zaltys Formats** of this document are recommended as a strong baseline. Zaltys formats are easy, popular and frequently updated. Past samples are provided as well. **Monky Caveman Format** is recommended for people intending to publish scenarios, worried about token or memory use and wanting to share their work. It is very understandable, easy to modify and works well with regular prose. Very easy to combine with scripts too. Recently Monky has developed Neanderthal which is also recommended. **Onyx** has written formats and WI tricks for specific purposes such as defining locations and lists of things. Aspects from formats may be combined together and different WI may use different formats within the same scenario. You the writer have complete creative freedom, although our experiences may benefit you.

For comedic purposes here is what one very opinionated user has to say on the current state of formats:
```
From CrisAIcilian: Zaltys is better in every way, it's actually designed for its purpose. JSON is like Chinese medicine, goo of mumbo jumbo happens to contain three elements that had an effect (colon, comma, square brackets).
```


## Remember WorldInfo AuthorsNotes Worlds and how the game handles them
Remember was the first of these features implemented in the game. This segment goes into technical details so you may want to skip it if you're here for World Info formats, but it is very important if you want to understand AID as a tool and it's limits. Grossly simplified remember is a world info that gets constantly fed to the game after your every input. World info is like remember, but has user-defined key-words `keys` that after getting triggered make the game use them as part of the context. Keys use string-matching which will be important later. Having this element of context makes world info less restricting to work with because when you're happy with it you can forget and leave it while things in remember require more frequent attention and updates as the story progresses. History means old prompts written by you and the AI. The order the AI reads your input is in:

+ Summary										| Experimental feature to help devs
+ WI
+ Remember
+ History
+ AN if you've exactly 3 newlines in history
+ Rest of history
+ Fresh Input
+ Front memory from scripting					| Advanced/premium users

The following data is based on information available to scripters or based on their limit testing, documented on discord. All of the above consumes characters or as we will soon discuss tokens from the AI's `memory`. Anything that goes over the `memory` limitations will not be read by GPT-3 and is therefore irrelevant to the output process. The entirety of the data(text) that gets fed to GPT-3 is called the `context`. The following is a list of rules the `context` follows as related to the many parts that form it:
```
- Max. input character length: 2772
- Max. Summary length: 500
- If (Summary + WI + Remember) < 50%, then extra story aka history will fill up the empty space
- If (Story + A/N + Input + frontMemory) < 50%, the unused space will not be allocated to (Summary + WI + Remember)
```
For brevity the maximum context length is often shortened as 2.8k. In regular use a single WI has a limit of 500 characters and remember has a limit of 1000 ch. On the plus side thus far no limits have been discovered on *how many* WI one can define. It's not recommended, but it is possible to define multiple WI with the same keys, essentially increasing the space by 500 ch on every iteration. Keep in mind AID always adds a newline between each WI even those using the sake key. AID is smart about using WI, but not about remember and updating remember constantly is tiresome therefore many advanced users focus on producing high quality WI while foregoing longer remember pins. The limitations of GPT-3 and AID is why understanding and researching WI is of interest. Worlds will be discussed later as they use the same data structures as regular AID.

With scripting it's possible to circumvent the regular ordering of each element. This is a topic that the user should keep in mind if they are interested in scripting and the potential benefits.


## Griffin vs Dragon
AID contains multiple machine learning models the primary ones being Griffin and Dragon with Lovecraft being a later seasonal addition. Modern versions of AID Griffin and Dragon are both GPT-3. This has been confirmed by the devs on twitter, discord and other public forums such as their medium. That being said there are different sizes of GPT-3. Griffin uses a subset of GPT-3 that is closer to the original 1.542 billion parameters used by the GPT-2. The full-sized GPT-3 that Dragon is based on has a whopping 175 billion parameters. This shows in the quality, consistency and most importantly *forgiveness* of the model and the output it produces. Both models love a detailed and consistent context, but Dragon is more forgiving in terms of formatting. This shows especially in how differently it responds to WI you feed it. Griffin has quirks you have to be mindful of. These affect what kind of WI you write and will be discussed in the following sections.

It used to be that the first prompt and response were generated in *GPT-2* but this is no longer the case, it depends on what model you use. If there is a populated prompt, the first output will be in whatever model you're using. Only a completely empty prompt would be GPT-2. So at minimum put something in your prompts. This is official information from Alan. Both detailed history and WI are important for training the AI for your scenario and getting what you want out of it. Either redo until the writing looks high quality enough to your liking or spice it up with attentive alters, the key to enjoying AID is building on success.


## Tokenization - understanding limitations and special characters
The character limitations mentioned in the previous section are ones imposed by the devs of AID. They are quite sensible, plenty for generating good stories, not bankrupting Latitude and not horribly far off from the true limitations of GPT-3 that both models now run on. For a machine learning transformer to read an input it must first be tokenized into a form the model understands. GPT-3 has a hard limit of 2048 tokens. It won't read more at once. Devs have access to 2048 tokens, the users are given 1024 per input. But as you've noticed, you have more words to work with that the AI clearly understands. Tokens are just the way data is encoded into the machine learning model; during training, common words became recognized and encoded into an efficient form, sometimes even a large word gets recognized as 1 token. From kim's research, depending on how a world info is defined, the counts average to 2-4 characters per token meaning you get to use at least double the characters of the tokens you really have hence why a commonly mentioned character limit is 2048. 

A few things have been learned from experience; common words and phrases like `hello` or `25y` get treated as a single token. White spaces are special because they often get rolled into words as is grammatically correct. An uncommon word like ` Enterprise` takes 1 token with the white space while `Enterprise` takes 2 tokens. Unicode emoji like `😃` and some special symbols are expensive, they cost 2 tokens per character. As we will discuss characters like `<>` `[]` are useful for WI, but tend to have a cost ratio of 1 token. There are also rare characters that the AI hates like `ù`. Experiments have shown that longer strings of symbols like `}}}` get rolled into 1 token.

If you want to test out and learn more about GPT-2/3 tokens (they work the same) visit this link: https://colab.research.google.com/drive/1i1wXfc4SWDprHZuLTaMsgWPv6YwvlQ_I?usp=sharing#scrollTo=mlBq9zO67b2n

kim has also made a tokenizer scenario/script you can use inside AID: https://play.aidungeon.io/main/scenarioView?publicId=6927d610-34b7-11eb-b8e1-c185d026672d

Any section of this document discussing tokenization has been verified with either of the above tools.


## On certain characters
Functionalities of certain characters have been found and described thanks to the development of `world info formats` but it's useful to discuss these beforehand. Their functionality can be somewhat verified through the tokenizer Colab. Some characters connect. The most important of these is `-`. `hair-color` or `red-color` are words that get better results for colorization than other methods (may depend on format and order of traits). `-hue` doesn't work as well but may be used. Other characters have been tried for connecting words, but few show as much power in scenario testing. As a matter of fact often you can string random words together which will be discussed later. AID recognizes many separators. Letters , '' "" ; / | all separate words and concepts in rough degree of power. You would want to separate members of a list with , like in common grammar, but ; might be better for separating categories. Inside brackets `/` is useful for keeping the traits related to what they are describing. () {} [] <> : are connectors. You can put lists inside all of them or use `:` to create a logical implication. For example it might be preferred to do a list like `MENTAL:[happy, go-lucky, energetic];` and define logical connections like `AGE(25y)` or `sword(polished)` the AI usually recognizes that the thing you're describing is roughly 25 years old or that the sword is polished. Enclosures can be interchanged, but these are two commonly seen and powerful methods.

The above used to be the case. Whenever OpenAI or Latitude updates their back-end models, the significance of a character may change. As of January 2021 `-` works more like the mathematical minus sign. An example is provided in the document of doing pseudo-math inside AID. It is preferable to mashup words that are normally separated by `-` together inside the WI entry.

< and > are special characters. You may have noticed in do or say mode the AI always prefaces your inputs in the story with `>`. In AID the character is reserved for this purpose. You may have noticed that the AI *never* outputs < or > on its own. Since the information inside WI is handled differently we can still use any unicode letter inside it. In WI this can be used to our benefit as we will see when we discuss the Zaltys format.

The GPT is an autoregressive transformer language model trained on English writing. Any particularities AID are have due to unique parameters set by the dev or habits learned from big datasets like fanfiction.net. Characters like ! and ? aren't useful or interesting. They are only useful in conveying meanings they have within natural conversational English where they most often are employed in prose. The GPT isn't magic and it wasn't trained on programming languages. The fascinating thing is algorithms and linguistic expression have many things in common by happenstance.


## Authors Notes specifics
Let's discuss the premium feature `A/N:` first. If you're looking to inject flavor into the AI's outputs A/N will be your first and probably most efficient stop. It is a worthwhile for the convenience, yet it's usefulness is not entirely restricted to premium users with our advice. What A/N does is  it insert a line of text into the history the AI reads e.g. `Author's Note: Some details like mood about this story.` exactly 3 newlines up. On the scenario screen it has a 150 character limit, but the in-built maximum limit is 300 characters according to kim's LMI testing and community experiences.

Discovering manual Author's Note inputs during dev testing and how heavily it affected output is why it was added to the core game. Even as a free user you can manually add Author's Notes into your story. With manual A/Ns you have more characters to work with, but it is doubtful that making a super long A/N: offers any extra benefit. Editor's Note is another interesting take on the concept by user gnurro. Research suggests that A/N: can be used to reinforce writing style even of well-known authors through repetition. It can also be used to change perspectives and the `focus` of the storytelling. These findings suggest that the prompt informs the AI what kind of writing to expect directly after A/N: has been invoked. Exemplified below is a medium-length A/N: with reinforcing that has been confirmed to be capable of producing outputs where characters break the 4th wall:
```
[Author's note: This novel is esoteric and descriptive. Author: Terry Pratchett. Writing style:  Metafiction, in the style of Deadpool. Genre: Witty, talkative.]
```
The film rating system (not others) has been confirmed working. With `RATING: G` in A/N you can noticeably lower the amount of NSFW in your scenarios. Alternatively `RATING: R` makes things more racy. `X-RATED` works too with predictable results. As with most things this is more effective on Dragon.

Switching focus is easy, but may require extra use of alter afterwards to make sure the AI understands what you want. This style of A/N: is best used manually written into the story (although you can change your global A/N whenever you want; a trick we recommend using).
`[Author's note: The story now follows Mary's perspective.]` or `[Author's note: We now switch focus on Mary.]`
The main character can also be defined in A/N vs memory `A/N: Main character: <species>, <gender>, <job>`

A/N likes both style-describing nouns and adjectives. `strikingly elegant` makes the AI output more elegant prose than just `elegant`. This may also work for other words predicated with strikingly. `THEME:` is a word that can get the AI to focus on concepts and their attributes such as pirates or ninjas.
An example of output generated with `strikingly elegant style`:
```
You look around.
The sun softly kisses your exposed skin with her rays. You gently bask in her glory, raising your face towards her. Below, the plants softly sway in a gentle breeze as though worshipping her as well.
```
Short list of useful A/N words: AUTHOR, WRITING STYLE (Writing style:grandiose), GENRE, THEME, STORYLINE

A/N understands JSON too. Its entirely possible to do something like `A\N: [{"writing style":["descriptive", "elegant", "gritty"], "wording":["archaic", "Cockney accent"], "current state":"indoors"}]`

Some users have been experimenting with A/N for doctoring output and specifically managing randomness. Lower randomness is more consistent and predictable as it makes the AI's predictions more deterministic, but this comes at the expense of it feeling 'more dumb', hence higher randomness being desirable for verbosity. According to CrisAIcilian `This novel has no plot twists, it follows a linear storyline.` is a very effective phrase to include in A/N to increase consistency. This way his stories can stay on track even at 1.3 randomness. He finds that `Writing style:` and `Genre:` are useful categories to use most of the time. Writing style improves writing, genre works like a mini-prompt. More useful notes on writing A/N from Cris:
```
"Author:" is added when you want to do a convincing real author (combined with "Writing style: literary, author-name.) "Setting:" "Theme:" "Subject:" have their uses, depends on the content. "Title:" works at least on LOTR and "gone with the wind". Didn't test other titles. Famous ones should work. Has to be a real title, "star wars" is not a title, it's a movie franchise. The writing styles, most don't work on Griffin.
Only the descriptive-branch works on Griffin. "descriptive" works, "narrative" and another one I can't remember (according to Zalty) also works on Griffin.
```
Reminder: while less effective on Griffin anyone can benefit from using Author's Notes properly. With scripting, any user can mimic the functionality of Author's or Editor's Notes or create their own variant. A helpful user is experimenting with Author's Notes words all the time and has provided a resource that's being updated frequently: https://justpaste.it/9ofj1
 

## World Info and Formats
The section most users will be interested in. For the longest time users of AID were writing WI in pure prose. It helped things, but things remained inconsistent especially with how fickle Griffin tends to be. Users started researching using different letters and sentence structures. Some people even tried coding inside WI or getting AID to code for them. After some time, users shared their results and many came to the shared conclusion that deliberate `world info formatting` leads to more desirable outcomes. These became known as `world info formats`. Some goals of structuring WI include:

+ Reducing randomness (or later adapted into a RND generator)
+ Defining characters, locations, lore AND getting the AI to remember and mention these things
+ Describing those things consistently and repeatedly
+ Staying on topic: more magic and less guns in fantasy worlds, no magic and more scientific guns in sci-fi worlds
+ Preventing leakage - the AI picks up on patterns and WI can leak (especially in Griffin)
+ Consistency both in how the AI receives data and how the user writes their WI (the AI learns from consistency, consistent WI are easier to write)
+ Formats can be mixed and matched for different tasks; characters, new verbs/actions, locales and regions might each have an optimal format that better suits the user's needs
+ Breakdown: sloppy WI and sloppy writing can lead to the AI 'losing focus' faster and forgetting about what you're trying to tell it with WI (occurs faster in regular stories)

One of the first formats commonly shared online was what the author calls the `Ben format` popularized on Reddit by a user curious_nekomimi in August 2020. Every newline would have the key of the WI usually a character and every sentence would be followed by a newline. This raised public awareness of reinforcing the AI with WI. A simple prose format like this can get the AI to be mindful of most attributes assigned to said character or object. But it tends to be heavy on used-up characters and especially Griffin might start mixing up actors from different WI together.

Here it bears mentioning that many users want to define `you` inside WI. There are caveats to this, especially in a heavier format like the above one. If you use `you` as a key this WI would likely fire every turn and make the AI focus strongly on things mentioned in this WI as well as constantly eating up space. This poses the risk of burying other WI. Users should make personal decisions on what they value of their WI and scenarios.

Thus far the best method of handling `you` has been developed for use through scripts like JSONthing or EWIJSON. This script relies on defining objects. You can define keyword `you.character` with the entry `You (John)`. It also has a function `you._synonyms` that lets you define `(John|protagonist|player|hero)`, making you and John synonyms. This concept could be burrowed for script-free WI but as of now no standard way of defining you exists.


### JSON Format
WI can be exported and imported out of AID as JSON files. What we discuss here is something different. Interest in WI formats blew up after people started publicly sharing anecdotes of JSON-like WI based on Discord user Zaltys 🐍#5362 experimentation. Here we define `the pure JSON format`:
```
keys: AKA: you,Anon
Entry: [{"You":{"name":"Anon","age":24,"gender":"male","species":["human","man"],"eyes":{"eye-color":"brown"},"hair":{"hair-color":"dark brown","hair-length":"intermediate","hair-style":"spiky"},"relationships":{"orphan":"no relatives"},"body":{"physique":"average","height":"average-178cm","weight":"average-80kg"},"personality":["sarcastic","friendly","wishes he had a gf","plays AI dungeon"]}}]
```
The many things we have learned about using JSON inside WI can be summarized with the following list:
+ Super consistent, the AI understands it well and tends to stick on point with it
+ Simple syntax, making it easier to produce for non-native English speakers
+ No reports of leakage (on discord)
+ Worked even during the AI-issues that happened during the cache-bug fixes
+ If you know how to write JSON you can describe more complex relationships using the syntax and have the AI use them
+ Consumes many tokens, space/memory wasted on "" and other symbols
+ Motivated research into other formats because of it's usefulness and caveats
+ Painful to write and spell-check, use a linter/editor like jsonmate.com and validate with jsonlint.com or use a coding text editor
+ You're best off writing minified JSON because styled/linted JSON has no benefits, uses more memory space and looks weird when you paste it into WI or inside the proper JSON-file
+ In Discord community testing reduced JSON without quotation marks produced mostly similar quality output
+ Has online production tools like https://aid-world-builder.ey.r.appspot.com (thanks uusu)

JSON has its uses. If you're already using it feel free to continue doing so. If it looks frightening, don't feel obliged to invest learning it. The consensus of the author and the users he cites is other formats serve specific needs better plus we now have the online world builder. 


### Zaltys Formats
Zaltys 🐍#5362 has developed many interesting formats and motivated the community to test different keywords in WI. This document will be updated to reflect his latest and most recommended format variants with some explanation of his goals and motivations. First we have standard Zaltys that works with any model.
```
Mike Haggar:[Human<male, 202cm, 140kg>. APPEARANCE<Haggar>:Stocky, muscular, big arms, hair<brown>, mustache<brown>; MENTAL<Haggar>: Just, upright, direct, upbeat; WORN:Eyeglasses, pants:<green>; TRAITS<Haggar>:Born in 1943, from Final Fight and Street Fighter games, ex prowrestler, grew up on streets in Metro City, mayor of Metro City, fought Mad Gear & Skull Cross gangs, fights gangs, "It's my job to keep Metro City safe!", loves curry, friends:<Cody, Guy>, daughter:<Jessica>.]
```

+ This format must have a . near the end to work correctly. Do not use extra periods within categories, not even in quotes.
+  '<color>' is needed to make colors work. (Jan 2021 change.)
+ Avoid using -s. (Jan 2021 change.)
+ Categories must be typed in UPPER CAPS, with the exact spelling.
+ Including the name after the categories helps Griffin parse the entry.
+ Other useful categories include MOV (for movement-types, such as 'slithering'), GRAB (for object manipulation, such as 'talons', 'beak', or 'coiling'), ORIGIN (for birthplace, game of origin, etc).
+ If you need to add age, use age<num> in same section as weight/height.
+ All of it goes into single WI entry with no line breaks, under a keyword such as haggar.

We also have an alternate advanced format specifically designed for Griffin. It saves space inside the WI and context, but is both harder to read and write.
```
Mike Haggar:[Human<male/202cm/140kg>;APPE<Haggar>:Stocky&muscular/bigarms/brown<shorthair&mustache>;MENT<Haggar>:Just/direct/upbeat/hardy/upright;WORN:Eyeglasses<at work>/brownboots&greenpants/bandolier;TRA<Haggar>:Born 1943/from:<Final Fight&Street Fighter> games/ex:prowrestler/grew up on streets<in Metro City>/mayor<Metro City>/fought gangs:<Mad Gear&Skull Cross>/fights gangs/"It's my job to keep Metro City safe!"/loves:curry&rice/friends:<Cody&Guy&Carlos:from Brazil>/daughter:<Jessica>.]
```

Zaltys combines most WI writing trick the community has discovered into one very efficient package. One caveat would be how strictly you have to follow the pattern to guarantee functionality.

Based on experiments and accumulating experience the community has found all-caps categories are useful. Later on in the document we discuss categories in depth and include a long list of them that have been discovered to be useful for any WI format desiring to utilize them.

Zaltys likes to write a lot of species from different series. He has written on discord that his most used categories are APPEAR, LACK, GRAB, MOV, MENTAL and TRAITS. For defining a type of character all these have purpose. Appear is self explanatory. Lack has to do what the characters can and can't do; a lamia without legs shouldn't be able to walk. Grab is their preferred way of interacting through appendages, a bird would use it's beak or talons, a lamia would use their hands or tail. Mov works for movement. Lamia would crawl/slither on top of their tails, slimes ooze and other species may do more imaginative things. Mental is traits of the mind, traits is more intrinsic physical, sensory or nature related traits. Traits has proven to be the Swiss army knife of categories after months of use. INV and EQUIP work similarly to one another as alternatives to WORN and can be used to define the types of equipment a RPG character would carry on top of the usual stuff like clothes. WORN is better at handling style like `goth` clothing. `gothic` is more applicable to architecture. 

The AI is good at overwriting preset associations. On Zaltys format the community has succeeded in writing interesting recipes. For some reason the AI seems very good at describing recipes. This is another example of an indicator helping the AI understand WI better. Example from Mono:
```
Siren:[A popular drink;DESCRIPT<Siren>:Sapphire-colored/silvery bubbles/sprig of mint/smells alcoholic/tastes like rusty metal;EFFECTS<Siren>:Inspires drinker to dance.]

> You describe the siren to Severn.
"It's a drink," you say, "a sapphire-colored liquid with silvery bubbles that smells of alcohol and tastes like rusty metal."
"Sounds poisonous," says Severn.
"No. Not at all. It's delicious."
"You've had it before?" asks Severn.
You nod.

> You tell him about the side effects.
"It inspires people to dance," you say, "my sides hurt after I've had it."
"What else?" asks Severn.
"That's it," you say.
"That's all of it?" asks Severn, "no other effects?"
You shake your head.
```


### Useful Categories
With methods like JSON, pseudo-JSON, Zaltys and its relatives we've noticed a capitalized `CATEGORY:` followed by a list of attributes is very effective and commonly used among many different formats. Here we share some categories that have proven useful. The category words will be provided as a list as their use is self-explanatory. Some interesting categories will receive further attention. We recommend experimentation, but these words have been chosen because they felt more effective than other synonyms the Discord tested. Based on experience we have the following recommendation: plural forms are preferred for noun categories e.g. HOBBIES, POWERS whereas for verb categories its preferred to skip the `s` e.g. APPEAR, LACK. The long list of categories:
```
APPEAR, BODY, LOOKS, WORN(STYLE/FASHION), INV, EQUIP, MENTAL, LIKES, DISLIKES, HATE, LACK, RELATIONS, FRIENDS, ENEMIES, TALENTS, HOBBIES, POWERS, THEME, ORIGIN (for series/titles), ATMOSPHERE, TONE, MOOD, CLIMATE, GEOGRAPHY/GEO, FEATURES (for locations), EXIT (for rooms), CITIZENS, CENSUS, PASSION, BONDS, ALIGNMENT, STATE/EFFECT(character status like being dirty, sleepy or ill), SEE, BIOME, FLORA (must match the biome), CREATE
```
Categories can be shortened to save characters. The effectiveness of this is based on tokenization and following up with relevant traits. Not all short-hands work due to the AI mixing it up with other words with different meanings. If an example isn't given, you have to figure out the meaning yourself. Some examples: APPE, MENT, RAITS, CLIM, GEOGR, IZENS, PERSONA

The AI tends to be very literal-minded. TRAITS is the usually agreed upon god-category where you can put any attribute of an object. But the AI seems to treat it like the *nature* of said thing. The best example of this is the trait `cool`. In order to get a character to have a `cool` personality it must go in MENTAL. Inside TRAITS it might make the character literally `radiate cold` instead. The same is suspected of traits like `hotheaded`. Also, uncommon or complicated concepts like fauna do not work well as categories. `TRAITS: Fauna<species>` works better.

`SEE:` is the most interesting sense specified as a category. Using it you can get characters to do interesting things like 'see' the world through vibrations. Using sounds in SEE does interesting things, but doesn't produce synesthesia consistently, for that you need TRAITS. Other senses have been tried, but haven't demonstrated impressive results. Combining `LACKS:eyes;SEE:blind;` has produced the most consistent Zaltys format blind characters. Research continues into creating mutes albeit TRAITS works decently.

Shortening when used with purpose and tact works with other aspects of the game too so a trick bears mentioning here. If you want to save characters you can save multiple keys like `Enterprise, Enterp` and use the shorter `Enterp` as they key in your WI. This works better if the small key is a substring of the long name. You also want to use a substring that isn't easily confused with other words as is with this example. This may be more difficult and less worth your time if you use scripts.

Not all details need their own category. RACE, GENDER, AGE can be condensed to `elf/female/25y` then followed by the bigger categories. The AI picks up common traits just fine by their lonesome. TRAITS is a catch-all for when you don't need a big list on a single topic.

The AI understands the concept of speech patterns and accent to some degree. `SPEECH:`, `WORDING:` and `ACCENT:` have been tried **BUT** thus far `TRAITS:` has proven to handle this attribute the best. Writing style theme in author's notes can also give characters accent across the entire setting. Known working accents: pirates, sailors, Shakespearean accent, archaic, Cockney accent, valley girl (valley girl is the best method for getting a Southern accent on both sexes), hillbilly, Jamaica, crook/criminal. Below is a sample Zalty provided with the WI `Zack:[Male human. TRAITS: surfer, heavy accent<Jamaica>.]`
```
QUOTES FROM ZACK:
"Zis is a really bad time to be messin wit me mon!"
"The waves, the wind and the sky, zey are all one like you and I, man."
"I hope dat you don't mind some reggae music, man."
```


### Monky Formats
Optimization and consistency are major goals for WI-experimenters. Monky's solution to many isseus with WI is what he calls the `Caveman` format. Looking at an example will show us why:
```
keys:Chagra, initiate, apprentice
Chagra AKA 'Tusk': Orc she 20y.
Chagra 220cm ht. 100kg hardy.
Chagra green scar skin. Long brown hair.
Chagra loyal careful outcast pranked Healing magic initiate coven.
Chagra Hergea's daughter Friends You Renni.
Chagra likes fruit veggies Hates meat cheese.
```
In this WI all words lacking semantic value (content meaning) have been removed. Still, testing has AI mention all the relevant details with very low risk of leaking into other things defined with WI. It is likely that the brutal efficiency of AI doesn't care about words like `is`. Users of the format feel that it sticks to the given information better than the base AI alone or untested prose. And so far no one has reported that the AI starts speaking caveman when using this. If issues like this do occur they can always be solved with alter or retry. You should probably use highly descriptive inputs or an A/N: for making the AI write descriptive prose though. Worth noting is that Monky never uses commas when writing his style of Caveman.

Colors are one detail the WI has frequent problems getting right. Monky claims that he gets good results on caveman for results just stating the color and what it is for. A verbose example of monky defining adjectives to describe his character (typically the main character) is as follows `Alisha long dark brown silky shiny wavy hair.`

There are two good resources for this format, Monky's test scenario and kim's caveman generator, both linked in order:
https://play.aidungeon.io/main/scenarioView?publicId=558a8d40-2fbd-11eb-9239-8b8f17f7a2b0
https://play.aidungeon.io/main/scenarioView?publicId=3f094990-2e40-11eb-b81d-a3d32aaa3e7a

During experimentation on caveman Monky made important observations. When he used the caveman format extensively on Griffin he found if he didn't mention the character/key every 50 characters/15 tokens the AI would 'lose focus' and start describing random attributes outside of the WI. On Dragon the limit before he needed to start mentioning the key again was around 200 characters. Discoveries like these have led some users to believe the key should be reinforced at mentioned intervals depending on which model you use.

TL;DR A maxed out Griffin Caveman WI should be ~10 50-character lines long, each line starting with the key. On Dragon this could be 2-3 lines each maximum 200 characters. 

It is worth checking out birb's research section for tips that relate to Caveman use as well. Monky has developed a second format based off Caveman. It's called Caveman 2.0 or more affectionately `the Neanderthal Format`. Monky himself has authored a thorough write-up on the format and it's merits which is provided in this repository as a supplementary material, much thanks for his hard work. Below is a sample Neanderthal WI.
```
 Alexandria:[ Age 29 human she tall strong thin brave knight, your ally]. 
 Alexandria:[ Has Blackflame rune sword, steel armor, healing potion, amulet].
 Alexandria:[ Long red hair, fair skin, narrow green eyes, clean].
```
It looks like a mixture of Zaltys and Caveman. Some important observations follow. Every newline and every sentence inside brackets starts with an empty whitespace. This is done to guarantee proper tokenization of the word. The `.` follows the brackets due to the model's natural writing bias. Since it looks more like prose, commas are allowed. On Griffin sentences inside brackets are no longer than 50 characters/15 tokens to maintain focus. Below we provide the full dissection of Neanderthal written personally by Monky:
https://github.com/valahraban/AID-World-Info-research-sheet/blob/main/Neanderthal_Unscripted_Results_by_Monky.txt


### Onyx Formats
Onyx has come up with interesting and reliable tricks for specialized uses. With RND we can actually have pseudo-random lists. Testing so far suggests with 10 members the list is completely consistent, more members or simple words make it output more random, but still conceptually related things. This works better on Dragon, but the concept can be used to invoke lists of names on Griffin. All Onyx formats still work, just remember to replace `()` with `<>`.
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
From further testing the community has discovered that defining locales like this may benefit from placing each category on a newline. Both methods are effective. 

Zaltys has also made discoveries on how to semi-reliably define the structure of a building. These are listed with Onyx as an expansion of his concept. As a category EXIT works better than EXITS. Directions are good categories, but specifically for architecture.
```
keys: library
Library room: large room. EXIT: corridor (north), lounge (west), trophy room (southeast). FEATURES: Bookshelves (dusty tomes, esoteric, mythology, grimoires), man-eating plant (huge), spiral staircase (to second floor), chandelier (creepy).
```


### birb research
Many sources confirm that newlines (pressing enter) is powerful at grouping and separating certain traits. ALL CAPS is good for defining the group, encapsulation like () is good for the reference point. Thanks to monky caveman we know the AI doesn't care about grammatical correctness in the WI either. Sometimes `red-color` or `hair-color` may refuse to work consistently. In an attempt to fix this the community discovered that AI groups words strongly together, yet knows to separate them if you smash them together into compound words even when grammatically incorrect. For example:`verylonghair`,`platinumblondehair`, `tatteredgrayrobes`. birb calls this smashing words the more correct term being mashup. With this discovery even traits like `flamingorangeeyes` are possible to get out consistently. birb is still experimenting and undergoing revision on both of his formats, because while preliminary results were good, they broke on long scenarios. The angriest wizard that ever lived is provided for historic purposes (now outdated due to `()` being weaker than before replace with `<>`):
```
keys: Rick,the angriest wizard that ever lived
[TITLE(Rick):The angriest wizard that ever lived;
RACE(Rick):human, wizard;
APPEAR(Rick):old, tall, lanky, wrinkly, mullethair, verylonghair, grayhair, graybeard, messybeard;
EYES(Rick):orangeeyes, flamingorangeeyes;
WORN(Rick):tatteredgrayrobes, pointywizardhat;
LACK(Rick):anyshoes;
MENTAL(Rick):furious, loud, irritated, unstable, complaining;
HOBBIES(Rick):shoutingatkids, breakingwands, misusingmagic.]
```
Smashing words together is efficient but there is ALWAYS a risk of the AI misinterpreting the words depending on tokenization when you try this. For example `catears` gets tokenized as ca|tear|s. One caveat of the format birb used is high token use. A "worst case scenario" WI that used many Japanese words and symbols produced 387 characters, 160 tokens so 2.42 ch/tk. Smashed words may be used with any format even Caveman (potentially leading to the greatest character and token savings). If you encounter the AI shortening names excessively while using Caveman, you can start each newline with an empty whitespace ` ` to change how the name tokenizes. For example `Vladimir` tokenizes as V|lad|imir but ` Vladimir` is all a single token.

During testing we discovered characters can be forced to wear certain outfits with complete consistency. This format needs at least 2 WI the character being dressed and the custom outfit. This theory could be applied for customizing the appearance of any object in your game world. Example.
```
Lillie:[Human female. Her eyes are blue. Her hair is blonde. Her hairstyle is long twintails. WORN:{Nightfall Raiment}.]
```
```
[The Nightfall Raiment is a one-piece halterneck dress. The dress is in gothic lolita fashion. The Raiment has a black and white color-scheme. The Raiment has detached white sleeves and shows off the wearer's armpits. The bust and frills of the relatively long nightfall raiment dress are white while the rest of it is black. Kneehigh black leather boots are a part of the raiment.]
```
When you examine Lillie in the story and have the AI describe what she is wearing you will get an output like the following very consistently: `She's wearing a nightfall raiment. The dress consists of detached white sleeves and shows the armpits. The bust and the frills of the dress are white while the rest is black. Kneehigh boots are a part of the outfit.` When tested on other outfits `the clothing consists of` was a very good input to continue from for the story. This was demonstrated on Discord. The above example combines almost-Caveman with Zaltys. If you use a style like this, you must make it conform to your writing style. Zaltys calls the uncategorized part of his format the indicator. This is usually details like sex species and age. In the Lillie example species, sex and colors were indicators. birb always wants the AI to describe the traits of the characters in the format `Her eyes are X color. Her hair is Y color in Z style.` This is why this type of WI works for his scenarios. Also, if the AI starts describing the clothing of the character before you get to ask what the character is wearing, it will usually output the wrong set of clothes. First the story must mention the name of the outfit, then it will use the correct WI to describe the clothes.
The document now provides evidence for certain methods working. Included is a gif of the outfit being in use. You will have to take the author's word that the AI can consistently mention the outfit and then describe it.
https://files.catbox.moe/9dibjo.gif

Now that the category `EQUIP:` is confirmed working with clothes and other inventory it would be possible to use the above example to come up with custom named weapons for your characters, write WI for the custom weapons and have the AI invoke their traits reliably.


### Misc Tips
More tips and tricks that fall out of larger headers will be added here as they are discovered. `NOTES ON X` has been used by some as a WI before, but most feel it serves better as a remember pin or an input for the story. CrisAIcilian has discovered `Character sheet - ` as an effective header for WI, using an otherwise Zaltys-like format together with it in his test character. After roughly 100 rolls he claimed to achieve a 70% success rate on his character traits & actions being mentioned correctly as long as the story input features new letters instead of being a simple `continue`. The author speculates this could be a good way to define something akin to `you` for those interested. All of the random experimentation and associated pictures can be found on Discord as the author remains unconvinced to the relevancy of testing inside AID.

Earlier in the document we mentioned Editor's Notes. This concept is used manually or with a simple script. The in-game tooltips describe author's notes as style hints or being useful for controlling what and how the AI will generate. Paraphrasing from Gnurro(who came up with the idea and authored the script) `EN behaves more like tooltips suggest AN should work as some kind of more or less direct command about how things should go on. AN needs other wording, but which work best for it has been fiddled with a lot by now. Someone should try combining AN and EN though, would be interesting to see if that gives some kind of belt/brace effect.` Private testing has shown that EN works more like a conductor, keeping the output on whatever track you've given it. More testing is needed combined with things like the `Editor's Notes: This novel has no plot twists, it follows a linear storyline.` consistency string and `RATING:` when used in notes. EWIJSON can achieve this easily. 


### Character-to-token ratio tests
Here are some testing results with the tokenizer from kimtaengsshi and the community's testing. This is largely older data that motivated strides in format production. One result was the format monky now uses. Results like these are a big motivator for why people get obsessed over formatting. We tested a bunch of WI-formats from the discord and analyzed their tokens to characters ratio with the following results:

Full prose (standard): 514 chars, 118 tokens = 4.36 ch/tk

Pure JSON: 512 chars (-2), 172 tokens (+54) = 2.98 ch/tk

Pure JSON (minimized): 475 chars (-51), 138 tokens (+16) = 3.44 ch/tk

Zaltys-Basic: 393 chars (-121), tokens 122 (+4) = 3.22 ch/tk

Zaltys-Advanced: 358 chars (-156), 128 tokens (+10) = 2.8 ch/tk

Monky-Modern: 352 chars (-162), 123 tokens (+3) = 2.86 ch/tk

Monky-Caveman: 362 chars (-152), 87 tokens (-31) = 4.16 ch/tk

Worst-case-scenario birb: 387 chars, 160 tokens = 2.42 ch/tk

The goal here is optimization. Full prose has the best character-ratio, but has all the caveats AID is known for. Many users love the output pure json gives, but it has some of the worst ratios and character usage. Zaltys results aren't much better although this can be attributed to heavy symbol and possibly unicode use. Caveman is technically the winner here. It allows the most characters to be used for the context while preserving content meaning while it may not have the absolute best token use. birb format is a historic curiosity that despite promising testing results been removed while the author focuses on ironing out goals like consistent color output.

Full prose is outside the scope of this document. Its the default format people write on story mode or for the common WI. On Dragon it can provide good results if you're a very skilled writer, but isn't specialized to achieve specific goals. The following is an opinionated take on prose:
```
From Monky: using prose descriptions for griffin is like trying to get a drunk guy to follow and regurgitate a paragraph
```


### Worlds Specific Tips
Worlds is the latest feature and as such has received the least amount of experimentation. Thankfully worlds works mostly likely a regular scenario containing the same data structures while a lot of it may be hidden from the player. Worlds have more thorough input fields at the beginning and settings akin to sub-scenarios. Most WI enthusiasts prefer making their own custom scenarios. The world experience can be improved by adding your own WI or with a world-specific prompt trick. This is usually done through the `Name:` field during character creation this being the main way of influencing how the World generates. You input your info into the `Name:` field. This is most effective with JSON-like formats. For example if you put `[{"name":"Eve","theme":"pirate"}]` in the name, the World will name your character Eve, while giving the World a noticeably more pirate feel. Quoting what we know so far from a user named Gen.Alexander:
```
Prose names like "Bill the fallen paladin," "Jessica the necromancer," etc. affect World prompt generation too. But with JSON you can really cram a lot of stuff into it (and it's likely to include at least half of it in the prompt.) Like
[{"name":"Jessica","personality": "ambitious", "birthplace": "Waterdeep", "worships":{"Mystra":"godess of magic"},"relationship":{"Elminster":["teacher","boyfriend"]} }]
I just tried "theme" for that yesterday and it seems to work too.
```
```
Xaxas is a world of peace and prosperity. It is a land in which all races live together in harmony. The gnomes build their machines and live among the humans. Elves run the academies in which people come to learn. Ogres have no qualms living in the same town as humans or gnomes. Yet, all is not well.

You are Jessica, a female human mage from the northern human nation of Waterdeep. You've come to Uruk to meet your boyfriend, Elminster, who previously taught you magic in Waterdeep. You've been secretly practising magic in your spare time and have recently come to the conclusion that you want to dedicate your life to becoming a great mage and opening a school of magic in Waterdeep.
```


## Other Scenarios and Scripts
Since this document shares some scenarios for testing purposes more bear mentioning. This scenario uses a modified Zaltys-like format. You give it names of characters or even concepts and test how the AI model generates information for it. This can be used to check how well the AI understands interesting words by it's lonesome. While designed for characters you can use it to have the AI describe ANY word. If this does not meet your needs, you can always use this as a base and create your own scenario with your own format.
https://play.aidungeon.io/main/scenarioView?publicId=6a344070-2a58-11eb-9973-ef0c0f08ab62

Not all users have the same imagination, language skills or amount of time for creating scenarios. Maybe you're lacking ideas but have a general idea for a testing scenario to test your new WIs with. AID user LaPapaya has created a public tag-based scenario generator. You should still use retry and alter but it has worked well for the author. Linked also is a later variant made by perroquit.
https://play.aidungeon.io/main/scenarioView?playPublicId=913fd250-cc8f-11ea-aa35-1f790f909406
https://play.aidungeon.io/main/scenarioView?playPublicId=1b604670-33df-11eb-abb1-03ac45f1d495

Scripts are finally free for all users! Creating or editing a script also requires knowledge on coding. A user named Zynj is working on some of the biggest scripts that may help WI-users. Below is his public git. It now contains all his scripts ready for at least beta-level public use. JSONthing has been for a long time praised by premium users as drastically increasing the consistency of their outputs. This has now been combined with Zynj's EWI to form EWIJSON which can be found in the `AID-Script-Examples` directory. This is extremely recommended by Discord users despite the extra tedium scripting introduces. Zynj has also provided wiki-format documentation.
https://github.com/Zynj-git/AIDungeon
https://github.com/Zynj-git/AIDungeon/tree/master/AID-Script-Examples/EWIJSON/release
https://github.com/Zynj-git/AIDungeon/wiki/EWIJSON

Protip on convenient scripting and scenario writing. Write one completely empty base scenario in your stuff. Include all the scripts and WI you like to use in all your scenarios in it. When you run this scenario, you will be asked to write a starting prompt. Use this scenario as your testing ground for scripts and scenarios. When you're happy and everything seems stable, you can copy everything from this scenario into a new scenario, where you can write the actual prompt and new extended WI with less of a hassle.


## Obsoleted Methods
This section covers methods WI enthusiasts have used in the past but are no longer recommended. For example earlier Zaltys formats are included here for historic reasons. This is so we leave a record and can reference things we used to do if OpenAI or Latitude revert any changes they've made on the back-end.

Title: Symbol use
Updated 10 Jan 2021:
`-` `'-'` and `()` are no longer effective. They are treated more like math symbols by the AI now. Onyx has done extensive testing on the Discord to showcase the AI's creative math abilities. 
https://files.catbox.moe/8y17ji.png

Title: Legacy Zaltys Deekin & Aarakocra 
Updated 10 Jan 2021:
```
keys: Deekin, Scalesinger, Deekin Scalesinger
Deekin Scalesinger:[Kobold/♂/<72cm>;ORIGIN<Deekin>:Ally from Neverwinter Nights games(CRPGs);APPEAR:Reptilian/orange-scaled/horns(nubs)/snout/legs(2)/eyes(brown/2)/arms(2)/tail(scaly)/wings(2/red/scaly);LACK:hair/ears;WORN:Bard clothes;MENTAL:Pessimist/creative/polite/kind/friendly/self-conscious;TRAITS<Deekin>:Musician/poet/bard/writer/raspy voice/hero/sorcerer/plays lute/beat Mephistopheles/bard-magic/former servant(of dragon:Tymofarrar)/"Yes, Deekin very kobold, last Deekin look in mirror."]
```
```
Aarakocra:[Avian(150cm/42kg). APPE<Aarakocra>:Slim/feather-covered(♀:brown-hue or white-hue/♂:colorful)/feathered head/2-eyes(keen)/tail-feathers/2-wings(big wingspan)/digitigrade legs(2/bird-feet/talons)/2-hands(clawed/5-digits)/hooked-beak(sharp);LACK<Aarakocra>: hair/ears/arms/breasts;MENT:Wise/honest/calm/benevolent;TRAITS<Aarakocra>:Prefer flying/lowtech tribal/hunters/scouts/adventurers(rare/rangers/druids)/no ownership/javelins(weapons)/live in mountains(isolationist)/claustrophobic.]
```

Title: Old style categories with heavy encapsulation
Updated 10 Jan 2021 (original from Nov 2020):
Hunter, Lazy and other users have been working with shortened formats motivated by JSON. Originally parentheses were removed perceived as unnecessary. Early tests suggest it works just as well as proper JSON, bearing in mind the tricks discussed above. The author would recommend pursuing variants of this format for a good balance between accurate details and token use. Originally people called this pseudo-JSON, but more accurately it is pseudo-prose. Token testing shows commas and brackets work as contextual linkers just like in regular grammar.
```
keys: Thea Smith
[{name:Thea Smith,age:25y/o,♀,BODY:[toned,muscular,athletic],skin:[tanned],HEIGHT:[tall],BREASTS:[medium],HAIR:[Purple,very short,mohawk],EYES:[glowing,blue,heavy eyeliner],PERSONALITY:[calls you shorty,Punk,adventures,kind,rebellious],LIKES:[dancing,music,working out],relationship:{Tess:[little sister],Finn:[little brother],Beth:[Mother,Mom],Bob:[father,Dad]}}].
```


## Special Inputs Commands
What are world infos if you can't utilize them? Here are some interesting and recommended `inputs` discovered by the community. A lot of them work in conjunction with objects you've defined into WI. If an input has a field surrounded by <> you don't need to type those container characters into AID. `Author's Note:` can also be used as an input to have the AI generate something interesting for you.

- Name:"Text"
- List of X:
- POV [character]: or [character]'s POV:
- 2nd Person POV:
- Your POV:
- Description of x:
- X described by Y:
- Do mode >examine/inspect X
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
- [character] gives a lengthy monologue about [topic]
- Advice from the voices in your head:
- Rationale:
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
- TL;DR: (or Too long didn't read:)


## Ending Notes
As mentioned before AID is in constant development and flux of change. Changes to OpenAI and the GPT will reflect on AID too. WI testing was difficult in October, because updates to safe mode and tackling a long-standing caching bug caused issues with the AI especially on Griffin an event which is now affectionately called the Great Stupid. We look forward to what updates the devs have in store for us. Make sure to export and catalog your WI so you can test them out whenever the AI feels stable enough to your liking.

4chan has threads and guides too. They have been beneficial and entertaining to the author. Go on /vg/ and search for /aidg/ if you want their guides and sources, some of it is good especially for NSFW. The special inputs section benefited greatly from their work. Prompts collections exist online but are not provided here due to their NSFW nature. Below is a link to the Anonymous resources & guides, please cross-reference their work with ours:
https://guide.aidg.club

CREDITS: Everyone whose name has been mentioned here (you guys rock), the many AID related communities and the author for compiling this. Everyone who shares their experiments and results online. Anonymous imageboards, aidiscord-wiki and the Discord itself which can be found through provided links. Kanye West is divorcing his wife for a trap and a whole lot of sociopolitical shenaningans are going on. Looks like 2021 will be a wild year. See You Space Cowboy.