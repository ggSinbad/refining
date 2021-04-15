# refining
Probability of success when overrefining gear on EinherjarRO with expected materials cost.


On official Ragnarok Online servers the refining mechanism is simple: given your refine level there's a fixed probability of success and going up one level. Failure means your item is immediately destroyed. This makes finding the probability of refining an item from +0 to +10 as simple as multiplying each of the 10 success probabilities together.

Quality of life changes on the Einherjar server aim to make refining a fairer system, with higher overall success rates and reduced frustration from destroying 100s of equipment items. To do this the probability of your item being permanently destroyed is no longer 1 - success chance. For 90% of failures, the item will lose a random number of refine levels. Only in the remaining 10% of failures will the item be permanently destroyed.

This change, will increasing the probability of reaching high refines levels, makes the probability calculation more calculated. Whereas previously simply multiplying the success rates for each refine gave the probability of completing that many refines, this number now underestimates the true probability. An example to explain this is given below:

 Previously to find the total probability of +4 > +6  you would simply multiply the probabilites of +4 > +5 and +5 > +6.
 Now, this fails to capture the probability of a route such as +4 > (failure) +2 > +3 > +4 > +5 > +6.
 As you can see, there would be infinite ways of reaching +6 starting at +4, whereas before there was only one.
 
 The approach we take to solve this issue is to use markov chains to analytically find the "hitting probability" of each refine given a starting refine. We simply have to calculate the 1 step transition matrix, that tells us the chance of going from each refine level to any other refine level in a single refine action. Once we have this we use the R package markovchain which has a built in function to compute the matrix of hitting probabilities.
