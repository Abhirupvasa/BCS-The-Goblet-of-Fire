# BCS-The-Goblet-of-Fire
BCS secretary recruitment task - The Goblet of Fire

(For testing the code use the code in the file : "submission_code", add the pickle file in the line 176, The default number of episodes are set to 200 (to be able to perceive the percenaage win per 100 episodes) you can change the number of episodes in the lines 167 and 264, change the render = True to False for the fast training in line 167 and the line 264, The values are trained on the maze V1.txt which is to provided in line 36. and all the required python packages should be installed(pygame, numpy, matplotlib, tqdm), pickle file is available in the files section named : "q_table_30000.pkl")

I trained the model using the code in the file : training_code for 30,000 episodes:
No. of generations it takes for Harry to escape the Death eater consistently (10 times in a row) = 23922 : with the win rate of nearly 60 percent in that span of 100 episodes
![image](https://github.com/user-attachments/assets/e778bbcb-84f1-4281-906d-2bc197a33030)
When I trained the model several times for 30,000 episodes I have got maximum win rate of 63% for the 100 episodes spanning across the 24000th generation consistenly.
At the end of 30,000 generations the win rate was around 58% and is almost constant for all the future episodes(tried until 50,000 episodes).
The statistics for the training are : ![image](https://github.com/user-attachments/assets/5f8a9af3-bda3-46c5-a7ff-93a5de5ea865)
The statistics include : Per episode reward, Training errors with rolling length of 50 and win rate over time(tracking for every 100 episodes)
The pickle file containing the q table recoreded at the end of the training : q_table_30000.pkl(in the files section)

Assumption: The death eater can cross the cup.
            The maze donot have a position surrounded by walls on all sides.

Code Explanation:
1)I recreated the maze from the input file, allowing the option to upload differnet mazes. Here I used the line.strip() to remove the white spaces in a line and then iterated through the characters of every line to recreate the maze.
2)Used pygame for rendering the animation. Created the 15*10 grids with the individual grid size of 40*40 pixels. The free path is shown in white and the obstacles in gray. Cup is shown in green color, Harry in blue and death eater in red. Used the time.sleep(0.2) to help us percieve the rendering thorough human eye.
3)Wanted to implement it in the similar manner to the blackjack game with the environment already defined in the gymnasium, so defined a class maze_env, and the functions render,reset, get_state, step (also move_harry and move_death for moving harry and death)
4)functions in maze_env : (i)reset : resets the environment and returns the random positions of harry,cup and death eater
                          (ii)get_state : gives the state of the agent, here state is defined as the tuple of the positions of harry, death eater and cup
                          (iii)step : Takes the action as input and moves the harry and death eater through the functions harry_pos and death_pos and also assigns the rewards, if harry wins then 50 points , loses to death eater then -100 points. I also added a reward 0f 0.5 points if the action taken by harry leads him closer to cup than before.
                          (iv)move_harry : Takes the action as input and changes the coordinates of harry
                          (v)move_death : Taked the position of harry and itself and inputs, and using the BFS algorithm changes the position of death eater
5)Function q learning:
  Used the learning rate 0.1, discount factor 0.95, initial epsilon as 1.0, the epsilon decay as 0.995 for best outcome possible.
  Used tqdm to track the progress of training 
  For every episode first used env.render to start the rendering, then figured out what action should be taken by the harry using the greedy epsilon method, then calcuated the temporal difference then the q values of the next state and then updated the dictionary of q table and finally updated epsilon
  In the progress, for figuring out what action to be taken by harry, first filtered out the possible actions(actions which donot lead to a wall or obstacle) and then using the greedy epsilon method - if the random probability is higher than the epsilon, in that case too I ruled out the actions which lead to a obstacle or wall by making their q values - infinity.
  Figured out the successful episodes, win rates per 100 episodes and reward list for each episode from the reward values and whether if the final state of harry is same as the state of cup
  I also limited the number of episodes to 200, to avoid and remove the cases of oscillation where if harry moves up, death eater moves up and viceversa.
6)Stored the q values in pickle file cause it retains all the builtin data types.
7)Plotted the statistics.

Randomess1.txt: Harry gets the ability to move 2 spots every 10% of the time :
(Included the randomness : 'Harry gets the ability to move 2 spots every 10% of the time' in the code in the file named randomenss1.py where inorder to turn the renderng on , change the render = True in the lines 177 and 270, to change the number of episodes for training : change the episodes in the lines 177 and 270 :
In this code I have changed the move_harry function making it able to move two steps if the random probillity predicted is less than 10 percent as if harry gets the oppertunity to move 2 spots every 10% of the time across all the episodes this just means that at every point of time there is a 10% for him to take 2 steps.)

For 2 death eaters:
(if the death eaters donot cross each other : for every death eater just treat the other death eater as obstacle or wall and proceed and should also rule out the case in which the death eater 1 is surrounded by walls on 3 sides and the 2nd death eater on the fourth side.
if they can cross over each other : need to well formulate the case as for example when the death 2 is at a distance greater than 1 from death1 and it is in the future path of death 1 as predicted by bfs algorithm of death1, then I need to compare the future paths of death1 and death2 so as to consider death2 as obstacle or free space, if they cross each other then they both follow the same path)
