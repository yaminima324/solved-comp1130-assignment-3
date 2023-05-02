Download Link: https://assignmentchef.com/product/solved-comp1130-assignment-3
<br>
Chapter 1

<h1><a name="_Toc19209"></a>Introduction</h1>

<h2><a name="_Toc19210"></a>1.1     Description of the program</h2>

This program is a board game named Ataxx. In this game, users can choose to play with different bots. They can also select any two bots to compete against each other.

<h2><a name="_Toc19211"></a>1.2     How to use this program</h2>

This game can be run by using cabal commands. Running cabal v2-run game — –p1 PLAYER –p2 PLAYER in the terminal can start the game. The PLAYER can be substituted by human or ai:AINAME. This command-line could also be followed by a list of arguments. For example, –comp1130 enables playing on a 9×9 board, –debug-lookahead can show how many moves the AI has considered.

Chapter 2

<h1><a name="_Toc19212"></a>Contents</h1>

<h2><a name="_Toc19213"></a>2.1     Program design</h2>

The design of the program can be divided into four parts, which are “Heuristic”, “Algorithms for AIs”, “Functions”, “Integration of the program”.

<h3><a name="_Toc19214"></a>2.1.1    Heuristic</h3>

There are three types of heuristics. The first one is to count the number of Pieces on the board for our focused Player. It is the simplest one and is applied to Greedy AI. The second heuristic uses the number of Pieces of the own side minus that of the other side. AIs using this heuristic would become aggressive since it will attempt to maximize the score of itself and minimize the opponent’s score at the same time. The Minimax AI and Pruning AI are using this heuristic. The third heuristic is to add more weights to some of the Locations on the board. For Ataxx, the Locations on the margins of the board or near the blocks are more important than others. So, extra scores are awarded for each Player if they have Pieces in these Locations. However, since this heuristic will slow down the lookahead AIs, I decided not to use it.

<h3><a name="_Toc19215"></a>2.1.2    Algorithms for AIs</h3>

There are four algorithms used by different AIs in AI.hs. The first one is called firstLegalMove, the second one is Greedy, the third one is Minimax and the last one is called Alpha-beta pruning.

For the firstLegalMove, it will not consider what will happen in the next round. It only returns the first element from the list of all legal Moves.

In terms of Greedy, it considers exactly one step ahead. It will apply all the possible Moves to the current GameState. So, it will know what the GameStates can be in the next round. Then, apply the heuristic to all GameStates to get the all next-round game scores. The Greedy AI can identify the specific move to apply such that it can reach the GameState with the highest score in the next round.

With regard to Minimax, it considers as many steps as it could within the time limit. It assumes that both Players are playing optimally. So, the Player at the own side will choose the Move that leads to the highest game score, and the opponent will select the Move to minimize the game score to suppress the other Player. Thus, the result derived from Minimax is the best possible outcome for the focused Player if both Players behave perfectly. The general concept of Minimax can be divided into Max case and Min case. Firstly, we need to construct a GameTree consisting of all possible GameStates. There are Max nodes and Min nodes in the tree. The node is Max if the current turn is for focused Player, and vice versa. For Max nodes, the Minimax will find the largest value in the list of game scores (generated from the subtree of this node). Whereas, for the Min nodes, the lowest score in the list would be chosen.

For Alpha-beta pruning, it is like the advanced version of Minimax. It can also consider as many steps as it could, but it will generally be faster than the Minimax. Since it will omit certain impossible outcomes, there are fewer possibilities to consider. The general concept of Alpha-beta Pruning can be divided into Max and Min cases. For Max nodes, if the val is larger than alpha, we substitute alpha with val and continue to check other subtrees. After finishing checking, return alpha as the value of this node. It is similar for Min nodes that if val is smaller than beta, we substitute beta with val and continue to check. Finally, we return the value of beta for Min nodes.

Next, I will illustrate the pruning works. For all alpha and beta in the nodes of

GameTrees, alpha is smaller than beta. Suppose we are checking a Max node. If the val (value of one child node) is larger than beta, and thus it is greater than alpha. According to the algorithm, we substitute alpha with val. Then, the current value of node n should be in the set S

<em>S </em>= { <em>n </em>| <em>n </em>∈ <em>N,</em>val≤n≤beta}

Because val is larger than beta, the set S is empty. So, we cannot find a real number n in this set. Therefore, no value would be returned for this node and we do not have to continue to check the remaining subtrees.

<h3><a name="_Toc19216"></a>2.1.3    Functions</h3>

The code for four AIs are written by using a variety of helper functions.

In terms of the firstLegalMove AI, it uses the head function to obtain the first element in the list generated by legamMoves and current GameState.

In terms of the Greedy AI, the allNextStates function combines legalMoves, allMaybeStates and deleteMaybe functions to return a list of all possible GameStates in the next round. They are applied to the first heuristic and bestOptionIndex to get the index of the highest score (the same as optimal Move). Finally, the index is used by the greedy function.

With regard to Minimax AI, the miniMaxPieces function integrates formGameTree and the second heuristic and returns the highest possible score. The index of this score is found by indexMinimax, which uses the miniMaxPieces function. The miniMaxMove utilizes this index and return the optimal Move.

For the Alpha-beta pruning AI, it is set to be the default AI. The pruningOptimal function combines formGameTree, alphaPrune, betaPrune and the second heuristic to return the highest game score. The indexPruning function utilizes pruningOptimal function to return the index of the highest score. This index is then inputted to pruningMove.

<h3><a name="_Toc19217"></a>2.1.4    Integration of the program</h3>

