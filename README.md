# Search-Algorithms-Implementation-for-Pac-Man-AI
## 📚 Introduction to Search Methods
This project implements classical search algorithms for solving pathfinding problems in a Pac-Man game environment. These algorithms form the foundation of artificial intelligence and are used to find optimal or near-optimal paths from a start state to a goal state in a state space.
## 🎯 Problem Structure
The SearchProblem class defines the interface for search problems:
 - **States**: Represent positions in the maze
 - **Actions**: Movement directions (North, South, East, West)
 - **Transitions**: getSuccessors(state) returns (next_state, action, cost)
 - **Goal Test**: isGoalState(state) checks if Pac-Man reached the goal
## 🔍 Implemented Search Algorithms
### 1. Depth-First Search (DFS)
**Strategy**: Explore deepest nodes first (LIFO - Last In, First Out)\
**Implementation**: Uses a Stack data structure\
**Characteristics**:
 - May not find the shortest path
 - Can get stuck in deep paths
 - Memory efficient for shallow solutions
```python
def depthFirstSearch(problem):
    from util import Stack
    fringe = Stack()
    fringe.push((problem.getStartState(), []))
    visited = set()
    
    while not fringe.isEmpty():
        current_state, path = fringe.pop()
        if problem.isGoalState(current_state):
            return path
        if current_state not in visited:
            visited.add(current_state)
            for successor, action, step_cost in problem.getSuccessors(current_state):
                if successor not in visited:
                    fringe.push((successor, path + [action]))
    return []
```
### 2. Breadth-First Search (BFS)
**Strategy**: Explore shallowest nodes first (FIFO - First In, First Out)\
**Implementation**: Uses a Queue data structure\
**Characteristics**:
 - Guarantees shortest path (in terms of steps)
 - Explores all nodes at depth d before depth d+1
 - Can be memory intensive
```python
def breadthFirstSearch(problem):
    from util import Queue
    fringe = Queue()
    fringe.push((problem.getStartState(), []))
    visited = set()
    
    while not fringe.isEmpty():
        current_state, path = fringe.pop()
        if problem.isGoalState(current_state):
            return path
        if current_state not in visited:
            visited.add(current_state)
            for successor, action, step_cost in problem.getSuccessors(current_state):
                if successor not in visited:
                    fringe.push((successor, path + [action]))
    return []
```
### 3. Uniform Cost Search (UCS)
**Strategy**: Explore nodes with lowest path cost first\
**Implementation**: Uses a Priority Queue with cost as priority\
**Characteristics**:
 - Optimal for any step cost
 - Dijkstra's algorithm variant
 - Expands nodes in order of g(n) - cost from start
```python
def uniformCostSearch(problem):
    from util import PriorityQueue
    fringe = PriorityQueue()
    fringe.push((problem.getStartState(), [], 0), 0)
    visited = {}
    
    while not fringe.isEmpty():
        current_state, path, cost = fringe.pop()
        if problem.isGoalState(current_state):
            return path
        if current_state not in visited or cost < visited[current_state]:
            visited[current_state] = cost
            for successor, action, step_cost in problem.getSuccessors(current_state):
                new_cost = cost + step_cost
                if successor not in visited or new_cost < visited.get(successor, float('inf')):
                    fringe.push((successor, path + [action], new_cost), new_cost)
    return []
```
### 4. A* Search
**Strategy**: Combine path cost and heuristic estimate\
**Implementation**: Priority Queue with f(n) = g(n) + h(n)\
**Characteristics**:
 - Optimal with admissible heuristic
 - Most efficient informed search
 - f(n) = actual cost + heuristic estimate to goal
```python
def aStarSearch(problem, heuristic=nullHeuristic):
    from util import PriorityQueue
    fringe = PriorityQueue()
    fringe.push((problem.getStartState(), [], 0), 0)
    visited = {}
    
    while not fringe.isEmpty():
        current_state, path, cost = fringe.pop()
        if problem.isGoalState(current_state):
            return path
        if current_state not in visited or cost < visited[current_state]:
            visited[current_state] = cost
            for successor, action, step_cost in problem.getSuccessors(current_state):
                new_cost = cost + step_cost
                priority = new_cost + heuristic(successor, problem)
                if successor not in visited or new_cost < visited.get(successor, float('inf')):
                    fringe.push((successor, path + [action], new_cost), priority)
    return []
```
## 📊 Algorithm Comparison
| Algorithm |	Data Structure | Optimal? | Complete? | Time Complexity | Space Complexity |
| --------- | -------------- | -------- | --------- | --------------- | ---------------- |
| DFS |Stack|No|No(infinite trees)|O(b^m)|O(bm)|
| BFS |Queue|Yes(step cost=1)|Yes|O(b^d|O(b^d)|
| UCS |Priority Queue|Yes|Yes|O(b^(c*/ε))|O(b^(C*/ε))|
| A* |Priority Queue|Yes (with admissible h)|Yes|O(b^d)|O(b^d)|

Where:
 - b = branching factor
 - d = depth of solution
 - m = maximum depth
 - C* = cost of optimal solution
 - ε = minimum step cost
## 🚀 Running the Code
```bash
# Test specific algorithms
python pacman.py -l mediumMaze -p SearchAgent -a fn=dfs
python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs  
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs
python pacman.py -l mediumMaze -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic

# Compare algorithm performance
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic
```



