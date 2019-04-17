# nfsu2-ai
This repository is used to create an AI that is able to drive races in the racing game Need for Speed: Underground 2

This document is meant to be some where between a documentation and a written journal that informs on the thought processes behind certain decisions.

Need For Speed Underground 2 is a racing game, made by EA and published in 2004(?) in which the player mainly competes in races against other racers. To win a race and receive the prize money, the protagonist has to become first in each race. In the career mode on which we will focus, a player can also earn reputation. A larger lead at the end of the race yields more reputation. 

Initial idea of the project: Create an AI for Need For Speed Underground 2 that is able to beat the ingame AI opponents on easiest difficulty in one race in the modes 'Circuit', 'Sprint', 'Street X' and / or 'Underground Racing Leauge' ('URL').
To achieve this, we will develop a neural network (?) to determine the lead (or leeway (?)) over the other opponents. With the help of this information, we can reward the AI for being in front of the other racers or punish it otherwise. Hence, we will use reinforcment learning in order to train the AI. The current plan is to utilise a neural network (again?) that uses the whole window of the game and the lead over others as inputs for the network. These inputs will be used to determine the most optimal button presses (outputs) to gain the largest lead over other cars.

The reason behind the limitation of racing modes is that here, leads are constantly displayed in seconds. Moreover, they are easier to handle since in other ones, there are additional 'rules'. In the case of drag races, the player has to shift gears manually and is only informed on its over all position on the leaderboard and does not contain information about leads. For drift races, the goal is to perform drifts to obtain points. Here, the leads are displayed in points as well.

RETRIVAL OF LEADS:

The very first goal is to obtain information about the distance (lead) with regard to other racers. As stated above, we will employ a neural network for this task. It is trained by 'feeding' it a large (~6h) video of the YouTuber 'EwilCZ' (https://www.youtube.com/user/EwilCZ). In this video, Ewil performs a so-called speedrun of the game, in which he tries to finish the game as quickly as possible. At this moment, we are only interested in the parts where he drives during a race of the category 'Circuit', 'Sprint', 'Street X' or 'Underground Racing Leauge' ('URL'). For the training, we hence take out the segment of the video where the racing is done and examine each frame. We take each image of a frame and crop out the part where the leads are displayed.

Insert image

As we can see, there are four rows: one where the name of the player (Ewil) is displayed and three with the leads over the opponents. The three rows are the ones where we get our information from. Thus, they are the ones that will be inserted into the initial neural network. To reduce the number of classes that arise, we separate each row further. As a consequence, we will further cut out five images of each row. 

Insert image

The five images of one row together contain all the necessary information regarding the respective opponent. The image in the center and the one on its left-hand side yield the lead in seconds. Depending on the size of the gap, the more left one of these two images may also display a positive or negative sign. Leads are denoted by a plus sign, while minus signs are used if the player is behind an opponent. For large leads, a plus sign may be found in the left most image of all five images. To give more accurate rewards, we will also include the two images on the right. They specify the lead on a sub-second level. They will be more relevant in later stages of the training if the player competes against enemies on higher difficulties.