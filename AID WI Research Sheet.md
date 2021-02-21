# AI Dungeon World Info Research and Reference
- [AI Dungeon World Info Research and Reference](#ai-dungeon-world-info-research-and-reference)
  * [Preamble](#preamble)
  * [Recommendations or the TLDR](#recommendations-or-the-tldr)
  * [Remember WorldInfo AuthorsNotes Worlds and how the game handles them](#remember-worldinfo-authorsnotes-worlds-and-how-the-game-handles-them)
  * [Griffin vs Dragon](#griffin-vs-dragon)
  * [Tokenization - understanding limitations and special characters](#tokenization---understanding-limitations-and-special-characters)
  * [On certain characters](#on-certain-characters)
  * [Authors Notes specifics](#authors-notes-specifics)
  * [World Info and Formatting](#world-info-and-formatting)
    + [JSON Formatting](#json-formatting)
    + [Zaltys Formatting](#zaltys-formatting)
    + [Useful Categories](#useful-categories)
    + [Categories Table](#categories-table)
      + [TRAITS and SUMMARY (SUMM)](#traits-and-summary)
      + [MIND (MENTAL)](#mental)
      + [APPEAR](#appear)
      + [WORN](#worn)
      + [DESC](#desc)
      + [SEE](#see)
      + [LIMIT (LACK)](#lack)
      + [COND](#cond) ([STATUS](#status))
      + [DIET](#diet)
      + [LOOT](#loot)
      + [DETAIL](#detail)
      + [More on SUMMARY](#more-on-summary)
	+ [Personality keywords](#personality-keywords)
	  + [Bad guy / evil / cruel / saristic](#bad-guy--evil--cruel--sadistic)
	  + [Paladin / lawful good](#paladin--lawful-good)
	  + [Manly / serious](#manly--serious)
	  + [Other assorted useful traits](#other-assorted-useful-traits)
    + [Monky Formatting](#monky-formatting)
    + [Onyx Formatting Tricks](#onyx-formatting-tricks)
    + [birb research](#birb-research)
	+ [CrisAi research](#crisai-research)
    + [Misc Tips](#misc-tips)
    + [character-to-token ratio tests](#character-to-token-ratio-tests)
    + [Worlds Specific Tips](#worlds-specific-tips)
  * [Other Scenarios and Scripts](#other-scenarios-and-scripts)
  * [Writing Tools](#writing-tools)
  * [Obsoleted Methods](#obsoleted-methods)
  * [Special Inputs Commands](#special-inputs-commands)
  * [Ending Notes](#ending-notes)


## Readability note
This document contains a lot of examples in code blocks, with very long lines of text. If you're reading this from GithHub website (I bet you are), you might want to add this CSS style in your browser to enable word wraps:
```css
code {
    white-space : pre-wrap !important;
}
```

You can also use [Dark Reader](https://darkreader.org/) or similar browser extensions to switch GitHub to a dark theme.


## Preamble
Perhaps the greatest addition to the AI Dungeon for generating high quality stories has been world info. Learning and figuring out how to use world info effectively is of great interest to many users: using it one can get vastly more consistent output from the AI and define terms that the pre-trained AI might otherwise have difficulty understanding. Features like `Remember` (pins) and `A/N:` (author's notes) will also be covered as they are heavily related and connected to world info. Everything here is of use to both free and premium users. The differences between griffin and dragon will be discussed as there are some differences one should be mindful of. Good world info can make griffin almost as good as dragon, but requires more attention and effort from the user. While world info is a powerful tool there are also caveats and concerns to using it effectively. With good world info the user can create any kind of scenario and a `world` of their own. This document is written with enthusiasts in mind and will not be dumbed down as that would be counterproductive to building deeper understanding.

A friendly reminder: This document isn't authoritative and doesn't pretend to be so in any section. It is a compilation of the community's collective and shared experience, primarily through the official AID Discord and burrowing publicly available material from other fan-run communities. Originally it was the author's personal notes and should be treated as such. This means the evidence and advice is mostly anecdotal. Citations are shared with permission from the users. Most of them can be found as messages on Discord. Some links may be provided, but please understand imagehosting is unreliable. Attempts have been made to explain which practices show the most promise. Everything has been repeatedly tested inside AID scenarios and the tools provided to us by developers and scripters. You will have to take the author's word on it for the peer-reviewing process. Outputs will mostly be skipped for the sake of brevity as they can be tested inside AID. Do your own research and testing. If you fail to do so, criticism won't be taken seriously.

The start of the New Year has been a time of rapid changes for the AI and Latitude's business model. There have been many changes that have affected formatting and scripting is now a free feature. Materials from other authors and their work will be provided with permission. A new section is included where we discuss past methods that no longer work as well as they used to. The owner of the repo is the author of this document and goes by many pseudonyms, birb being one of them.

Special credit to kimtaengsshi for help with editorship and fact checking the technical details. This document will contain data kim and others have confirmed through monitoring LMI (Last Model Input) and other trustworthy testing methods. Much of this information comes from and is shared with the community-run wiki and Discord. A link to the wiki will be provided where you can also find an invite to the Discord and other useful content: https://wiki.aidiscord.cc/wiki/Main_Page

If you don't know anything about world info yet and want to get up to speed with the bare basics it is recommended you read the wiki article first: https://wiki.aidiscord.cc/wiki/World_Info  

This document isn't anti-prose, but prose clearly has nothing to do with the subject matter and as such won't be focused on. This document is about getting results by manipulating AID's context using *any* method available. There exists a countless amount of resources online to help you become a better writer overall which should also improve your AID experience.

How to get featured or help improve this documentation? Send a text file (preferably .md) or suggested edits in a pastebin variant to birb/valahraban through github or discord. This is how he primarily communicates with other enthusiasts and writes this document. Other communication methods aren't supported. Included information isn't chosen at random: it must be novel and immediately useful to birb and WI enthusiasts.


## Recommendations or the TLDR
**Scripting has been made free for all users, it is warmly recommended you scroll down to Zynj's github and check out EWIJSON. Loading WI through these scripts alone gives you a lot more control over their location. If you do decide to use EWIJSON, also check out Monky Formatting and the Futureman guide and birb's guide on EWIJSON basics inside the docs.** This document will focus on scripting-free methods. Scripting focused documents are included in the docs folder.

For those of you who came here for the WI formats or have short attention spans. Click on the links contained in the table of contents above or hit CTRL+F to search for names that interest you. **JSON formatting** is neither recommended or discouraged. If you're comfortable using it continue doing so. The **Zaltys Formatting** of this document is recommended as a strong baseline to start from. Zaltys formatting is easy, popularized and frequently updated to keep up with AID. Past samples are provided as well. **Monky Formatting** is recommended for people with specific goals and interests such as intending to publish scenarios, worried about WI space and token use and wanting to share their work. It is very understandable, easy to modify and works well with regular prose. Very easy to combine with scripts too. Monky formatting has developed into Neanderthal and Futureman which are recommended reading for people they benefit. **Onyx** has written formats and WI tricks for specific purposes such as defining locations and listing things. 

We now use the term formatting as some of the concepts are interchangeable and may be combined together. One scenario may also use different WI formatting for achieving different things with different entries. You the writer have complete creative freedom, although our experiences may benefit you.

If you don't care about the complicated lunacy of this document and want something simpler, more elegant and less of a headache, learn how to write good prose or read a simple guide on it! Now provided by our fellows at /aidg/  
https://rentry.co/aidg-format

For comedic purposes here is what one very opinionated user has to say on the current state of formats:
```
From CrisAIcilian: Zaltys is better in every way, it's actually designed for its purpose. JSON is like Chinese medicine, goo of mumbo jumbo happens to contain three elements that had an effect (colon, comma, square brackets).
```


## Remember WorldInfo AuthorsNotes Worlds and how the game handles them
Remember was the first of these features implemented in the game. This segment goes into technical details so you may want to skip it if you're here for World Info formats. This section is very important if you want to understand AID as a tool and it's limits. It will also answer most problems people have with their WI not working. Grossly simplified remember is a world info that gets constantly fed to the game after your every input. World info is like remember, but has user-defined key-words `keys` that after getting triggered make the game use them as part of the context. Keys use string-matching which will be important later. Having this element of context makes world info less restricting to work with because when you're happy with it you can forget and leave it while things in remember require more frequent attention and updates as the story progresses. History means old prompts written by you and the AI. The order the AI reads your input is in:

+ Summary										| Experimental/deprecated feature to help devs
+ WI
+ Remember
+ History
+ AN if you've exactly 3 newlines in history
+ Rest of history
+ Fresh Input
+ Front memory from scripting					

The following data is based on information available to scripters or based on their limit testing, documented on discord. All of the above consumes characters or as we will soon discuss tokens from the AI's `memory`. Anything that goes over the `memory` limitations will not be read by GPT-3 and is therefore irrelevant to the output process. The entirety of the data(text) that gets fed to GPT-3 is called the `context`. The following is a list of rules the `context` follows as related to the many parts that form it:
```
- Max. input character length: 2772
- Max. Summary length: 500
- If (Summary + WI + Remember) < 50%, then extra story aka history will fill up the empty space
- If (Story + A/N + Input + frontMemory) < 50%, the unused space will not be allocated to (Summary + WI + Remember)
- GPT-3 has a hard cap of 1024 tokens for AID users, 2048 for the devs
```
For brevity the maximum context length is often shortened as 2.8k. In regular use a single WI has a limit of 500 characters and remember has a limit of 1000 ch. On the plus side thus far no limits have been discovered on *how many* WI one can define. It's not recommended, but it is possible to define multiple WI with the same keys, essentially increasing the space by 500 ch on every iteration. Keep in mind AID always adds a newline between each WI even those using the sake key. AID is smart about using WI, but not about remember and updating remember constantly is tiresome therefore many advanced users focus on producing high quality WI while foregoing longer remember pins. The limitations of GPT-3 and AID is why understanding and researching WI is of interest. Worlds will be discussed later as they use the same data structures as regular AID.

With scripting it's possible to circumvent the regular ordering of each element. This is a topic that the user should keep in mind if they are interested in scripting and the potential benefits.


## Griffin vs Dragon
AID contains multiple machine learning models the primary ones being Griffin and Dragon with Lovecraft being a later seasonal addition. Modern versions of AID Griffin and Dragon are both GPT-3. This has been confirmed by the devs on twitter, discord and other public forums such as their medium. That being said there are different sizes of GPT-3. Griffin uses a subset of GPT-3 that is closer to the original 1.542 billion parameters used by the GPT-2. The full-sized GPT-3 that Dragon is based on has a whopping 175 billion parameters. This shows in the quality, consistency and most importantly *forgiveness* of the model and the output it produces. Both models love a detailed and consistent context, but Dragon is more forgiving in terms of formatting. This shows especially in how differently it responds to WI you feed it. Griffin has quirks you have to be mindful of. These affect what kind of WI you write and will be discussed in the following sections.

It used to be that the first prompt and response were generated in *GPT-2* but this is no longer the case, it depends on what model you use. If there is a populated prompt, the first output will be in whatever model you're using. Only a completely empty prompt would be GPT-2. So at minimum put something in your prompts. This is official information from Alan. Both detailed history and WI are important for training the AI for your scenario and getting what you want out of it. Either redo until the writing looks high quality enough to your liking or spice it up with attentive alters, the key to enjoying AID is building on 	success.


## Tokenization - understanding limitations and special characters
The character limitations mentioned in the previous section are ones imposed by the devs of AID. They are quite sensible, plenty for generating good stories, not bankrupting Latitude and not horribly far off from the true limitations of GPT-3 that both models now run on. For a machine learning transformer to read an input it must first be tokenized into a form the model understands. GPT-3 has a hard limit of 2048 tokens. It won't read more at once. Devs have access to 2048 tokens, the users are given 1024 per input. But as you've noticed, you have more words to work with that the AI clearly understands. Tokens are just the way data is encoded into the machine learning model; during training, common words became recognized and encoded into an efficient form, sometimes even a large word gets recognized as 1 token. From kim's research, depending on how a world info is defined, the counts average to 2-4 characters per token meaning you get to use at least double the characters of the tokens you really have hence why a commonly mentioned character limit is 2048. 

A few things have been learned from experience; common words and phrases like `hello` or `25y` get treated as a single token. White spaces are special because they often get rolled into words as is grammatically correct. An uncommon word like ` Enterprise` takes 1 token with the white space while `Enterprise` takes 2 tokens. Unicode emoji like `😃` and some special symbols are expensive, they cost 2 tokens per character. As we will discuss characters like `<>` `[]` are useful for WI, but tend to have a cost ratio of 1 token. There are also rare characters that the AI hates like `ù`. Experiments have shown that longer strings of symbols like `}}}` get rolled into 1 token.

If you want to test out and learn more about GPT-2/3 tokens (they work the same) visit this link: https://colab.research.google.com/drive/1i1wXfc4SWDprHZuLTaMsgWPv6YwvlQ_I?usp=sharing#scrollTo=mlBq9zO67b2n

kim has also made a tokenizer scenario/script you can use inside AID: https://play.aidungeon.io/main/scenarioView?publicId=6927d610-34b7-11eb-b8e1-c185d026672d

Tokenstuff script by Gnurro that can be used to mass search and catch what tokens exist inside GPT-2. Remember, by the word of the developers(God) GPT-2 and GPT-3 share the same tokens: https://github.com/Gnurro/AIDscripts/blob/main/tokenStuff.zip  

It can not be understated what an important tool the tokenizer has been for WI enthusiasts. Thanks to it we have a fairly good idea why certain words work as poorly as they do and a legitimate information-based argument for why this is the case. If the document talks about word or character tokens, they were ran through the tokenizer colab or scenario alongside synonyms or longer compound words containing it.

A common misconception is that """natural writing""" is clearly the best writing method and all other methods and tricks should be ignored because some marketing person said so. To these people I must tell: Stop. You don't know what you're talking about and are likely arguing from emotion. Machine Learning language transformation algorithms expose their training biases through tokens. You can check the tokens through the tokenizers. They are the same for GPT-2/3 and the Griffin and Dragon model trained off GPT-3. The GPT-3 has eaten a massive amount of online datasets, not restricted to plain English. Tokens exist for html comment wrappers like `<!--` and `-->`. This means they were a relevant part of the GPT-3 training and will be recognized by any AI program using the same algorithms.

Input the string `  SolidGoldMagikarp` into the tokenizers above. This nonsense word is one token I kid you not. One possible explanation for this we've found is reddit.com/r/SolidGoldMagikarp/ although the true source of the token remains unknown. `================================` and many other chapter breakers are one token too. This means GPT-3 strongly recognizes these strings and gave them a unique definition inside of its tokens. The AI has eaten data from a plethora of writing sources. You can further train GPT-3 to specialize on other forms of writing, as Latitude did with CYOAs for the AID models. That is an official statement. The author would have preferred they focused on high quality prose novels like the Witcher series or Dune (copyright problems), but we live with the tool given to us. We provide pictures of two posts by Gnurro as he researched the topic of unusual tokens:  
https://files.catbox.moe/xj9a6a.PNG	https://files.catbox.moe/qz4hwx.PNG

Any section of this document discussing tokenization has been verified with either of the above tools.


## On certain characters
Functionalities of certain characters have been found and described thanks to the development of `world info formatting` but it's useful to discuss these beforehand. Their functionality can be somewhat verified through the tokenizer Colab. Some characters connect. The most important of these is `-`. `hair-color` or `red-color` are words that get better results for colorization than other methods (may depend on format and order of traits). `-hue` doesn't work as well but may be used. Other characters have been tried for connecting words, but few show as much power in scenario testing. As a matter of fact often you can string random words together which will be discussed later. AID recognizes many separators. Letters , '' "" ; / | all separate words and concepts in rough degree of power. You would want to separate members of a list with , like in common grammar, but ; might be better for separating categories. Inside brackets `/` is useful for keeping the traits related to what they are describing. () {} [] <> : are connectors. You can put lists inside all of them or use `:` to create a logical implication. For example it might be preferred to do a list like `MENTAL:[happy, go-lucky, energetic];` and define logical connections like `AGE(25y)` or `sword(polished)` the AI usually recognizes that the thing you're describing is roughly 25 years old or that the sword is polished. Enclosures can be interchanged, but these are two commonly seen and powerful methods.

**The above used to be the case.** Whenever OpenAI or Latitude updates their back-end models, the significance of a character may change. As of January 2021 `-` works more like the mathematical minus sign. An example is provided in the document of doing pseudo-math inside AID. It is preferable to mashup words that are normally separated by `-` together inside the WI entry.  
https://files.catbox.moe/8y17ji.png

< and > are special characters. You may have noticed in do or say mode the AI always prefaces your inputs in the story with `>`. In AID the character is reserved for this purpose. You may have noticed that the AI *never* outputs < or > on its own. Since the information inside WI is handled differently we can still use any unicode letter inside it. In WI this can be used to our benefit as we will see when we discuss the work Zaltys has done and his formatting. The uses we have discovered for < and > are particularly useful, because they are unlikely to change even with future updates unless Latitude decides to drastically change their AI model.

The GPT is an autoregressive transformer language model trained on English writing. Any particularities AID are have due to unique parameters set by the dev or habits learned from big datasets like fanfiction.net. Characters like ! and ? aren't useful or interesting. They are only useful in conveying meanings they have within natural conversational English where they most often are employed in prose. The GPT isn't magic and it wasn't purpose trained on programming languages. The fascinating thing is algorithms and linguistic expression have many things in common by happenstance. As mentioned in the above section the GPT-3 also has many really weird tokens due to the datasets given to it.


## Authors Notes specifics
Let's discuss the premium feature `A/N:` first. If you're looking to inject flavor into the AI's outputs A/N will be your first and probably most efficient stop. It is a worthwhile for the convenience, yet it's usefulness is not entirely restricted to premium users with our advice. What A/N does is it insert a line of text into the history the AI reads e.g. `Author's Note: Some details like mood about this story.` exactly 3 newlines up. On the scenario screen it has a 150 character limit, but the in-built maximum limit is 300 characters according to kim's LMI testing and community experiences.

Discovering manual Author's Note inputs during dev testing and how heavily it affected output is why it was added to the core game. Even as a free user you can manually add Author's Notes into your story. With manual A/Ns you have more characters to work with, but it is doubtful that making a super long A/N: offers any extra benefit. Research suggests that A/N: can be used to reinforce writing style even of well-known authors through repetition. It can also be used to change perspectives and the `focus` of the storytelling. These findings suggest that the prompt informs the AI what kind of writing to expect directly after A/N: has been invoked. Exemplified below is a medium-length A/N: with reinforcing that has been confirmed to be capable of producing outputs where characters break the 4th wall:
```
[Author's note: This novel is esoteric and descriptive. Author: Terry Pratchett. Writing style:  Metafiction, in the style of Deadpool. Genre: Witty, talkative.]
```
The film rating system (not others) has been confirmed working. With `RATING: G` in A/N you can noticeably lower the amount of NSFW in your scenarios. Alternatively `RATING: R` makes things more racy. `X-RATED` works too with predictable results. As with most things this is more effective on Dragon. Arguably an even more effective method is using `Author:` and a known author who considered profanity to be rude such as Samuel Clemens or Edgar Allan Poe.

Switching focus is easy, but may require extra use of alter afterwards to make sure the AI understands what you want. This style of A/N: is best used manually written into the story (although you can change your global A/N whenever you want; a trick we recommend using).
`[Author's note: The story now follows Mary's perspective.]` or `[Author's note: We now switch focus on Mary.]`
The main character can also be defined in A/N vs memory `A/N: Main character: <species>, <gender>, <job>`

A/N likes both style-describing nouns and adjectives. `strikingly elegant` makes the AI output more elegant prose than just `elegant`. This may also work for other words predicated with strikingly. `THEME:` is a word that can get the AI to focus on concepts and their attributes such as pirates or ninjas.
An example of output generated with `strikingly elegant style`:
```
You look around.
The sun softly kisses your exposed skin with her rays. You gently bask in her glory, raising your face towards her. Below, the plants softly sway in a gentle breeze as though worshipping her as well.
```
List of common categories to throw inside Author's Notes: author, writing style, genre, theme, setting, scene, format, goal, situation, storyline.

A/N understands JSON too. Its entirely possible to do something like `A/N: [{"writing style":["descriptive", "elegant", "gritty"], "wording":["archaic", "Cockney accent"], "current state":"indoors"}]`

Some users have been experimenting with A/N for doctoring output and specifically managing randomness. Lower randomness is more consistent and predictable as it makes the AI's predictions more deterministic, but this comes at the expense of it feeling 'more dumb', hence higher randomness being desirable for verbosity. According to CrisAIcilian `This novel has no plot twists, it follows a linear storyline.` is a very effective phrase to include in A/N to increase consistency. This way his stories can stay on track even at 1.3 randomness. He finds that `Writing style:` and `Genre:` are useful categories to use most of the time. Writing style improves writing, genre works like a mini-prompt. More useful notes on writing A/N from Cris: 
```
"Author:" is added when you want to do a convincing real author (combined with "Writing style: literary, author-name.) "Setting:" "Theme:" "Subject:" have their uses, depends on the content. "Title:" works at least on LOTR and "gone with the wind". Didn't test other titles. Famous ones should work. Has to be a real title, "star wars" is not a title, it's a movie franchise. The writing styles, most don't work on Griffin.
Only the descriptive-branch works on Griffin. "descriptive" works, "narrative" and another one I can't remember (according to Zalty) also works on Griffin.
```

#### Reminder:
While less effective on Griffin anyone can benefit from using Author's Notes properly. With scripting, any user can mimic the functionality of Author's or Editor's Notes or even insert any string they want in a specific part of the context. A helpful user is experimenting with Author's Notes words all the time and has provided a resource that's being updated frequently: https://justpaste.it/9ofj1
 

## World Info and Formatting
The section most users will be interested in. For the longest time users of AID were writing WI in pure prose. It helped things, but things remained inconsistent especially with how fickle Griffin tends to be. Users started researching using different letters and sentence structures. Some people even tried coding inside WI or getting AID to code for them. After some time, users shared their results and many came to the shared conclusion that deliberate `world info formatting` leads to more desirable outcomes. These became known as `world info formats`. Some goals of structuring WI include:

+ Reducing randomness (or later adapted into a RND generator)
+ Defining characters, locations, lore AND getting the AI to remember and mention these things
+ Describing those things consistently and repeatedly
+ Staying on topic: more magic and less guns in fantasy worlds, no magic and more scientific guns in sci-fi worlds
+ Preventing leakage - the AI picks up on patterns and WI can leak (especially in Griffin)
+ Consistency both in how the AI receives data and how the user writes their WI (the AI learns from consistency, consistent WI are easier to write)
+ Formats can be mixed and matched for different tasks; characters, new verbs/actions, locales and regions might each have an optimal format that better suits the user's needs
+ Breakdown: sloppy WI and sloppy writing can lead to the AI 'losing focus' faster and forgetting about what you're trying to tell it with WI (occurs faster in regular stories)

One of the first formats commonly shared online was popularized on Reddit by a user curious_nekomimi in August 2020. The format had no common name, but each new line of the example started with Ben so that's what people usually called it. Every newline would start with the key being mentioned and every sentence would be followed by a newline. This raised public awareness of reinforcing the AI with WI. A simple prose format like this can get the AI to be mindful of most attributes assigned to said character or object. But it tends to be heavy on used-up characters and especially Griffin might start mixing up actors from different WI together, now called leaking.

Before we go further let's discuss a common, yet counterintuitive pitfall of WI use. That is defining the main character or `you` in keys and ultimately the WI entry. This makes sense in short scenarios focused on the main character and a small group of characters that have one-on-one interactions. But if we use many naive WI keys in a longer fantasy adventure with many unique characters, we notice the AI paying less attention to the information you've given to each entity inside WI.

This is due to WI overloading going by the rules we discussed at the beginning of the document. When simple keys like `you` are used they are active almost all the time, taking up precious space which is restricted to roughly 1386 characters going by the rules. If we use a fully stacked A/N and WI entry for you, we've used up 650 of the possible 2772 maximum. When you have WIs for the location and multiple characters, these space limits get eaten up fast, leading the AI to ignore some of your WIs. Enthusiasts generally avoid keys that are active too often for this reason. They are best used as a strong reminder when needed, the shorter the better. 

It is often preferred to define the character `you` towards the bottom of the Remember pin as that gives you the fullest amount of control where those details get inserted. No best practice exists for this type of WI, so it is left to the reader to deliberate on what their needs for AID are.

The author advises against flooding the world with more format names. At best it's a waste of time and confusing at worst it's the harmful and misleading kind of confusing. Originally formats were a quick way to reference different directions of WI research. Now the different methods are starting to converge back as the enthusiasts are attempting to optimize their personal stacks and styles. Formatting is a much more accurate term and beneficial to everyone in the community without excluding any form of manipulating language. This document hopes to demonstrate this.

Fun fact: `format:` works extremely well inside Author's Notes as a keyword/category you can use to tell the AI whether you're writing a script or a sonnet or a manifesto, etc.


### JSON Formatting
WI can be exported and imported out of AID as JSON files. What we discuss here is something different. Interest in WI formats blew up after people started publicly sharing anecdotes of a JSON-like WI format having great results in August 2020. Many people did private research at the time but Zaltys 🐍#5362 is often credited with raising public interest. Here we define `the pure JSON format`:
```
keys: AKA: you,Anon
Entry: [{"You":{"name":"Anon","age":24,"gender":"male","species":["human","man"],"eyes":{"eye-color":"brown"},"hair":{"hair-color":"dark brown","hair-length":"intermediate","hair-style":"spiky"},"relationships":{"orphan":"no relatives"},"body":{"physique":"average","height":"average-178cm","weight":"average-80kg"},"personality":["sarcastic","friendly","wishes he had a gf","plays AI dungeon"]}}]
```
<details>
  <summary>Spoiler: Expanded version</summary>
  
The same entry, with tab indents (don't put it into WI entry, it's here just for readability):

```
[{
	"You": {
		"name": "Anon", 
		"age": 24, 
		"gender": "male", 
		"species": [
			"human", 
			"man"
		], 
		"eyes": {
			"eye-color": "brown"
		}, 
		"hair": {
			"hair-color": "dark brown", 
			"hair-length": "intermediate", 
			"hair-style": "spiky"
		}, 
		"relationships": {
			"orphan": "no relatives"
		}, 
		"body": {
			"physique": "average", 
			"height": "average-178cm", 
			"weight": "average-80kg"
		}, 
		"personality": [
			"sarcastic", 
			"friendly", 
			"wishes he had a gf", 
			"plays AI dungeon"
		]
	}
}]
```

</details>

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
<!--+ Has online production tools like https://aid-world-builder.ey.r.appspot.com (thanks uusu) -->

JSON had its uses. If you're already using it feel free to continue doing so. If it looks frightening, don't feel obliged to invest learning it.<!-- Uusu's world builder still exists to easily write JSON-formatted WI online.--> Most WI users have moved on to using other methods specialized for their purposes. 


### Zaltys Formatting
Zaltys 🐍#5362 has popularized many interesting formatting methods and spurred the community to try many different things out with the tools AID provides us. Because originally there were few enthusiasts trying unique tricks with the world entry screen, Zaltys distinct style of formatting text became simply known as Zaltys and was adopted so people immediately knew what was being discussed. This document will be kept up to date with the most recommended format variants of Zaltys with some explanation of his goals and motivations. First we have standard Zaltys that works with any model.
```
Mike Haggar:[Human<male, 202cm, 140kg>. APPEARANCE<Haggar>:Stocky, muscular, big arms, hair<brown>, mustache<brown>; MENTAL<Haggar>: Just, upright, direct, upbeat; WORN:Eyeglasses, pants:<green>; TRAITS<Haggar>:Born in 1943, from Final Fight and Street Fighter games, ex prowrestler, grew up on streets in Metro City, mayor of Metro City, fought Mad Gear & Skull Cross gangs, fights gangs, "It's my job to keep Metro City safe!", loves curry, friends:<Cody, Guy>, daughter:<Jessica>.]
```

+ This format must have a . near the end to work correctly. Do not use extra periods within categories, not even in quotes.
+  '<color>' is needed to make colors work. (Jan 2021 change.)
+ Avoid using -s. (Jan 2021 change.)
+ Categories must be typed in UPPER CAPS, with the exact spelling.
+ Including the name after the categories helps Griffin parse the entry.
+ Other useful categories include `MOV` (for movement-types, such as 'slithering'), `GRAB` (for object manipulation, such as 'talons', 'beak', or 'coiling'), `ORIGIN` (for birthplace, game of origin, etc).
+ If you need to add age, use age<num> in same section as weight/height.
+ All of it goes into single WI entry with no line breaks, under a keyword such as haggar.

We also have an alternate advanced version specifically designed for Griffin. It saves space inside the WI and context, but is both harder to read and write.
```
Mike Haggar:[Human<male/202cm/140kg>;APPE<Haggar>:Stocky&muscular/bigarms/brown<shorthair&mustache>;MENT<Haggar>:Just/direct/upbeat/hardy/upright;WORN:Eyeglasses<at work>/brownboots&greenpants/bandolier;TRA<Haggar>:Born 1943/from:<Final Fight&Street Fighter> games/ex:prowrestler/grew up on streets<in Metro City>/mayor<Metro City>/fought gangs:<Mad Gear&Skull Cross>/fights gangs/"It's my job to keep Metro City safe!"/loves:curry&rice/friends:<Cody&Guy&Carlos:from Brazil>/daughter:<Jessica>.]
```

Zaltys combines most WI writing tricks the community has discovered into one very efficient package. One caveat would be how strictly you have to follow the pattern to guarantee functionality.

Based on experiments and accumulating experience the community has found all-caps categories are useful. Later on in the document we discuss categories in depth and include a long list of them that have been discovered to be useful for any WI format desiring to utilize them.

Zaltys likes to write a lot of species from different series. He has written on discord that his most used categories are APPEAR, LACK, GRAB, MOV, MENTAL and TRAITS. For defining a type of character all these have purpose. Appear is self explanatory. Lack has to do what the characters can and can't do; a lamia without legs shouldn't be able to walk. Grab is their preferred way of interacting through appendages, a bird would use it's beak or talons, a lamia would use their hands or tail. Mov works for movement. Lamia would crawl/slither on top of their tails, slimes ooze and other species may do more imaginative things. Mental is traits of the mind, traits is more intrinsic physical, sensory or nature related traits. Traits has proven to be the Swiss army knife of categories after months of use. INV and EQUIP work similarly to one another as alternatives to WORN and can be used to define the types of equipment a RPG character would carry on top of the usual stuff like clothes. WORN is better at handling style like `goth` clothing. `gothic` is more applicable to architecture. 

The AI is good at overwriting preset associations. Following Zaltys examples the community has succeeded in writing interesting recipes. For some reason the AI seems very good at describing recipes. This is another example of an indicator helping the AI understand WI better. Example from Mono:
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

Zaltys version of defining `you` or the main character inside Remember:
```
You are male human named Zaltys.
[ you.DESC: adult, gamer;
you.INV: wallet, keys<in pocket>;
you.MIND: confused;
you.LOCATION: floating in outer space.]
```
Result after a 2K character context:
```
> You think about your current situation.
You are most definitely NOT in Kansas anymore.
The last thing you remember, you were laying down to take a nap during a break from work. Now you find yourself floating in darkness.
Actually it's more like a darkness with stars. You look around and see the entire night sky around you. You don't feel the ground beneath you or anything else for that matter.
You try to cry out, but no sound comes out.
In the distance you see Earth hanging in space like a bright blue and green ball.

> You check your pockets.
You pat yourself down and realize you still have all of your possessions, even your wallet and keys.
```


### Useful Categories
With formatting methods like JSON, pseudo-JSON, Zaltys and its relatives we've noticed a capitalized `CATEGORY:` followed by a list of attributes is very effective and now in common use. Here we share some categories that have proven useful to many WI enthusiasts. 

There are three types of category: Propercase, lowercase and UPPERCASE. Any of them may be used and is down to user preference. Due to the history of WI research we focus on the UPPERCASE category. UPPERCASE is good because it clearly distinguishes the category word from the list associated with it or any other formatting you may use. UPPERCASE tokenizes differently from categories in other cases and should be tested with the tokenizer especially when trying to incorporate fresh categories the writer came up with themselves. It has been found that if a unique word contains tokens of a shorthand, that shorthand tends to work very effectively e.g. `APPEARANCE && APPEAR` and `SUMMARY && SUMM`.

Here we have a picture of some common UPPERCASE category basis and their tokenization: https://files.catbox.moe/avn9j4.png

The above tokenization is the reason Zaltys and other enthusiasts ended up picking `APPEAR`, `TRAITS`, `WORN`, `EQUIP`, `MENTAL` and their even shorter forms. If a WI enthusiast tells you that a category is bad, it's probably because they already tried it themselves, looked at the tokens and the results weren't good with evidence to support this.

Different authors prefer using different cases for categorization. In the past birb and Zaltys recommended you use uppercase categories. Citation from Zaltys on why we use uppercase categories:
```
Problem with propercase categories is that if you load several WIs, the AI will be hesitant to use those words (because of the repetition penalty). The uppercase avoids that, and also makes it less likely that the AI starts copying the style into output. Not much of a problem for 'Traits' since that's unlikely to appear in the output anyway, but might be a problem for 'relationship' or 'wear*'.
```
Based on experimentation there may be some exceptions. For some scenarios tested where the AI was tricked into formatting its own writing `writingstyle:` was seemingly preferred and produced better output than `Writing Style:`. Both work for use with A/N and may come down to user preference.

UPPERCASE category keywords will be provided as a list. Their use is mostly self-explanatory. Some interesting categories will receive further attention. We recommend experimentation, but these words have been chosen because they felt more effective than other synonyms the Discord tested. Based on experience we have the following recommendation: plural forms are preferred for noun categories e.g. `HOBBIES`, `POWERS` whereas for verb categories its preferred to skip the `s` e.g. `APPEAR`, `LACK`. 

On further play-testing the preferred hypothesis is that everything comes down to tokenization. Let's explore variants of the word effect. In tokens `EFFECT` is EFF + ECT and `EFFECTS` is EFF+ EC + TS. The latter combo works, the former is very weak. `EFFEC` is EFF + EC and seems to work even better than the aforementioned. For whatever reason for GPT-3 2 token combinations usually work best. `POWER` is single token, `POWERS` is POW + ERS, the 2 token POWERS tends to work better. Most of the chosen categories are based on user reports and this hypothesis.

The long list of recommended categories:
```
APPEAR, WORN/WEAR, INV, EQUIP, MIND, SUMMARY, LIKES, HATE, RELATIONS, FRIENDS, ENEMIES, TALENTS, HOBBIES, POWERS, THEME, ORIGIN (for series/titles), ATMOSPHERE, TONE, MOOD, CLIMATE, GEOGRAPHY, FEATURES (for locations), EXIT (for rooms), CITIZENS, CENSUS, PASSION, BONDS, ALIGNMENT, EFFEC(for objects and entities, used to add magic or science effects), SEE, BIOME, FLORA (must match the biome), CREATE, LIMIT, FACTS, LOOT, BANNED (forbidden behavior), COND (status condition like PRLZ from Pokemon), DIET 
```
Problematic categories (avoid or use with caution):
```
BODY(only good for humanoids), LOOKS(for humanoids, has multiple meanings), LACK(superseded), STATE/STATUS(superceded, multiple meanings)
```

### Categories Table

Here's a table of the forementioned categories (see below why some alternatives are undesired).
The categories marked with *`ITALIC`* aren't exactly perfect for all cases (again, see below: it might be better, worse and even both).


| Yes | Alt./Short | No/Superseded | Comment |
| --- | --- | --- | --- |
| [SUMMARY](#traits-and-summary) | `SUMM` | `TRAITS` | Swiss army knife category, for anything that doesn't have it's own cat. [See also this](#more-on-summary) about summary. |
| [MIND](#mental) |  | `MENTAL` |  |
| [APPEAR](#appear) | `APPE`<br>*`PHYSIQ`, `FORMS`* | `BODY`, `LOOKS` |  |
| [WORN](#worn) | `WEAR`<br>*`CLOTHES`, `CLOTHING`, `DRESS`, `COSTUME`* | `OUTFIT`, `SUIT`, `ATTIRE`, `WARDROBE` |  |
| [DESC](#desc) |  |  | for something that is a part of appearance but it's functionality is more important than the looks |
| [SEE](#see) |  |  | used to describe how a character perceives the world |
| *[LIMIT](#lack)* | *`LACK`* |  |  |
| [COND](#cond) |  | *[STATE / STATUS](#status)* | status condition like `PSN` / `PRLZ` from Pokemon |
| [DIET](#diet) |  |  |  |
| [LOOT](#loot) |  |  | what `INV` items can be dropped when a creature is defeated |
| [DETA](#detail) | `DETAIL` |  |  |
| INV |  |  |  |
| EQUIP |  |  |  |
| LIKES |  |  |  |
| HATE |  |  |  |
| RELATIONS |  |  |  |
| FRIENDS |  |  |  |
| ENEMIES |  |  |  |
| TALENTS |  |  |  |
| HOBBIES |  |  |  |
| POWERS |  |  |  |
| THEME |  |  |  |
| ORIGIN |  |  | for series/titles |
| ATMOSPHERE |  |  |  |
| TONE |  |  |  |
| MOOD |  |  |  |
| CLIMATE | `CLIM` |  |  |
| GEOGRAPHY | `GEO`, `GEOGR` |  |  |
| FEATURES |  |  | for locations |
| EXIT |  |  | for rooms |
| CITIZENS | `CITIZ` |  |  |
| CENSUS |  |  |  |
| PASSION |  |  |  |
| BONDS |  |  |  |
| ALIGNMENT | `ALIGN` |  |  |
| EFFEC |  |  | for objects and entities, used to add magic or science effects |
| BIOME |  |  |  |
| FLORA |  |  | must match the biome |
| CREATE |  |  |  |
| FACTS |  |  |  |
| BANNED | `BANN` |  | forbidden behavior |


Categories can be shortened to save characters. The effectiveness of this is based on tokenization and following up with relevant traits. Not all short-hands work due to the AI mixing it up with other words with different meanings. Some examples: `APPE`, `MENT`, `RAITS`, `SUMM`, `CLIM`, `GEO`/`GEOGR`, `CITIZ`, `PERSONA`, `ALIGN`, `BANN`. Sometimes the shorthand might be preferred such as `CONDITION` vs `COND`.

#### `TRAITS` and `SUMMARY`
Let's discuss `TRAITS` and `SUMMARY` first. Due to Zaltys popularized use of categories, `TRAITS` was used as a universal category for a long time. The exact effect of `TRAITS` is unknown, but it possible to throw essentially any intrinsic characteristic trait inside it and have the AI respect it. It's still a good category for characteristics lacking specific category, but testing of WI has since continued. 

Users of the world-info channel have been playing around with `SUMMARY` or `SUMM` for short since it was discovered and found it works near identically to `TRAITS`, except it feels even more versatile. You can put pretty much anything in it that'd fit in a character 'summary' and it combines great with other categories, even referencing them without causing leakage. Since they achieve the same thing, but `SUMM` is preferred by everyone we've talked to so far it is the recommendation for a universal swiss army knife category.

Also, uncommon or complicated concepts like fauna do not work well as categories. `SUMM: Fauna<species>` works better.

#### `MENTAL`
is the category that was in popular use for mental, personality and behavioral traits. `MIND` achieves the same goal more effectively while using less space so `MIND` is recommended for behavioral traits now. Consult the [Personality Keywords](#personality-keywords) segment for more information on behavior.

The AI tends to be very literal-minded about certain things. The associative power of a category is one of its advantages. For example with `TRAITS`, the AI treated `cool` as the nature of a thing. Entities with the `TRAIT` `cool` tended to radiate coldness. So to have characters *behave* in a cool manner, the trait should go inside `MENTAL` or `MIND`. This may be the case for other words like `hotheaded` that the AI could take too literally.

#### `APPEAR:`
is typically used for appearances and shortened to `APPE:`. `PHYSIQ:` and `FORMS:` are good alternatives although their unique uses are unknown as of writing.

#### `WORN:`
is and remains the most used category for describing worn attire. Testing suggests `WEAR:` works just as well, while tokenizing better regardless of location or preceding characters. There are many clothing related categories that have been tried and experimented on. `CLOTHES:` is an effective, but dry and mechanical alternative. During testing many users found that `DRESS:` works similarly well, but sacrifices some accuracy for more creative prose. Provided is a picture from Zaltys testing and his suggestions.  
https://files.catbox.moe/0ur88f.PNG

#### `DESC:`
sees some use. It relates to the physical functionality of an entity and may be more useful than `APPEAR:` for specific cases where the functionality is more important than the looks of a thing. `DESC:` might as well as be used for describing the functional limbs of a huge insect or to describe the mechanisms and staff inside a spaceship. Testing from many enthusiasts also suggests that with `DESC:` and optionally prose descriptions at the front, the AI recognizes and uses other categories better.

#### `SEE:`
is the most interesting sense specified as a category. Using it you can get characters to do interesting things like 'see' the world through vibrations. Using sounds in SEE does interesting things, but doesn't produce synesthesia consistently, for that you need `SUMM`. Other senses have been tried, but haven't demonstrated impressive results. Combining `LACK:eyes;SEE:blind;` has produced the most consistent Zaltys formatting blind characters. Research continues into creating mutes (now featured in a separate miscellaneous doc).

#### `LACK:`
is a category that has often been used to denote something is missing with mediocre results. `LIMIT:` has proven more powerful for this use with some caveats. `LIMIT:` seems to get treated like a mathematical term and denotes a discrete limit for something. `LIMIT: zero_arms` could be used to key the AI on the fact the character has zero arms while you could just as easily use the category to convince the AI the character is supposed to have `LIMIT: four_arms`. `LIMIT:` doesn't work for hard-to-quantify things like attributes (or the amount of) associated with things like `skin`.

#### `STATUS:`
doesn't necessarily work as expected. It has a decent association with things like `STATUS:broken leg` or other conditions you'd generally see in RPGs or CYOAs. But it can also be used to associate a character with something. `STATUS: from Pokemon` makes the AI often mention the character is somehow associated with the world of Pokemon. Still it's not a completely consistent category for this purpose despite the interesting results. 

#### `COND:`
is much more interesting and powerful for actually having characters suffer from conditions or status effects. It has been tested working with injuries and other debilitations. It even works with game and Pokemon status conditions like `PSN` or `PRLZ`.
```
COND:sick/dying/poisoned
"I'm... not sure what help I'll be, sir. I'm dying. I was poisoned by a snake in the garden."
COND:PRLZ
"I cannot. I am paralyzed from the waist down."
```

#### `DIET:`
can be used to make unusual things like herbivorous snakes. It's a powerful category, because AID has a lot of training data on snakes being carnivores.

#### `LOOT:`
works wonderfully for monster hunting or fighting scenarios, very often giving the listed item(s) after the creature in question dies. This could be combined with the probability bots that the devs of AID are working on.

#### `DETAIL:`
shortened to `DETA:` works fine with just about anything related to details, but it's unknown how this is different from summary.

#### More on `SUMMARY:`
has already been pointed out as a strong and versatile category for defining points of interest you want described consistently. It can even reference previously used categories, meaning it has abilities the older `TRAITS:` lacked. Impressive examples below:
```
SUMMARY:Jamaican/grew up on streets, Output:
He grew up in Jamaica with his rich parents. When he was 16, he was on vacation in Hawaii and decided to stay there when his parents left without him. He learned how to surf and just bummed around the island for a while. He eventually ran out of money, so he started working at a surf shop and selling some of his paintings on the side.
```
```
Zak:[sentient continent<gigantic>. MENT<Zak>:upbeat/cool/loyal;SUMMARY<Zak>:Sentient landmass/living continent/mostly forest&taiga/can float.] Output:

Zak appears as a massive continent with a thick green forest canopy covering most of it. The bright sun shines through the leaves creating dancing lights that brighten up the otherwise dark green expanse. It is a beautiful, massive thing drifting in the sky, mostly covered in trees and plains with mountains here and there.
Zak is the only sentient landmass in Raajhanaa that is not a nation. That's because it is a living being, and capable of controlled flight, though it is currently limited to within the bounds of the Raajhanaan sky. It is quite ancient, and it is unknown how it came to be sentient, though there are many myths and legends surrounding it.
Its anatomy is as varied as the landscape, and just like a regular body, different parts of it have different specialised organs that allow it to survive.
PERSONALITY (Zak):
Zak is just an overall calm, cool and collected being. Doesn't really get angry at things, though it has a healthy respect for gravity (no pun intended). It loves exploring, seeing new sights and visiting foreign places.
```
With `SUMM`:
```
Zak is a sentient floating continent. It is about the size of Eurasia, and can fly around at will, although it cannot currently fly very high or very fast. It is mostly covered in forests and taiga, although it has some large plains, and several mountain ranges. It is currently inhabited by a few tribes of stone age humans.
```

If you're lost and in doubt what category to use, you can probably throw it inside `SUMM` and AID will handle it fine. Not all details need their own category. `RACE`, `GENDER`, `AGE` can be condensed to `elf/female/25y` then followed by the bigger categories. The AI picks up common traits just fine by their lonesome. Zaltys-style formatting also benefits from having an indicator phrase like the one mentioned above.

The AI understands the concept of speech patterns and accent to some degree. Categories like `SPEECH:`, `WORDING:` and `ACCENT:` were tried, but didn't output desirable results.

Thus far `TRAITS:` (or now `SUMM`) has proven to handle this attribute the best. Writing style theme in author's notes can also give characters accent across the entire setting. Known working accents: pirates, sailors, Shakespearean accent, archaic, Cockney accent, valley girl (valley girl is the best method for getting a Southern accent on both sexes), hillbilly, Jamaica, crook/criminal. 

Below is an example of  giving a character an accent with TRAITS:  
```
Zack:[Male human. TRAITS: surfer, heavy accent<Jamaica>.]
QUOTES FROM ZACK:
"Zis is a really bad time to be messin wit me mon!"
"The waves, the wind and the sky, zey are all one like you and I, man."
"I hope dat you don't mind some reggae music, man."
```
To do categories: `ADJ`, `GRAM`, `MODIF`


### Personality Keywords
Character behaviors and personalities deserve their own category. One of the biggest goals of WI is getting the AI to remember **how** a character is and output their behavior consistently. To this end we need behavioral keywords. This section is entirely dedicated to personality-related keywords that the community has either discovered to work exceptionally well towards certain ends or keywords that are disappointingly weak.

For category-based formatting `MENTAL:` used to be the most commonly used keyword for listing out a character's mentality, personality and behavior. Recently we found `MIND:` works better towards this end in all aspects while also being a shorter word.

First we mention the special cases that you may want to avoid due to tokenization issues. AID has been known to be notoriously bad at negatives. For example the community has known for a long time that `White hates apples` is a better alternative to `White dislikes apples` as the AI doesn't seem to respect the intent of the latter statement at all. Words starting with dis- or un- rarely get respected by the AI due to tokens and how bad it's with negative prefixes. -less is a suffix that works somewhat better, but isn't recommended if you have synonyms known to be more explicit or to have unique tokens.

Long and growing list of behavioral keywords is provided in no particular order. There are too many to list, these are just some common useful ones as well as some commonly tried keywords you might want to avoid. The basis of a good personality trait is something strong and focused the AI has a strong unique association for. Broad words like good or evil aren't very useful, when the behavior is what we are after. 

#### Bad guy / `evil` / `cruel` / `sadistic`
`cold-blooded, psychopathic, mad, or ruthless` are all weak. The character may smile or act friendly, behaving more like an IRL sociopath might. `Bloodlust, crazy` are weak too, they don't create the degree of havoc you'd expect. `cruel, sadistic` are great, these tend to make the characters into violent sociopaths. `stabby` is a cool comedic trait in this category, it makes the character speak in stab puns without actually stabbing other characters more. `cruel` is good for creating violent sociopaths too. `brutal, dangerous` are effective evil sociopath traits. With these and other flavor traits you have your evil violent character. `lunatic` doesn't have much research but may be a better trait for insanity, `maniac` is good, but makes characters very energetic or bipolar. Or rarely it turns them into otaku figurine collectors.

Continuing with the bad guy theme the seven sins all work. `glutton(ous), lust(ful), greedy/avaritious, envious, slothful, proud` all work. Wrath is the exception and doesn't produce satisfying results, but can easily be replaced with `angry&cruel`. Based on research with the angriest wizard that ever lived, anger related tags especially `the angriest` are effective.

#### Paladin / lawful good
`chivalric, just, benevolent` work for creating paladin- or white knight types. These may be combined with more morally ambiguous or gray traits like `sour` to create a character that isn't as pristine or stereotypically lawful good. According to Zaltys using a combination of `MIND:chivalric&noble just knightly` might even make these behavioral traits too strong for some tastes.

#### Manly / `serious`
Then there's `stoic, serious, calm` for more brooding or detached traditionally masculine types. Of course these may be used just as well to create a dour experienced femme. `professional` creates the polite valet/waiter-type character. This would be good for a lawful evil type character too.

`horny, stoic, apathetic, wry, clever, witty, addicted` all do exactly what you'd expect out of them, they work good. `teasing, joking` are both good and even better when put together. There isn't much to say about mental keywords that have the intended effect. `judgmental` turns a character into a judgmental... prick. It is a very effective keyword for making an 'asshole' character. 

#### Other assorted useful traits
include `timid, selfconscious, polite, friendly, seductive, arrogant, oblivious, dumb, dry, cynical, motherly&generous` each doing exactly what you'd expect. `motherly` is best combined tightly with a word like `generous`, because it has a higher chance of turning the character into an actual or expecting mother.

The combination of `anxious, nervous` with `SUMM: "h-he...llo"` is very interesting, it's one of the cutest and most appealing ways of creating a timid character that stutters a lot in dialogue. 

Below is an example of AID taking a large selection of `MIND` keywords and producing something very interesting out of them:
```
Zak:[Human male. MIND<Zak>:secretive/dangerous/manipulative/stoic/callous/servile/polite.]
Zak is a middle-age, balding man with brown hair and dark eyes. He wears a black overcoat and small, wire-rimmed glasses. He is of average height and build.
Zak has a very serious look on his face most of the time and he rarely, if ever, smiles. His eyes are constantly darting around, as if he mistrusts everything and everyone. Despite his serious demeanor, he is very polite, although in a very formal, stoic way. He always speaks in a very respectful tone, even to those who are not as educated as he is or when talking about something distasteful.
```
You can also consult the [Author's Note research justpaste.it](#reminder) for more tips. Some of the keywords tailor the AI's output too much as a note, but work great as personality traits.


### Monky Formatting
Optimization and consistency are major goals for WI-experimenters. Monky was one of the first users to focus on this and his solution to many issues with WI is what he calls the `Caveman` format. Looking at an example will show us why:
```
keys:Chagra, initiate, apprentice
Chagra AKA 'Tusk': Orc she 20y.
Chagra 220cm ht. 100kg hardy.
Chagra green scar skin. Long brown hair.
Chagra loyal careful outcast pranked Healing magic initiate coven.
Chagra Hergea's daughter Friends You Renni.
Chagra likes fruit veggies Hates meat cheese.
```
In this WI all words lacking semantic value (content meaning) have been removed. Still, testing has AI mention all the relevant details with very low risk of leaking into other things defined with WI. It is likely that the brutal efficiency of AI doesn't care about words like `is`. Users of the format feel that it sticks to the given information better than the base AI alone or untested prose. And so far no one has reported that the AI starts speaking caveman when using this. If issues like this do occur they can always be solved with alter or retry. You should use highly descriptive inputs or an A/N: (now free with scripting) to make the AI write descriptive prose anyways. Worth noting is that Monky never used commas when writing his style of Caveman.

Colors are one detail the WI has frequent problems getting right. Monky claims that he gets good results on caveman for results just stating the color and what it is for. A verbose example of monky defining adjectives to describe his character (typically the main character) is as follows `Alisha long dark brown silky shiny wavy hair.`

There are two good resources for caveman, Monky's test scenario and kim's caveman generator, both linked in order:
https://play.aidungeon.io/main/scenarioView?publicId=558a8d40-2fbd-11eb-9239-8b8f17f7a2b0  
https://play.aidungeon.io/main/scenarioView?publicId=3f094990-2e40-11eb-b81d-a3d32aaa3e7a

During experimentation on caveman Monky made important observations. When he used the caveman format extensively on Griffin he found if he didn't mention the character/key every 50 characters/15 tokens the AI would 'lose focus' and start describing random attributes outside of the WI. On Dragon the limit before he needed to start mentioning the key again was around 200 characters. Discoveries like these have led some users to believe the key should be reinforced at the specified intervals depending on which model you use.

TL;DR A maxed out Griffin Caveman WI should be ~10 50-character lines long, each line starting with the key. On Dragon this could be 2-3 lines each maximum 200 characters. It should also be noted that Monky formats achieve some of the best character/token ratios if you're interested in that.

Caveman is perhaps the most forgiving basis for a format. It plays well with many tricks discovered later like mashing up words or pointing to them with `<>`. Monky has developed a second format based off Caveman. This is affectionately called `the Neanderthal Format` or just plain Neanderthal. Monky himself has authored a thorough write-up on the format and it's merits which is provided in this repository as a supplementary material, much thanks for his hard work. Below is a sample Neanderthal WI.
```
 Alexandria:[ Age 29 human she tall strong thin brave knight, your ally]. 
 Alexandria:[ Has Blackflame rune sword, steel armor, healing potion, amulet].
 Alexandria:[ Long red hair, fair skin, narrow green eyes, clean].
```
It clearly still looks like caveman, but now including commas, symbols and implementing enclosure tricks first seen with Zaltys. Some important observations follow. Every newline and every sentence inside brackets starts with an empty whitespace. This is done to guarantee proper tokenization of the word. The `.` follows the brackets due to the model's natural writing bias. Since it looks more like prose, commas are allowed. On Griffin sentences inside brackets are no longer than 50 characters/15 tokens to maintain focus. 

Below is the full dissection of Neanderthal written by Monky:  
https://github.com/valahraban/AID-World-Info-research-sheet/blob/main/docs/Neanderthal_Unscripted_Results_by_Monky.txt  
Users should write using any format they enjoy but by now Neanderthal is greatly recommended in part due to how easy it is to integrate other methods with it. With birb's observations some issues with EWIJSON were resolved and now Neanderthal is a format that plays very nicely with it.

Neanderthal style formatting has gone through even further tweaking so it may be effectively adapted for use with scripts like EWIJSON. Since we've gone from writing like a caveman to writing more like a modern person to writing like a scripter the modified syntax is logically called Futureman. It is simply a modification of Neanderthal that is extremely compatible with EWIJSON which has many benefits over vanilla scriptless AID. Futureman uses `<<` and `>>>>` for encapsulation, both of these using only 1 token. Below is a simple example of Alexandria converted into Futureman and used together with EWIJSON.
```keys:Alexandria#[t=1]
<<  Alexandria age 29 human she tall strong thin brave knight, your ally>>>>
```
Monky has written and provided (with some editorial assistance from birb) a thorough writeup on the history of the formatting he uses and what Futureman is all about. It is extremely recommended reading.  
https://github.com/valahraban/AID-World-Info-research-sheet/blob/main/docs/Futureman_by_monky.md


### Onyx Formatting Tricks
Onyx has come up with interesting and reliable tricks for specialized uses. With RND we can actually have pseudo-random lists. Testing so far suggests with less than 10 members the list usually stays consistent, more members or simple words make it output more random, but still conceptually related things. This works better on Dragon, but the concept can be used to invoke lists of names on Griffin. Updated January 2021. If any example has `()` replace them with `<>`.
```
keys: monsters, forest
Many monsters live in the forest. [RND<monsters>: slime, goblin, orc, Ent, giant carnivorous sloth, pixie, beholder.]
OR
keys: goal, objective, quest
RND<objectives>: slay the dragon, rescue the princess, challenge the demon lord
```
Another one is `IF<action>: THEN<things happen>` this is especially useful for creating custom spells or other conditional effects like the format suggests. 
```
keys: cursed book,open cursed book
IF<open cursed book>: THEN<the pages of the cursed book form into a face, shriek loudly and place a terrible curse on you.>
```
With the discovery of logic algorithms we can improve on this. A =>B essentially means A leads into B or A implies B. This is a form of IF THEN.
```
IF{open cursed book} =>THEN{The pages of the cursed book form into a face, shriek loudly and place a terrible curse on you.}
```

From his work we have a reliable and easy way to define locales especially when combined with data mentioned above. These get interesting when combined with other short WI.
```
keys: Rask
Rask<kingdom>: TERRAIN: tundra<full of ogres and giants>, fertile land<southern>. FEATURES: Wolfsholme<capital, walled city>, primitive gunpowder weapons. CLIMATE: cold, snowy. RESOURCES: livestock, horses, furs, lumber. CITIZENS: humans, elves, giants, hardy, fierce, wield warhammers and axes, worship Nahr. RULER: Queen Cassandra.
```
From further testing the community has discovered that defining locales like this may benefit from placing each category on a newline. Both methods are effective. 

One day during just playing the game like regular Onyx happened upon impressive categories and keywords that could be used as input or inside Author's Notes to direct the AI:
```
Writing style (deaf, visual descriptions): [The sunlight was streaming from the sky and on to his head. He had a surfboard under his arm. His nose was like a little ski slope for the snow to go down. His eyes were blue, like a crystal clear pool of water. The sand felt warm and soft beneath his feet.
```
```
Writing style (blind, auditory descriptions): [Sounds came to his ears. The pounding of the surf. A seabird crying overhead. The warm caress of the sun on his skin. The tingling taste of salt in the air. The smell of fish and honey. The feel of wet sand slipping between his toes. The sound of a gentle breeze blowing through his hair. The sheen of those scales, like pearl in the sun. The roughness of that scaly skin, like the bark of an ancient redwood tree.
```
Visual and auditory descriptions seem to be the important part. The output is fancy enough to definitely consider as their own lines at the bottom of remember or integrated into A/N.

Zaltys has also made discoveries on how to semi-reliably define the structure of a building. These are listed with Onyx as an expansion of his concept. As a category EXIT works better than EXITS. Directions are good categories, but specifically for architecture.
```
keys: library
Library room: large room. EXIT: corridor<north>, lounge<west>, trophy room<southeast>. FEATURES: Bookshelves<dusty tomes, esoteric, mythology, grimoires>, man-eating plant<huge>, spiral staircase<to second floor>, chandelier<creepy>.
```


### birb research
Many sources confirm that newlines (pressing enter) is powerful at grouping and separating certain traits. ALL CAPS is good for defining the group, encapsulation like ~~() is good~~ used to be for the reference point. Thanks to monky caveman we know the AI doesn't care about grammatical correctness in the WI either. Sometimes `red-color` or `hair-color` may refuse to work consistently. In an attempt to fix this the community discovered that AI groups words strongly together, yet knows to separate them if you smash them together into compound words even when grammatically incorrect. For example:`verylonghair`,`platinumblondehair`, `tatteredgrayrobes`. birb calls this smashing words the more correct term being mashup. With this discovery even traits like `flamingorangeeyes` are possible to get out consistently. birb is still experimenting and undergoing revision on both of his formats, because while preliminary results were good, they broke on long scenarios. Also note that the following format is basically Zaltys with newlines and commas (and predates catnip). The angriest wizard that ever lived is provided for historic purposes (now outdated due to `()` being weaker than before replace with `<>`):
```
Original date: October 12 2020  
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
Smashing words together is efficient but there is **ALWAYS** a risk of the AI misinterpreting the words depending on tokenization when you try this. For example `catears` gets tokenized as ca|tear|s. One caveat of the format birb used is high token use. A "worst case scenario" WI that used many Japanese words and symbols produced 387 characters, 160 tokens so 2.42 ch/tk. Smashed words may be used with any format even Caveman (potentially leading to the greatest character and token savings). If you encounter the AI shortening names excessively while using Caveman, you can start each newline with an empty whitespace ` ` to change how the name tokenizes. For example `Vladimir` tokenizes as V|lad|imir but ` Vladimir` is all a single token.

During testing we discovered characters can be forced to wear certain outfits with complete consistency. This format needs at least 2 WI the character being dressed and the custom outfit. This theory could be applied for customizing the appearance of any object in your game world. Example.
```
Lillie:[Human female. Her eyes are blue. Her hair is blonde. Her hairstyle is long twintails. WORN:<Nightfall Raiment>.]
```
```
[The Nightfall Raiment is a onepiece halterneck dress. The dress is in gothic fashion. The Raiment has a black and white colorscheme. The Raiment has detached white sleeves and shows off the wearer's armpits. The bust and frills of the relatively long nightfall raiment dress are white while the rest of it is black. Kneehigh black leather boots are a part of the raiment.]
```
When you examine Lillie in the story and have the AI describe what she is wearing you will get an output like the following very consistently: `She's wearing a nightfall raiment. The dress consists of detached white sleeves and shows the armpits. The bust and the frills of the dress are white while the rest is black. Kneehigh boots are a part of the outfit.` When tested on other outfits `the clothing consists of` was a very good input to continue from for the story. This was demonstrated on Discord. The above example combines almost-Caveman with Zaltys. If you use a style like this, you must make it conform to your writing style. Zaltys calls the uncategorized part of his format the indicator. This is usually details like sex species and age. In the Lillie example species, sex and colors were indicators. birb always wants the AI to describe the traits of the characters in the format `Her eyes are X color. Her hair is Y color in Z style.` This is why this type of WI works for his scenarios. Also, if the AI starts describing the clothing of the character before you get to ask what the character is wearing, it will usually output the wrong set of clothes. First the story must mention the name of the outfit, then it will use the correct WI to describe the clothes.
The document now provides evidence for certain methods working. Included is a gif of the outfit being in use. You will have to take the author's word that the AI can consistently mention the outfit and then describe it.
https://files.catbox.moe/9dibjo.gif

For a period of time birb also used formatting commonly referred to as Ritz as she was the character he first posted an example of. This was abandoned as a historic curiosity after a 300k character long adventure. If requested, more examples may be provided in the obsoleted methods category.

Now that the category `EQUIP:` is confirmed working with clothes and other inventory it would be possible to use the above example to come up with custom named weapons for your characters, write WI for the custom weapons and have the AI invoke their traits reliably.


### CrisAI research
CrisAIcilian is a user from discord who has been developing methods for manipulating context, mostly for the purposes of maintaining consistency and staying on scene even with a high randomness value of 1.3. Now his research is robust and long enough to get it's own section. CrisAIcilian has discovered `Character sheet - ` as an effective header for WI, using an otherwise Zaltys-like format together with it in his test character. After roughly 100 rolls he claimed to achieve a 70% success rate on his character traits & actions being mentioned correctly as long as the story input features new letters instead of being a simple `continue`. The author speculates this could be a good way to define something akin to `you` for those interested. All of the random experimentation and associated pictures can be found on Discord.

CrisAIcilian is developing an alternative formatting method focusing on consistency and scene building. It looks like Zaltys visually with few additions/variations and utilizes both WI and Remember heavily. This is exclusively for use without scripting as the method relies on the proper ordering of each data type. Cris explains what each part does with comments inside the example he has provided. This is what his most recent work from Discord looks like:
```
// The WI section (put this in WI) -
⑶	// This unicode number, and its positioning, dramatically improves the AI's ability to process info. Theory: the positioning of this number makes the entry less list, more like a "chapter", the AI gets more creative and will utilize traits it normally would rather ignore.}
<Personal Profile of Vilze>	// This adds extra identification, and was added to combat leak.}
Vilze:[Description<A>:blue horns & skin, white hair, yellow eyes, black wings; she is without genitals; Vilze is 178 cm tall. Wear<B>:nothing. Mentality<C>:troublemaker, loud, lazy, easy-going. Relationship<D>:hates angels(especially Archangel Izrafiel),dislikes Isabella's rules, many friends(Meiling<succubus>). Trait<E>:grew up in hell, pokes fun at everyone, wild life style(parties).]	// Don't change the structure. Spread categories over multiple lines for example, will render the "Above are facts.." less effective.}
Above are facts on the female demon Vilze.	// Don't alter this sentence, adding "The" at the front for example, makes it less effective.}
#	// Separator, important.}
// The /remember section (in /remember) -
⒇
<Your Personal Profile>
You:[Name<A>:?, alias Jack. Gender<B>:?. Age<C>:?. Ethnicity<D>:?. Nationality<E>:?. Description<F>:skinny, etc; you're 170 cm tall. Appearance<G>:freckles etc.]
Above are facts regarding yourself.
#
// The /remember section that leads the story context (also in /remember) -
⑴	// I just feel arranging the two numbers this way is better, with no hard evidence behind it.}
<The Story>
Calendar: November 17, 2017.	// No ABC in this section, ever. The AI will pay too much attention to it and output "You're still sitting."}
The current location: in a hotel room in Washington D.C.
The current event: You're sitting in a chair.
```

Experiments on Discord show similar efficiency and accuracy to the formatting methods used above. In some ways it may be better, like sticking on to the scene you have created through the combined use of mentioned WI and remember. The caveat is how strictly the `#` letter is utilized, slightly restricting your writing and making it incompatible with scripting although this is intentional. The important thing is to make deliberate choices on your formatting across the board and making them play nice with any scripts you're interested in using. Cris has expressed interest in improving his syntax and supporting new users on discord. He also worked on a prompt method called the `Character Generator 3000` that didn't end up getting a scenario released by birb's understanding, 

Notice also the use of unicode symbols. GPT-3 and AID does understand them even including emoji such as :surfing:. Unicode symbols tend to be heavy in tokens and it's hard to find immediate uses for them so they are outside of birb's research but curious users are recommended to do their own research on the topic.


### Misc Tips
Thanks to scripting we can insert lines anywhere in context(history) now. One of the earlier implementations of this was Editor's Notes. This concept is used manually or with a simple script. The in-game tooltips describe author's notes as style hints or being useful for controlling what and how the AI will generate. Paraphrasing from Gnurro(who came up with the idea and authored the original script) `EN behaves more like tool tips suggest AN should work as some kind of more or less direct command about how things should go on. AN needs other wording, but which work best for it has been fiddled with a lot by now. Someone should try combining AN and EN though, would be interesting to see if that gives some kind of belt/brace effect.` Private testing has shown that EN works more like a conductor, keeping the output on whatever track you've given it. More testing is needed combined with things like the `Editor's Notes: This novel has no plot twists, it follows a linear storyline.` consistency string and `RATING:` when used in notes. EWIJSON can achieve this easily with the key: `.#[p=3]` and the entry containing whatever you want to insert 3 lines in front of your next action.

After we discovered that GPT-3 got updated to better handle numbers methods have been developed to utilize this inside WI. Right now the most interesting trick is using the unicode multiplication symbol `⋅`. Putting `0 ⋅ legs` inside `APPEAR` was an effective way of making making the AI realize some character lag legs, but better alternatives have been worked out like ¬legs `¬` being the unicode symbol for negation. It's also easier on tokens than the middle dot. The conjunction symbol `∧` is great for separating traits, but keeping them associated with the same origin. The equivalent symbol `⇔` is great for making the AI understand that an entity or name is equivalent to another another entity or even a group of traits.

The biggest and most thorough testing on symbols has been done by Monky and he has provided us with two documents for Griffin and Dragon he calls the CODEX_SIGNUM. We also have a second document for miscellaneous messy research including goodies like enforcing mute characters or writing logic expressions in AID. These and many other useful user-provided documents can be found in the docs section:
https://github.com/valahraban/AID-World-Info-research-sheet/tree/main/docs/


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
Worlds remains as the feature that has received the least amount of research. Frankly speaking as a WI-enthusiast at least the default worlds aren't very interesting. Their writers included long entries that clearly get cut off by the entry when you view the WI screen. The community respectfully requests the people in charge of Worlds fix this and other issues that may be present. People who still enjoy Worlds, please continue doing so and maybe use the following tricks.

Thankfully worlds works mostly likely a regular scenario containing the same data structures while a lot of it may be hidden from the player. Worlds have more thorough input fields at the beginning and settings akin to sub-scenarios. The world experience can be improved by adding your own WI or with a world-specific prompt trick. This is usually done through the `Name:` field during character creation this being the main way of influencing how the World generates. You input your info into the `Name:` field. This is most effective with JSON-like formats. For example if you put `[{"name":"Eve","theme":"pirate"}]` in the name, the World will name your character Eve, while giving the World a noticeably more pirate feel. Quoting what we know so far from a user named Gen.Alexander:
```
Prose names like "Bill the fallen paladin," "Jessica the necromancer," etc. affect World prompt generation too. But with JSON you can really cram a lot of stuff into it (and it's likely to include at least half of it in the prompt.) Like
[{"name":"Jessica","personality": "ambitious", "birthplace": "Waterdeep", "worships":{"Mystra":"godess of magic"},"relationship":{"Elminster":["teacher","boyfriend"]} }]
I just tried "theme" for that yesterday and it seems to work too.
```
```
Xaxas is a world of peace and prosperity. It is a land in which all races live together in harmony. The gnomes build their machines and live among the humans. Elves run the academies in which people come to learn. Ogres have no qualms living in the same town as humans or gnomes. Yet, all is not well.

You are Jessica, a female human mage from the northern human nation of Waterdeep. You've come to Uruk to meet your boyfriend, Elminster, who previously taught you magic in Waterdeep. You've been secretly practising magic in your spare time and have recently come to the conclusion that you want to dedicate your life to becoming a great mage and opening a school of magic in Waterdeep.
```
Based on Alexander's experiences, the author can cautiously suggest that every keyword trick we've discovered should work with the `Name:` field, affecting how your World is generated. All WI provided with a World stay the same for every play-through though. 


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


## Writing Tools
AI Dungeon is all about manipulating context. At the end of the day the goal of the player is to have a more fun text adventure while ideally spending less time tweaking his world infos or scripts. Or maybe he's a less verbose writer but is able to produce good content through use of >do and >say. Below are listed resources to help you spend less time troubleshooting your WI or to spice up your inputs.

Litscape has many word tools. The most immediately useful ones for WI enthusiasts are words starting and ending with. This search is used to find categories that are less likely to confuse the AI.  
https://www.litscape.com/words/litscape_default_word_list.html  

Relatedwords is a service that finds, well, words related to each other through algorithms and online databases. Good for closely related concepts. Their network also contains other language meta-searchers you may find useful.  
https://relatedwords.org

Quillbot paraphraser is of some arguable usefulness. Creative mode might be the most interesting for tweaking inputs. The premium costs money and free users can only tweak 700 characters at once. You can click words or phrases to have the tool suggest alternatives.  
https://quillbot.com

Power thesaurus is an almost excessive thesaurus containing synonyms, antonyms and definitions for words. It has an addon for chromium-based and firefox browsers that birb uses. Provided is a link to their homepage.  
https://www.powerthesaurus.org

Scripts like EWIJSON require the user learns regexp. birb did this in a week of playing with EWIJSON and reading zynj's wiki so tutorials are not included. A linter is very useful though for both learning and production. Use ECMAScript(JavaScript) mode only.  
https://regex101.com

birb is not a big fan of electron, but he uses the following program to manage and write his WI collections as .json files   
https://github.com/gimzani/ai-dungeon-worldbuilder


## Obsoleted Methods
This section covers methods WI enthusiasts have used in the past but are no longer recommended. For example earlier Zaltys formats are included here to keep a historic archive. This is so we leave a record and can reference things we used to do if OpenAI or Latitude revert any changes they've made on the back-end.

Title: `TRAITS:` category  
Updated 1 Feb 2021:  
Important enough to warrant an entry of its own. Citation from Zaltys since he popularized `TRAITS` in the first place: "I honestly can't think of any use for `TRAITS` now that we have `SUMM`. `SUMM` does the exact same job better." birb believes we may have an unknown use for traits and that people can use it if they desire, but most category users we talk to nowadays prefer `SUMM`.

Title: Old style heavy encapsulation and categories  
Created before 14 Dec 2020, deprecated shortly afterwards  
Updated 8 Feb 2021  
A lot of methods have been tried before you and have been phased out whenever the AI stopped working with them. Here we list a few with pictures, including the curiosity known as Ritz. Maybe they might be useful one day again. birb doesn't claim credit for coming up with anything because there is always someone trying out different things inside AID before you. Fun fact: Rick the Angriest Wizard is actually based on Dave the Angriest Wizard from 4chan's prompts.  
https://files.catbox.moe/5vo8kw.PNG  
https://files.catbox.moe/udidts.PNG  

Title: Symbol use  
Updated 10 Jan 2021:  
`-` `'-'` and `()` are no longer effective. They are treated more like math symbols by the AI now. Onyx has done extensive testing on the Discord to showcase the AI's creative math abilities. 
https://files.catbox.moe/8y17ji.png

Title: Legacy Zaltys Deekin & Aarakocra late 2020 edition
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
Hunter, Lazy and other users have been working with shortened formats motivated by JSON. Originally parentheses were removed being perceived as unnecessary. Early tests suggest it works just as well as proper JSON, bearing in mind the tricks discussed above. The author would recommend pursuing variants of this format for a good balance between accurate details and token use. Originally people called this pseudo-JSON, but more accurately it is pseudo-prose. Token testing shows commas and brackets work as contextual linkers just like in regular grammar.
```
keys: Thea Smith
[{name:Thea Smith,age:25y/o,♀,BODY:[toned,muscular,athletic],skin:[tanned],HEIGHT:[tall],BREASTS:[medium],HAIR:[Purple,very short,mohawk],EYES:[glowing,blue,heavy eyeliner],PERSONALITY:[calls you shorty,Punk,adventures,kind,rebellious],LIKES:[dancing,music,working out],relationship:{Tess:[little sister],Finn:[little brother],Beth:[Mother,Mom],Bob:[father,Dad]}}].
```


## Special Inputs Commands
Why write all this world info if we can't utilize them? Here are some interesting and recommended `inputs` discovered by the community, mostly aidg.club and people from 4chan. Some methods were modified and edited in by the author. A lot of them work in conjunction with things you've defined into WI. 

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
- TL;DR: or Too long didn't read:


## Ending Notes
As mentioned before AID is in constant development and flux of change. Changes to OpenAI and the GPT will reflect on AID too. WI testing was difficult in October, because updates to safe mode and tackling a long-standing caching bug caused issues with the AI especially on Griffin an event which is now affectionately called the Great Stupid. We look forward to what updates the devs have in store for us. Make sure to export and catalog your WI so you can test them out whenever the AI feels stable enough to your liking.

4chan has threads and guides too. They have been beneficial and entertaining to the author. Go on /vg/ and search for /aidg/ if you want their guides and sources, some of it is good especially for NSFW. The special inputs section benefited greatly from their work. Prompts collections exist online but are not provided here due to their NSFW nature. Below is a link to the Anonymous resources & guides, please cross-reference their work with ours:
https://guide.aidg.club

Remember that Anonymous imageboards aren't known for vetting or quality control. Not all the guides are written to the same standards or measures of decency. If a guide is written by a biased prick who is clearly too lazy to test all known methods, you can always just stop reading the guide mine included.

CREDITS: Everyone whose name has been mentioned here and the many AID related communities. Everyone who shares their experiments and results online.  
birb/valahraban for starting this project, CoomerDeveloper for help with aidg and rentry, Lex-DRL for his fork and back-merges  
Anonymous imageboards, aidiscord-wiki and the Discord itself which can be found through provided links.  
2021 is a wild year. I hope you survive. See You Space Cowboy.