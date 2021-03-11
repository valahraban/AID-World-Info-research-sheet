\<\<Futureman>>>> Feb 03 2021

GENESIS (recommended for new users eager to learn what is going on under the skin, skip to examples if you just want the format)

Futureman is a branch of Caveman format with lessons learned from the Neanderthal branch of Caveman in early January 2021.

To give a basic understanding, Caveman format (the base this is all built off of) achieves shorter length than essentially all other World Info and Remember formats, and uses fewer tokens (tokens are chunks of words or full words that the AI uses to figure out what it should output next - and they are important to keep to a minimum within reason, particularly for Griffin), allowing you to get the most bang for your buck by allowing *more* World Info's (aka WI's) to effectively trigger, larger effective Remember (aka Pin) space, and more of the context (aka history or story) to get inserted without the important details being lost, clipped off due to maximum length, etc. and reducing frustration of "why can't the AI remember what this NPC/my character's gender/appearance/name/personality is!" etc.

How Caveman format accomplished this was be removing pretty much anything that wasn't necessary to convey a basic sense of order and meaning in a sentence (and I mean VERY basic sense), and then *let the AI rebuild the connections between the remaining words in its output.

For example, take this block of Caveman format created at the end of Nov 2020;  
```
Keys: 
Chagra, Orc, witch, coven, friend

Entry:
Chagra AKA 'Tusk': Orc she 20y.
Chagra 220cm ht. 100kg hardy.
Chagra green scar skin. Long brown hair.
Chagra loyal careful outcast pranked Healing magic initiate coven.
Chagra Hergea's daughter Friends You Renni.
Chagra likes fruit veggies Hates meat cheese.
```  
Important factors: Keeps lines fewer than 15 tokens (approximately 50-60 characters) by refusing to use symbols and removing any word the AI doesn't strictly need to make appropriate connections. Each line ends in a period and a newline (enter key press) immediately after, to help focus the AI on the next line of description, which starts with the subject's name or noun. This is primarily to allow formats to work on Griffin, which tends to lose track of a subject after around 15 tokens, causing all sorts of problems like one character's attributes leaking onto another character half the time.

Essentially, this breaks down language into its key components and relies on the AI to rebuild the rest based on its own biases, which it generally accomplishes very effectively. The above example would accomplish outputs where Chagra would faithfully like fruits or veggies, be disgusted by meat and cheese, be a loyal friend, be pranked and shunned by others, etc. all while being super compact and efficient and not cause the AI to start talking like a caveman... until a math change occurred around Jan. 03 2021. After that, the AI would start talking like a caveman more often than was acceptable, particularly early on in stories where there wasn't enough prose to keep it stable.

To remedy this situation, Caveman needed to evolve. I experimented with a number of token efficient symbols and came to the conclusion leaks could effectively be solved with some encapsulation, and you could be efficient with symbols to convey meaning without wrecking your token efficiency if you used them smartly. 

This leads us to Neanderthal format;
```
Keys:
Alexandria, knight, ally, fight, friend

Entry:
 Alexandria:[ Age 29 human she 152cm height 74kg strong tough thin].
 Alexandria:[ Has Blackflame rune sword, steel plate mail armor, amulet].
 Alexandria:[ Long red hair, fair skin, narrow green eyes, clean].
 Alexandria:[ Your ally, serves Baron Gerald, worship war deity Herako].
 Alexandria:[ Has three healing potions, resurrection scroll, green cloak, dagger].
 Alexandria:[ Likes meatpie ale dancing fighting, hates gambling greed wizards dragons].
 Alexandria:[ Veteran tactical planner, quick wit jokes silver tongue, adventurous bold].
 Alexandria:[ Lives Brun Village, husband Eric, mother of Susan and Timothy].
```  
As you can compare in relation to Chagra, Neanderthal maintains the basic spirit of Caveman but adds some measures that assist in preventing leaks like encapsulating most of it in :[ and ]. which means on average, a Neanderthal line costs one more token than a Caveman line (through adding :[ as ]. replaces the period from Caveman). In exchange, the contents of the format are dramatically less likely to leak. In addition, more aggressive use of separators like commas to group ideas into more discreet sub sections helps keep them from influencing the wrong thing, as an example it's no good if Alexandria hates fighting because the words 'fighting' and 'hates' are next to each other, as she's a boisterous rough and tumble knight. Putting a comma between them groups the words better for the AI, minimizing mess ups by unintended associations.

Further refinement lead to the name of the subject being brought *inside* the brackets as such;  
`:[ Alexandria Lives Brun Village, husband Eric, mother of Susan and Timothy].`
This is to allow the subject's name/noun to not be broken into multiple tokens (usually, depending on the subject's name/noun) by lacking a preceding space, as it was found World Info entries would cull any spaces at the start of a new line. It *also* reduces the chances of `:[` outputting after a character speaks, as the AI occasionally grabbed `:[` and treated it like a speech indicator similar to the 'Character: I think we should x y z' format of speech AID is capable of.

Neanderthal is still viable, but it is stuck with the unfortunate fate of being lumped in way at the back due to how WI's are treated by AID, instead of being brought to where they are relevant in the context (ie; inserted just above the most recent keyword), they are way in the back where the AI has a really good chance to ignore or misread them, leading to refreshes, frustration, and eye rolling. But that is where EWIJSON comes in.

