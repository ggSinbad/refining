# refining
Probability of success when overrefining gear on EinherjarRO with expected materials cost.

## Issue

On official Ragnarok Online servers the refining mechanism is simple: given your refine level there's a fixed probability of success and going up one level. Failure means your item is immediately destroyed. This makes finding the probability of refining an item from +0 to +10 as simple as multiplying each of the 10 success probabilities together.

Quality of life changes on the Einherjar server aim to make refining a fairer system, with higher overall success rates and reduced frustration from destroying 100s of equipment items. To do this the probability of your item being permanently destroyed is no longer 1 - success chance. For 90% of failures, the item will lose a random number of refine levels. Only in the remaining 10% of failures will the item be permanently destroyed.

This change, will increasing the probability of reaching high refines levels, makes the probability calculation more calculated. Whereas previously simply multiplying the success rates for each refine gave the probability of completing that many refines, this number now underestimates the true probability. An example to explain this is given below:

 Previously to find the total probability of +4 > +6  you would simply multiply the probabilites of +4 > +5 and +5 > +6.
 Now, this fails to capture the probability of a route such as +4 > (failure) +2 > +3 > +4 > +5 > +6.
 As you can see, there would be infinite ways of reaching +6 starting at +4, whereas before there was only one.
 
 ## Approach
 
 The approach we take to solve this issue is to use markov chains to analytically find the "hitting probability" of each refine given a starting refine. We simply have to calculate the 1 step transition matrix, that tells us the chance of going from each refine level to any other refine level in a single refine action. I used success probabilities found on the wiki, for Whitesmith Level 70 and normal ores first:
 
 ![image](https://user-images.githubusercontent.com/69298405/114826068-eff3a000-9dbe-11eb-9602-69e74bf0cede.png)

and for enriched ores:
 
 ![image](https://user-images.githubusercontent.com/69298405/114826154-0c8fd800-9dbf-11eb-8768-6a0d561d4f5e.png)

and enriched ore destroying probability:

![image](https://user-images.githubusercontent.com/69298405/114826429-6abcbb00-9dbf-11eb-8853-4c55e07643b5.png)


Using these probabilities, I could calculate the rest of the transition probabilities for each state. I wrote a function ProbabilityCalculator to help me do this, then simply manually inputted these probabilities into an array. The rows here tell you the state you are in, where row 1 is destroyed, 2 is zero refines, 3 is 1 refine and so forth. The columns tell you the ending state after one step, and inside the cells are the probabilities. This is for normal ores, and I also computed it for enriched ores after.

![image](https://user-images.githubusercontent.com/69298405/114826881-f1719800-9dbf-11eb-87af-699423a18fab.png)

## Results

Now we have the transition matrix, we can use the R package markovchain to find the hitting probability.

![image](https://user-images.githubusercontent.com/69298405/114827222-53320200-9dc0-11eb-88d9-259bd4545e03.png)


We can see that starting at no refines, the probability of hitting +10 refine for level 4 weapons is around 1 in 3800. +7 refine is only 1 in 3, +8 1 in 15, and +9 1 in 183.

I then repeated the process, except this time using the probabilties for level 4 weapons and enriched ore.

![image](https://user-images.githubusercontent.com/69298405/114884152-716a2300-9dfd-11eb-8a54-00ba135f2624.png)


Here the probability of hitting +10 refine starting at no refine is 1 in 140. +7 refine is 7 in 10, +8 refine is 1 in 3, and +9 1 in 14.


## Analysis

After computing these hitting probabilities we have a much better idea of how rare a +10 refined level 4 weapon is. Obtaining one without using any enriched ore would on average destroy around 3800 weapons before obtaining one. This makes one practically impossible to obtain. Even with enriched ore, the probability is around 30 times higher, but needing on average 140 of a level 4 weapon to obtain one +10 is still unfeasible. This does not take into account the enormous costs of ores that would be needed which further compounds how insane it is to +10.

It's worth noting that enriched ores actually perform worse at +3 > +4 by about 0.035, so optimal results would mean using standard ores for those refines and enriched ores for higher refines.


## To-do List

There's still a lot more to be done. The most important task is to calculate the expected cost or "hitting time" for each refine level. As far as I can tell, this cannot be done analytically because the markov chain is reducible. Therefore I would have to move to a simulation based approach, for which I do not know if there exists a package that could do this for me. 

Less important is accounting for a small mechanic - when refining fails 75% of the time the item is broken (not destroyed) and needs to be repaired. The cost of repairing it is fairly small so it can usually be considered negligible. However it is still worth considering when computing the total cost.

Afterwards, some further analysis comparing the success rate of normal ore vs enriched ore is due. Enriched ore is orders of magnitude more valuable than normal ore, so it should be used sparingly where it will make the most difference. I plan at some point to add a section comparing various rules for when to use enriched ore (+7 only, +8 only ect) and how they perform.

Once all of the above is done, I can make a calculator that takes item value, oridecon/elunium value and enriched ore values to calculate what is the cheapest way to hit a certain refine level. For high cost items, using more enriched ores would be optimal, whereas for low cost items using more oridecon may be cheaper.
