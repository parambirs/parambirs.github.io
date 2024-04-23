---
title: 'Programming Problem – The City of Archers'
date: 2011-01-20T00:00:00-07:00
tags: ["algorithm"]
summary: ""
---

Here’s an interesting programming problem that a friend of mine sent me a few days back. Download a working Java solution for this problem [here](http://www.box.net/shared/jd834b59no).

**The City Of Archers**

_Back in human history there used to exist a city known as Genon City .It was always under threat from the enemies but it had the best archers in the world ever known to protect it._

_To prepare for battle all archers performed similar actions. First go to wamboo-a warehouse- to collect arrows. Number of arrows one gets is the sequence number in which he arrives, first one gets 1, second 2 and so on. No two archers arrive at the same time._

_Every archer then puts the arrows inside marina – a machine – which first cleans the arrow, then sharpens and finally colour’s it but every 5th arrows that is cleaned get broken ,so machine replaces it with a special arrow which is equivalent to 2 normal arrows and archer is not aware of this._

_At the battle fort they stand in same sequence as they arrived at warehouse and fire one by one starting with first. Far away are enemies. It takes 2n arrow to kill an enemy, first 2, and second 4, third 6 and so on. Every 7th arrow fired misses the enemy which increases that enemy’s strength and it requires 2 more arrows now to get killed ,Archers is not aware of this._

_Before every arrow shot, an archer knows how many enemies are already killed or semi killed (shot with few arrows before). So he calculates arrows for killing next enemy .An archer leaves whenever he doesn’t have enough arrows to kill the enemy and next archers come to action._

_A battle is won if all the enemies are killed with 0 or more arrows remaining._

_Write an application which models the above situation. First input to the application should be number of archers and the second input should be number of enemies._

**Solution**

Download Java solution for this problem by clicking [here](http://www.box.net/shared/jd834b59no).

**Sample Input/Outputs**

Input

Number of Archer 4  
Number of Enemies 2

Output

Battle – Won  
Total Enemies Killed: 2 Total Arrows Fired: 6

Input

Number of Archer 20  
Number of Enemies 5

Output

Battle – Won  
Total Enemies Killed: 5 Total Arrows Fired: 41

Input

Number of Archer 100  
Number of Enemies 50

Output

Battle – Lost  
Total Enemies Killed: 45 Total Arrows Fired: 2817

Input

Number of Archer 70  
Number of Enemies 40

Output

Battle – Lost  
Total Enemies Killed: 31 Total Arrows Fired: 1428