EWIJSON is a script tool made by Zynj for use in your own scenarios, and it is powerful enough to solve the above problem by letting you set where various entries can be tossed in, allowing the AI to focus on them so much better that it almost feels like upgrading Griffin to Dragon, and Dragon to something even better providing you use a little care. But... Neanderthal leaks at an unacceptable rate when it is launched forward into context, particularly if the story is just starting...

So now we need a hero, and in comes Futureman. To solve the problem of `:[` and `].` leaking, Futureman uses the fact the AI is unable to output `<` and `>` symbols due to the particular setup of AID (think using the Do or Say command `>` You do/say x y z, the AI can't output a `>` You do a b c instead, and sticks to prose). After experimenting, `<<` and `>>>>` were found to be strong replacements that were also only one token a piece. But this isn't all Futureman is, it's intended to be paired with specific things in EWIJSON that control *where exactly* Futureman lines are inserted.

Futureman example;  
WI 1  
```
Keys:
Alexandria|knight|ally|fight|friend#[l=3t=1]

Entry:
<< Alexandria age 29 human she 152cm height 74kg strong tough thin>>>>
<< Alexandria long red hair, fair skin, narrow green eyes, clean>>>>
```

WI 2  
```
Keys:
Alexandria|knight|ally|fight|friend#[l=3t=3]

Entry:
<< Alexandria wear blackflame rune sword, steel plate mail armor, amulet>>>>
<< Alexandria three healing potions, resurrection scroll, green cloak, dagger>>>>
```

WI 3  
```
Keys:
Alexandria|knight|ally|fight|friend#[l=3t=5]

Entry:
<< Alexandria veteran tactical planner, quick wit jokes silver tongue, adventurous bold>>>>
<< Alexandria likes meatpie ale dancing fighting, hates gambling greed wizards dragons>>>>
```

WI 4  
```
Keys:
Alexandria|knight|ally|fight|friend#[l=3t=7]

Entry:
<< Alexandria your ally, serves Baron Gerald, worship war deity Herako>>>>
```

WI 5  
```
Keys:
Alexandria|knight|ally|fight|friend#[l=3t=9]

Entry:
<< Alexandria lives Brun Village, husband Eric, mother of Susan and Timothy>>>>
```

In the above example, Alexandria's information is broken up into 5 separate WI's using *almost* identical keys. The only difference is the number after t in the `#[l=Xt=Y]` bracket in the key, which while it may seem complicated is actually pretty simple; the t means 'trail', and the number is the distance the WI's information 'trails' the most recent keyword by when it is fed to the AI. Doing this means the WI info is no longer stuck way at the back, but allowed to be a lot closer or -at- the front. It is broken up most efficiently into 1, 3, 5, 7, and 9 distance to keep it from being thrown in as a monolithic block, allowing the AI to 'breathe' some prose inbetween lines, reducing chances of the AI talking like a caveman to essentially nonexistent, and *also* preventing flooding out of the main scene that is going on and immediately preceding events as best as possible. To top it off, the l character in `#[l=Xt=Y]` section determined *how many turns the info will stick around* so that if there have been 5 turns since the last mention of any of Alexandria's keywords, it'll drop her info from the context until she's brought back up again, giving you back your space on demand.

The BIGGEST risk you have here is rigging too many entries to come too far forward in the context, flooding out what's going on in the story with a bunch of blocks of format. For this reason, I only recommend allowing two lines max to be brought up to t=1, three lines to t=3, and 4 line to positions 5, 7, and 9 respectively. You can add more after that at 11 and 13 or beyond if you want, but at a certain point it becomes viable to just let them sit at the back if they're not super important. For those entries that are not important, just put #[m] after their keys in the same fashion as you would otherwise do with `#[l=Xt=Y]`.

So that's a lot of information to take in so I'll try and briefly TL;DR the Futureman benefits.

	1; Can't leak its encapsulating symbols as the AI can't output `<` or `>`.

	2; Should be effectively able to replace Neanderthal (though Neanderthal is still fine in general w/o use of EWIJSON, and may survive use in EWIJSON if you prune occasional mistakes early in the story).

	3; Is capable of being brought to the very front without leaking or breaking flow of the story, giving your WI info access to the zone of context the AI pays *the most* attention to, allowing very accurate descriptions or behavior, depending on what you prioritize.

	4; While seemingly intimidating at first, once you learn the basics of separating keys with | instead of commas, and then slapping `#[l=Xt=Y]` with appropriate numbers in place of X and Y (3 to 5 generally works for l and odd numbers tend to work well for the t), you're golden.

	5; Don't overload the front end, just let the *very most* important things you DO NOT want the AI to get wrong except very occasionally when AI does its AI thing. For everything else, there's the #[m] tag.

If you made it this far, kudos, and happy WI formatting!  

EWIJSON scripts available at: https://github.com/Zynj-git/AIDungeon/tree/master/AID-Script-Examples/EWIJSON  
EWIJSON wiki available at: https://github.com/Zynj-git/AIDungeon/wiki/EWIJSON