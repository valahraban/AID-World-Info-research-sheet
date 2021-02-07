## About
This document contains research done on the Discord in completely random order. Discoveries may but probably won't be included in the main document. This work exists mainly to document problems WI enthusiasts have repeatedly slammed their heads against the wall to solve. Sometimes a dwarf atomizer was taken to crush a pebble and the pebble won.

## Miscellanneous WI research
The following are the problems, tips and solutions in no particular order. Don't expect this document to be organized or to make any sense out of context.


### Prose Problems
Prose is great. WI enthusiasts would like other AID players to know that they don't have anything against prose. It's simply that reaching certain goals is easier outside of the tools given to us by prose alongside there being many people in this world who are good at coding or logic, but less so at writing English for novella. Due to OpenAI's and AID's training data, prose is bad at doing certain statements we understand in natural writing. For example Onyx and other users have found that function words like `have` and `has` have only weak effect. These denote ownership, not adding content meaning to the phrase. If you say `Bob has` that could mean any number of things, not the concept we want to explicitly explain to the AI.

If you're writing prose certain ways lead to better desired outcomes. Going by the above example we have `Bob has red hair` vs `Bob's hair is red`. If using these in an entry, the former example could result in Bob having any number of things or any number of things being red, if red gets mentioned at all. `Bod's hair is red` is an explicit statement. There is no room for interpretations with poor logic so typically this results in the AI outputting Bob's hair is red more often. In similar fashion `Bob's wings are blue` or Cavemanned `Bob's wings blue` is a good way of defining his wings are blue. Many users believe connecting/function words are unimportant inside WI entries and their shared scenario results support this. If you're willing to write "impure prose" you could also insert `Bob's brown:<hair&mustache>` inside the entry to even more strongly associate the color brown with Bob's facial hair.

`Bob is bald` fails very often. This is a double problem with training data. Connecting words are less important and AID has been trained on data with all kinds of hairy and furry creatures, so it loves describing hair even if a character has already been named bald. `Bob's head<bald>` works better but isn't perfect because the AI has trouble understanding baldness that ISN'T a shaved head because of being a soldier/monk or so on. This is because in natural language adjectives are interchangeable. From the AI's perspective `Bob is bald` is nearly the same as `Bob is purple`. In terms of the language rule algorithm, they ARE the same thing. For the same reason WI for `Bob is dead` don't usually work, a problem the community has had since the beginning. What can be exploited is how sensibility the statement is. `Bob's head is cheerful` isn't something you'd say in English or any languages. The AI gets confused and tends to ignore such an illogical statement. So `Bob's head` has less possible traits associated with, making it more effective to talk about `Bob's head` when we want to give his head specific traits.

Horns are another problematic thing in prose. You could find a way to phrase in `Bob's horns` or `Bob's pair of horns` and the AI understands Bob has horns. Maybe the first couple of times it even places the horns on his head. But then the rep penalty kicks in and AID can start throwing them anywhere it wants. It doesn't help that CYOAs have monsters that have horns in every part of their body. So you must be explicit with the location of the horns. Working examples are `Bob's horns on shoulders` or `horns are on Bob's shoulders`. In caveman which can be mixed with any format you do `bob horns red pair mid forehead`.


### Muteness
Character muteness is a problem in AID. Due to the CYOA and prose training data, characters are expected to engage in dialogue. You can't simply make a character not talk, although you can decrease the likelihood of this occurring. Earlier examples on how AID expects a certain order or explicitness for concepts in prose can be exploited to better create mute or `not talking` characters. For muteness you usually get the AI to understand what you're after the best with the following order:
```
"AID," you begin, "How would you interpret this: Since John is mute, he cannot speak." 
"John cannot speak," AID replies instantly. 
```
Zaltys wrote a WI template that can be used to brute-force a character to behave as a mute. This is roughly 90% functional in Dragon. After he developed mute Zak, he conducted cruel AID experiments to see how mute he behaves in practice.
```
Zak:[Human male. FLAW<Zak>:mute;LACK:voice;SUMM<Zak>:literally mute/incapable of talking<sign language instead>/cannot reply<writes instead>;BANN<Zak>:voice.]
```
```
You knee him in the kidneys. The smile doesn't leave his face. He wobbles a bit, before falling to the ground, clutching his side. You stand over him, looking down.

You knee him in the kidneys.
He falls to the floor, curling around his injury.
Zak looks at you, looking confused and scared.
```

Since utilizing prose order works to some degree Neanderthal version muteness provides some (still needs occasional rerolling) effectiveness:
```
For example, 
:[ Francine since mute woman cannot talk, communicates with gestures only].

Describe Francine attempting to communicate:

"Hi Francine" You say to her.
Francine has a frown on her face. She shakes her head slightly.
```
```
"Hi Francine" You say to her.
Francine slowly gestures to her throat and then points to herself.
You can only guess that she is trying to tell you that she cannot talk or that she has something in her throat.
She then points to her ear and makes a concentric circular motion.
```

Don't expect much out of the methods mentioned here. The failure of respecting muteness is due to the model's training data and is unlikely to be solved for the foreseaable future as it would go against the principles of designing a D&D-like CYOA game.

Also giving the character the trait `silent` tends to make them deaf instead, I don't even know, just AID things. According to the guy testing this kind of mute character the presence of silent traits also made other characters use sing language more often. I don't know, it's AID, deal with it.


### Logic and logic symbols
Onyx did testing on math in AID after the January update with picture provided in the research sheet. It was discovered that somehow OpenAI taught the model how to do some logic and maths. Some users then took these concepts to the extreme. Many examples have been shared on Discord where 'equations' of logic symbols were highly effective at defining entities. The suggested terms for this syntax are Language Algorithm Math Expressions or World Info Script Object Notation. If you want a better understanding of what's going on with the below examples, go read about 'tautology logic symbols'. Qweepa example provided by birb with help from Onyx:
```
// proposed formatting for a functional style of WI
DEF:<Qweepa>:[ Qweepa ⇔ <mammal∧<bunny&lemming_hybrid>∧fluffy∧blob_shaped∧secret_evil∧wings_cherub∧herbivore<¬nice¬carnivore>>.] 
// improved version by Onyx
Qweepa:[adorable * (bunny + lemming) * wings)] *herbivore ^ secretly * (evil + carnivore)]
```
Onyx did a lot of testing on using different equations to represent Qweepa. Pictures are provided to show that LAME does indeed work even for complicated examples.
https://media.discordapp.net/attachments/760367080808644608/806786024557772800/Screenshot_2021-02-04_at_1.19.16_AM.png  
https://media.discordapp.net/attachments/760367080808644608/806789797988925440/Screenshot_2021-02-04_at_1.33.45_AM.png  
https://media.discordapp.net/attachments/760367080808644608/806792057217089646/Screenshot_2021-02-04_at_1.42.16_AM.png  
https://media.discordapp.net/attachments/760367080808644608/806794348062441502/Screenshot_2021-02-04_at_1.51.44_AM.png  
https://media.discordapp.net/attachments/760367080808644608/806798107698266142/Screenshot_2021-02-04_at_2.07.01_AM.png  
https://media.discordapp.net/attachments/760367080808644608/806798361730088963/Screenshot_2021-02-04_at_2.07.21_AM.png  
Onyx hypothesizes that the real strength of the syntax is in explaining which parts of a character's personality are activated by which environmental factors or how to explain a character's emotions towards different family members

A related approach can be used to trick out traits through association inside the remember pin. It doesn't always mention each trait, but seems to associate them strongly. Mr. Accountant called this BABA or BABEL, lacking a strict name for his experiments:
```
[<BABA is <RABBIT>>;
<BABA has <FUR>>;
<FUR is <BLACK>>;]
```
```
You are walking down the street from work. You had a long day at the office due to a busy season filing taxes. You just arrived from your subway stop. While you are waiting you see a strange person standing next to you. But pay them no heed.You board the train and the strange person sits across from you. Apparently, its a Baba!
You look over and immediately you wonder if this is a normal Baba. It looks like one, but it seems to be wearing a coat. Also, its fur is the color of coal. You try not to make eye contact with it.
You look out the window and notice the city's landscape is beginning to transform.
```
```
[<BABA is <WITCH>>;
<WITCH has <POINTED_NOSE>>;
<NOSE is <RED>>;]
```
```
You are walking down the street from work. You had a long day at the office due to a busy season filing taxes. You just arrived from your subway stop. While you are waiting you see a strange person standing next to you. But pay them no heed.You board the train and the strange person sits across from you. Apparently, its a Baba! But pay them no heed.Quite often they will foretell you your future, but today is not the day you wed!
And then a nose begins to protrudes from their face! You are horrified at what you see. But it disappears as quickly as it appears.The nose was red...
```

### More Miscellaneous Stuff
The useful categories section of the main document is growing too large for my tastes. I prefer to keep it on point for the most immediately useful categories. There are potentially countless other useful ones so I'll list the ones that are of personal interest here.

`CREDO:` and `SYMBOL:` are powerful and useful when used with religious entities or goddesses. 
```
Yerrtha.DESC:[major_goddess ancient;CREDO:magic night dreams&collective_unconscious creative_thinking;Yerrtha.SYMBOL:two_parallel_waves.]
The goddess of the night, Yerrtha is a goddess inspired by the collective unconsciousness. She represents things as wild and unknown. The dreaming mind where all traumas are healed and taboos broken. She is the goddess of witches and shamans, no doubt, but even magicians and other holy men find themselves seeking her guidance from time to time. She is not evil or good, but the things she offers are sometimes not for the faint of heart.
Yerrtha is worshipped under different names in different places, many of which are lost to history. Her symbol is a pair of parrallel waves, which represents the ocean of the mind and the shore of consciousness. The black wave is the darkness of the unconscious, while the other is the lightness of the conscious.
```