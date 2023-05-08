Download Link: https://assignmentchef.com/product/solved-ve593-project-1-search-algorithms
<br>
<h1>Abstract</h1>

The goal of this project is to help you better understand search algorithms and also to learn how to apply them to solve a “real” problem that can be represented as a state-space search problem.

<h1>Introduction</h1>

This project has two parts. In the first part, you will first code the main search algorithms covered in class. Then, you will apply them on the <em>n</em>puzzle and plot their computation times for increasing <em>n</em>.

In the second part, you will use your knowledge of search algorithms to solve the Clickomania puzzle (see presentation below).

<h1>Part 1: Search Algorithms</h1>

In order to implement the search algorithms, we assume that a non-valued state-space graph is represented by a class with the following methods:

<ul>

 <li>successors(state) returns the list of successor states of state</li>

 <li>isGoal(state) returns True if state is an end state</li>

</ul>

When the state-space graph is valued (i.e., actions have costs), the first method is defined as follows:

<ul>

 <li>successors(state) returns for a given state a list of 2-tuples consisting of costs and a corresponding successor states (see example below)</li>

</ul>

These two methods define the interface of a state-space graph. Note that as we assume that actions are deterministic, they do not need to be stated specifically in the interface, because given the current state and the next state, the information of the action is redundant.

Figure 1: Small State-Space Graph.

We provide below some examples of state-space graphs created with such interface. The code for those examples is available in the file testgraphs.py and can be imported in your Python code.

<strong>Example 1. </strong>If the costs are not taken into account, the non-valued statespace graph depicted on Figure 1 can be coded as follows:

class SimpleGraph:

“””Simple graph for testing.”””

def successors(self, state): if state==0: return [1, 2] if state==1: return [2, 3, 4] if state==2: return [4] if state==3: return [5] if state==4: return [3, 5] if state==5: return []

def isGoal(self, state): return state == 5

Recall that any method of a class always includes a first parameter (here, called self) that provides an access to the instance on which the method is invoked.

The corresponding valued state-space graph of Figure 1 can be implemented as follows:

class SimpleValuedGraph:

“””Simple graph for testing.”””

def successors(self, state):

if state==0: return [(2, 1), (3, 2)] if state==1: return [(2, 2), (5, 3), (1, 4)] if state==2: return [(1, 4)] if state==3: return [(2, 5)] if state==4: return [(2, 3), (1, 5)] if state==5: return []

def isGoal(self, state): return state == 5

<strong>Example 2. </strong>The n-puzzle can be implemented as follows:

class State:

“””State for nPuzzle graph that stores the position of the blank.”””

def __init__(self, v):

self.value = v self.blankPosition = v.index(0)

def clone(self): return State(list(self.value))

def __repr__(self):

“””Converts an instance in string.

Useful for debug or with print(state).””” return str(self.value)

def __eq__(self, other):

“””Overrides the default implementation of the equality operator.””” if isinstance(other, State):

return self.value == other.value

elif other==None: return False

return NotImplemented

def __hash__(self):

“””Returns a hash of an instance.

Useful when storing in some data structures.”””

return hash(str(self.value))

class Action:

“””Feasible actions for nPuzzle graph and their effects.”””

LEFT = 0

UP = 1

RIGHT = 2 DOWN = 3 shift = [-1, -3, 1, 3] # Initilalized for n=3, can be overwritten for other n

@classmethod # specifies that update is a class (not instance) method def update(cls, n):

cls.shift[Action.UP] = -n cls.shift[Action.DOWN] = n

class nPuzzleGraph:

“””nPuzzle graph based on State and Action.”””

def __init__(self, n):

Action.update(n)

def setInitialState(self, initialState): self.initialState = initialState

def succ(self, state, action): nextState = state.clone() shift = Action.shift[action] blankPosition = nextState.blankPosition

nextState.value[blankPosition] = nextState.value[blankPosition+shift] blankPosition += shift nextState.value[blankPosition] = 0 nextState.blankPosition = blankPosition return nextState

def successors(self, state):

succs = [] if (state.blankPosition % self.n != 0):

succs.append(self.succ(state, Action.LEFT)) if (state.blankPosition &gt;= self.n): succs.append(self.succ(state, Action.UP))

if (state.blankPosition % self.n != self.n-1):

succs.append(self.succ(state, Action.RIGHT))

if (state.blankPosition &lt; self.n*(self.n-1)):

succs.append(self.succ(state, Action.DOWN))

return succs

def isGoal(self, state): return state.value == [1, 2, 3, 4, 5, 6, 7, 8, 0]

For this example, we defined a class State and a class Action to represents states and actions and other information related to them. To create an instance of this class, we need to pass a parameter <em>n </em>which represents the number of rows (or columns) of the <em>n </em>× <em>n</em>-grid. Therefore, for the 8-puzzle, <em>n </em>is equal to 3.

A heuristic function is defined as a function that takes a state (i.e., heuristic(state)) and returns a non-negative value. Note that in Python, a function can be passed as an argument of another function.

<h2>Coding Tasks</h2>

You need to provide the code for the following search algorithms:

<ul>

 <li>Breadth-First Search (BFS)</li>

 <li>Uniform-Cost Search (UCS)</li>

 <li>Depth-First Search (DFS)</li>

 <li>Depth-Limited Search (DLS)</li>

 <li>Iterative Deepening Search (IDS)</li>

 <li>A* Search (A*)</li>

 <li>Monte Carlo Tree Search (MCTS)</li>

