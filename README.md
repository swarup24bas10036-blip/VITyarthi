# VITyarthiinstructions
## I. Agent and Environment Design

### 1. Environment Representation
* **2D Grid:** Represent the city as a 2D grid. Each cell $(x, y)$ on the grid represents a location.
* **Static Obstacles:** Use a value (e.g., -1 or a specific symbol like '#') to mark cells that are impassable.
* **Varying Terrain Costs:** Assign a positive integer cost to each non-obstacle cell. For example, a flat road could have a cost of 1, a dirt path a cost of 2, and a crowded street a cost of 3. These costs represent the "fuel" or "time" required to traverse that cell.
* **Dynamic Obstacles:** Represent moving obstacles as a list of coordinates and times. For example, a list of tuples $(x, y, t)$ indicating that a dynamic obstacle is at position $(x, y)$ at time $t$. The agent must be able to replan if it encounters one.

### 2. Agent State and Actions
* **State:** The agent's state can be defined by its current position $(x, y)$. For dynamic environments, the state could also include time, i.e., $(x, y, t)$.
* **Actions:** The agent can move to adjacent cells (up, down, left, right, and possibly diagonals). Each action has a cost equal to the terrain cost of the destination cell.

---

## II. Algorithm Implementation

### 1. Uninformed Search
* **Breadth-First Search (BFS):** Implement BFS to find the shortest path in terms of the number of steps. This is a good choice for unweighted grids (all costs are 1).
    * **Procedure:** Use a queue to explore nodes level by level.
* **Uniform-Cost Search (UCS):** Implement UCS to find the path with the minimum cumulative cost. This is the optimal uninformed search algorithm for weighted graphs.
    * **Procedure:** Use a priority queue to always expand the node with the lowest cumulative cost from the start.

### 2. Informed Search
* **A\* Search:** Implement A* with an **admissible heuristic**. An admissible heuristic never overestimates the true cost to the goal.
    * **Procedure:** A* uses a priority queue ordered by $f(n) = g(n) + h(n)$, where $g(n)$ is the cost from the start to node $n$, and $h(n)$ is the heuristic estimate of the cost from node $n$ to the goal.
    * **Heuristic Example:** For a grid, the **Manhattan distance** or **Euclidean distance** are common admissible heuristics.
        * Manhattan distance: $h(n) = |n_x - goal_x| + |n_y - goal_y|$
        * Euclidean distance: $h(n) = \sqrt{(n_x - goal_x)^2 + (n_y - goal_y)^2}$

### 3. Local Search Replanning
* **Objective:** Handle dynamic obstacles. When the agent's planned path becomes invalid (e.g., due to a new obstacle appearing), it must replan.
* **Replanning Strategy (Example: Hill-Climbing with Random Restarts):**
    * **Hill-Climbing:** At a local search, the agent explores a limited number of "neighboring" paths and picks the one with the best (lowest) cost. It continues this process until no better path is found in the immediate vicinity.
    * **Random Restarts:** If the agent gets stuck in a local optimum (a path that's good but not the best overall), it can abandon the current search and start a new one from a random, valid state to try and find a better solution.

---

## III. Experimental Comparison

### 1. Map Instances
* **Design a few different maps:**
    * A simple, low-cost map.
    * A complex map with many obstacles.
    * A map with significant variation in terrain costs.
    * A map with one or more dynamic obstacles that appear after the initial path is planned.

### 2. Data Collection
* **For each map and each algorithm (BFS, UCS, A*, and a replanning scenario):**
    * **Path Cost:** The final cumulative cost of the path taken by the agent.
    * **Nodes Expanded:** The total number of grid cells or nodes explored by the algorithm.
    * **Time:** The execution time of the algorithm.

---

## IV. Analysis and Reporting

### 1. Experimental Results
* Present the collected data in a clear format, like a table.
* Include columns for "Algorithm," "Map Instance," "Path Cost," "Nodes Expanded," and "Time."

### 2. Performance Analysis
* **Uninformed vs. Informed:**
    * Explain that **BFS** is optimal for unweighted graphs but inefficient for weighted graphs. **UCS** is optimal for weighted graphs but can be slow as it expands nodes in all directions.
    * Explain that **A*** is generally the most efficient and practical for pathfinding. It combines the guarantee of finding an optimal path (like UCS) with a heuristic to intelligently guide the search toward the goal, thus exploring fewer nodes.

* **Replanning vs. Static Planning:**
    * Explain why a **local search replanning strategy** is essential for dynamic environments. While initial A* provides a great path, it's brittle if the environment changes.
    * Explain that replanning allows the agent to be **robust** and **adaptive** to unforeseen events like a road closure or a moving vehicle, even if the replanning path isn't globally optimal.

* **Choosing the Right Algorithm:**
    * **Simple, unweighted maps:** BFS is simple and efficient.
    * **Weighted, static maps:** A* is the best choice for finding the optimal path efficiently.
    * **Dynamic, changing maps:** A* for initial planning combined with a local search replanning strategy is the most effective approach.