In AI.hs, there is a function called ais. It is the list of tuples consisting of AI

NAMEs and the functions (AIFunc) to find out the optimal Move. When a specific AI NAME is called by the command for running the game, the corresponding AIFunc starts to return the best Move. Then, this Move is inputted to applyMove function with the current GameState. Subsequently, the nextTurn function is applied, and change the Turn to the opponent. Then, the opponent Player will repeat the process above.

<h2><a name="_Toc19218"></a>2.2     Assumptions</h2>

For WithLookahead AIs, more specifically, Minimax AI and Alpha-beta Pruning AI, all the optimal Moves are returned by assuming that the opponent is also playing optimally. Because we are interested in what is the best Move we can take under the suppression of the opponent. This is the reason why we need to use maximum for Max nodes and minimum for Min nodes.

Another assumption was made for the design of the third heuristic. Because the board size is different for COMP1130 version and COMP1100 version of the game, the Locations for extra game scores are different. I assumed that the adding-weight heuristic will only be applied to the larger board.

<h2><a name="_Toc19219"></a>2.3    Testing</h2>

The testing for the Ataxx game can be divided into three parts.

The first part is to run the AITests.hs by typing cabal v2-test in the terminal. All functions used by Alpha-beta Pruning AI (default AI) are covered in the tests. For each function, the GameStates with different Turns of Players and varied game situations are used. These results of the tests are compared with the outcomes got from Minimax AI and those from hand calculations to make sure the functions of Pruning AI works as expected. The depth of testing is set to 2 for two reasons. Firstly, Minimax AI and Pruning AI should consider the same steps ahead so that they will get the same result if they work properly. Secondly, the results were checked manually, so the workload should not be extremely heavy. There are 22 tests in total and details can be found in the test file. Generally, the possibility for functions to work improperly is very low.

The second part is to test the Pruning AI holistically. I tried to run the game by specifying Pruning AI as either Player1 or Player2, and the opponent to be any AI among firstLegalMove, Greedy AI and Minimax AI. Besides, both board sizes are used for testing the default AI. The game result shows that the Pruning AI always wins. Moreover, –debug-lookahead argument is also used during the testing of the AI. It was observed that the Pruning AI can usually look deeper than the Minimax AI. So, the alpha-beta pruning algorithm is working properly.

The third part is to test individual functions by running cabal v2-repl comp1100-assignment3 in the terminal. Then, we can import any module and test any functions we want to test by giving them inputs. For example, when we input the focused Player and the current GameState to the heuristic functions, we can get the game score for that Player. Again, we can use hand calculations to verify the outputs of functions.

<h2><a name="_Toc19220"></a>2.4     Inspiration</h2>

The algorithm of the alpha-beta pruning was very abstract and I could hardly understand it. So, I did some research online to acquire a better understanding of it. I read the pseudocode carefully and thoroughly from <a href="https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-4-alpha-beta-pruning/">this</a> <a href="https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-4-alpha-beta-pruning/">webpage</a>. Then, I had some brief ideas of what the algorithm was. Finally, I succeeded in writing functions for the

Alpha-beta pruning AI.

Chapter 3

<h1><a name="_Toc19221"></a>Reflection</h1>

<h2><a name="_Toc19222"></a>3.1     Conceptual and technical issues</h2>

One technical issue happened when I was trying to write functions for four AIs. It is very difficult for me to write a single function to fulfill the functionality of AI. So, I decided to divide each AI into separated parts, and write pieces of helper functions to achieve the functionality of each part. Then, combine all the helper functions to the main function. There are generally two advantages to utilizing helper functions. Firstly, the helper functions defined for one AI may be useful in other AIs. So, there were fewer functions required to define and can reduce the workload. Secondly, it will be easier for programmers to identify the errors in codes since each AI is modulized.

The second issue is about the third heuristic. I checked the function of it by running the files in GHCI to make sure it worked properly. But I found that when using this heuristic, the Pruning AI cannot look as deeper as before. So, sometimes it may fail to beat the Minimax AI. Then, I analyzed the time complexity of the third heuristic function. The complexity is <em>O</em>(<em>n</em>). It is linear and should not be very slow. But when we apply this function to the leaves of the GameTree with depth 4. We will need to call this function for million to billion times. This is the reason why the Pruning AI becomes very slow if it uses the adding-weight heuristic. I have not come up with an idea of solving this problem. So, I decided not to apply it to any of the AIs.

Another issue is about using Alpha-beta Pruning AI and Minimax AI in a smaller board (COMP1100). I found that the advantage of the Pruning AI cannot be fully observed in the smaller board because there are not so many possibilities. The number of possibilities is within the calculation threshold of Minimax AI. Thus, in the mid-term of the game, the numbers of steps considered by two AIs are the same. This leads to a close game. Unlike the larger board, the Pruning AI will usually dominate the game.

<h2><a name="_Toc19223"></a>3.2     Something to do differently next time</h2>

All of my heuristics have some major drawbacks. The problem could be either slowing down the AIs or a bad scale for game scores. So, if I have more time, I will try to find out a more efficient and smarter heuristic to enhance the performance of my AIs.

Chapter 4

<h1><a name="_Toc19224"></a>Reference</h1>

– Aradhya, A. (no date). “Minimax Algorithm in Game Theory | Set 4 (Alpha-Beta Pruning)”. Retrieved on 23rd May 2020 from <a href="https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-4-alpha-beta-pruning/">https://www.geeks </a>forgeeks.org/minimax-algorithm-in-game-theory-set-4-alpha-beta-pruning/