</ul>

They should respect the following names and signatures:

<ul>

 <li>BFS: BFS(Graph, initialState)</li>

 <li>UCS: BFS(ValuedGraph, initialState)</li>

 <li>DFS: BFS(Graph, initialState)</li>

 <li>DLS: BFS(Graph, initialState, depthLimit)</li>

 <li>IDS: BFS(Graph, initialState)</li>

 <li>A*: Astar(ValuedGraph, initialState, heuristic)</li>

</ul>

where Graph (resp. ValuedGraph) follows the interface defined above for nonvalued (resp. valued) graphs, initialState is the initial state, depthLimit is the depth limit for DLS, and heuristic is a heuristic function. All those functions should return a 2-tuple whose first component is a path represented as a list of states from the initial state to the end state and whose second component is the cost of that path. In the non-valued case, this cost is the number of edges in the path and in the valued case, it is the sum of the costs of the edges of that path.

For MCTS, the function should be defined as follows:

<ul>

 <li>MCTS(ValuedGraph, state, budget)</li>

</ul>

where Graph (resp. ValuedGraph) follows the interface defined above for nonvalued (resp. valued) graphs, state represents the current state, and budget is a positive integer specifying how many simulations should be performed. The function should return the next state to be chosen in state.

<strong>Important: </strong>In your implementation, you will access the state-space graph <strong>only </strong>via the interface defined by the two methods listed above.

All those search functions should be placed in a file called search.py. Your code that uses and tests those implementations should be in another file, which does not need to be part of your final submission. You can use the previous simple graph (or create your own) for your tests.

To evaluate the computational times of those algorithms, write a script saved in a file named testtime.py that runs all your implementations on the <em>n</em>-puzzle with increasing <strong>odd </strong><em>n </em>starting from 3 and print the average running times for each algorithm (in the order listed above). The average is computed over 5 runs with different random solvable initial states. The format of the printed message is not important, but it should be clear which average corresponds to which algorithm. The max value of the odd <em>n </em>can be fixed when the computational times become greater than 30 minutes. For MCTS, you will of course call your function iteratively until you reach a goal state. You can terminate the running of a search if it lasts for more than one hour. In that case, the average running time of that algorithm for the corresponding <em>n </em>is NA.

Note that some initial states correspond to unsolvable states. To make sure that the initial state is solvable, the number of inversions should be

Figure 2: A version of Clickomania called Swell-Foop available in Gnome.

even. Assuming we are in state [<em>x</em><sub>1</sub><em>,x</em><sub>2</sub><em>,…,x<sub>n</sub></em>], an inversion happens when <em>x<sub>i </sub>&gt; x<sub>j </sub></em>and <em>i &lt; j</em>. For instance, the end state has an even number (i.e., 0) of inversion. State [7<em>,</em>2<em>,</em>4<em>,</em>5<em>,</em>0<em>,</em>6<em>,</em>8<em>,</em>3<em>,</em>1] has 16 inversions and is thus a solvable initial state.

<h1>Part 2: Clickomania</h1>

Clickomania<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> is a tile-matching puzzle game known under many names, e.g., ChainShot! or Swell-Foop. See Figure 2 for a screenshot. The game mechanics is described as follows on Wikipedia:

SameGame is played on a rectangular field, typically initially filled with four or five kinds of blocks placed at random. By selecting a group of adjoining blocks of the same color, a player may remove them from the screen. Blocks that are no longer supported will fall down, and a column without any blocks will be trimmed away by other columns always sliding to one side (often the left). The goal of the game is to remove as many blocks from the playing field as possible.

We will make the following assumptions:

<ul>

 <li>the game is played on a <em>N </em>× <em>M </em>grid</li>

 <li>there are <em>K </em>different colors</li>

 <li>during one move, the minimum tiles that can be removed is two</li>

</ul>

In this game, the goal is to maximize a score, which is obtained as follows. The initial score is zero. After one move, if <em>n </em>≥ 2 tiles are removed, the score is increased by (<em>n </em>− 1)<sup>2 </sup>points. The end game is reached when no tiles can be removed anymore. At that stage, the player receives a penalty: for each color C, the score is reduced by (<em>m</em>−1)<sup>2 </sup>where <em>m </em>is the number of remaining tiles (regardless of their positions) of that color C.

<h2>Coding Tasks</h2>

You will code this game as a valued state-space graph as described in Part 1. The obtained class should be called Clickomania. An instance of it will also store as attributes the three parameters: <em>N</em>, <em>M </em>and <em>K</em>. A state is identified to a grid which is simply represented as a list of length <em>N </em>× <em>M </em>where each component is a nonnegative integer value smaller than or equal to <em>K</em>. The values 1 to <em>K </em>are used to represent the different colors and 0 encodes an empty cell. This class should be saved in a file clickomania.py.

Beside that class, you need to provide a function called clickomaniaPlayer that takes as arguments an instance of Clickomania and an initial state. It returns a 2-tuple whose first component is a sequence of states from the initial state to the end game and whose second component is the final score. This function should be saved in a file named clickomaniaplayer.py.

For the definition of this solving function, you are free to use any search algorithms (or variant of them). Notably you can reuse the work you did in Part 1. For instance, if you use the A* algorithm, you can define any heuristic function you want. In that case, the heuristic function should also naturally be saved in clickomaniaplayer.py. Your goal is to provide the best solver you can.


