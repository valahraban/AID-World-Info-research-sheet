## About
This document contains research done on the Discord in completely random order. Discoveries may but probably won't be included in the main document. This work exists mainly to document problems WI enthusiasts have repeatedly slammed their heads against the wall to solve. Sometimes a dwarf atomizer was taken to crush a pebble and the pebble won.

## Miscellanneous WI research
The following are the problems, tips and solutions in no particular order. Don't expect this document to be organized or to make any sense out of context.

### Muteness
Character muteness is a problem in AID. Due to the CYOA and prose training data, characters are expected to engage in dialogue. You can't simply make a character not talk, although you can decrease the likelihood of this occurring. The explicitness and order of words AID expects can be used here. You can test it out for yourself. Prose expects certain explicitness. In Onyx prose experiments writing `Bob's hair is red` works much better than `Bob has red hair`. In the latter 'has red hair' could be applied to any part of your scenario while the former associates the red thing closely with Bob. Another thing Onyx found out is the order `since X, then Y` for logic. For muteness you usually get the AI to understand what you're after the best with the following order:
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
Onyx did testing on math in AID after the January update. It was discovered that somehow OpenAI taught the model how to do some logic and maths. Some users then took these concepts to the extreme. Many examples have been shared on Discord where 'equations' of logic symbols were highly effective at defining entities. One suggested term for this has been World Info Script Object Notation. If you want a better understanding of what's going on with the below examples, go read about 'tautology logic symbols'. Qweepa example provided by birb, BABA by Mr. Accountant:
```
DEF:<Qweepa>:[ Qweepa ⇔ <mammal∧<bunny&lemming_hybrid>fluffy∧blob_shaped∧secret_evil∧wings_cherub∧herbivore<¬good¬carnivore>>.]
```
A variant of WILSON can be used to trick out traits through association inside the remember pin. It doesn't always mention each trait, but seems to associate them strongly.
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