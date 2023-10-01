## Breadth-First Search (BFS) Algorithm Explanation with Example

**Breadth-First Search (BFS)** is an algorithm used for traversing or searching tree or graph data structures. It explores all the vertices (or nodes) at the current level before moving to the next level. BFS is often used to find the shortest path between two nodes or to visit all nodes in a graph.

### Example:

Consider a simple graph with nodes and connections as shown below:

```
A -- B
|    | 
C -- D -- E
| 
F -- G
```

In this graph:
- Nodes A, B, C, D, E, F, and G are connected by edges.
- We want to find the shortest path from node A to node E using the BFS algorithm.

### BFS Step-by-Step:

1. **Initialization**:
   - Start at the source node (A).
   - Create an empty queue and enqueue the source node (A).
   - Mark A as visited.

2. **Level 1**:
   - Dequeue A.
   - Explore its neighbors (B and C).
   - Enqueue unvisited neighbors (B and C).

3. **Level 2**:
   - Dequeue B.
   - Explore its neighbor (D).
   - Enqueue D (not visited yet).
   - Dequeue C.
   - Explore its neighbor (D).
   - D is already enqueued, so skip it.

4. **Level 3**:
   - Dequeue D.
   - Explore its neighbors (E).
   - Enqueue E (not visited yet).

5. **Level 4**:
   - The queue is now [E] and that is our endpoint.

6. **Path Reconstruction**:
   - We have found the shortest path from A to E: A -> B -> D -> E.
### BFS Algorithm in C++:

Here's a C++ implementation of the BFS algorithm:

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <unordered_map>

using namespace std;

unordered_map<char, vector<char>> graph;

vector<char> bfs(char start, char end) {
    queue<vector<char>> q;
    q.push({start});
    unordered_map<char, bool> visited;
    visited[start] = true;

    while (!q.empty()) {
        vector<char> path = q.front();
        q.pop();
        char node = path.back();

        if (node == end) {
            return path;
        }

        for (char neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                vector<char> new_path = path;
                new_path.push_back(neighbor);
                q.push(new_path);
            }
        }
    }

    return {};
}

int main() {
    // Define the graph
    graph['A'] = {'B', 'C'};
    graph['B'] = {'A', 'D'};
    graph['C'] = {'A', 'D', 'F'};
    graph['D'] = {'B', 'C', 'E', 'G'};
    graph['E'] = {'D'};
    graph['F'] = {'C'};
    graph['G'] = {'D'};

    char start = 'A';
    char end = 'E';

    vector<char> shortest_path = bfs(start, end);

    cout << "Shortest path from " << start << " to " << end << ": ";
    for (char node : shortest_path) {
        cout << node << " ";
    }
    cout << endl;

    return 0;
}
```

## Breadth-First Search (BFS) Algorithm in Maze Solving

Let's walk through a simple example of how the Breadth-First Search (BFS) algorithm can be used to find a path in a maze.

### Maze Example

Consider the following 5x5 maze where:
- "S" represents the start cell.
- "E" represents the endpoint (goal).
- "X" represents walls (obstacles).
- "." represents open cells:
```
S . . . .
X X . X .
. . . . .
. X X X .
E . . . .

```

In this maze, the objective is to find the shortest path from the start cell "S" to the endpoint "E" while avoiding the obstacles (represented by "X").

### Applying BFS

Let's apply the BFS algorithm to find the shortest path in this maze:

1. **Initialization**:
   - Start at the "S" cell.
   - Initialize a queue and enqueue the start cell.
   - Initialize a visited array to keep track of visited cells.

2. **Exploration**:
   - Dequeue the start cell from the queue and mark it as visited.
   - Explore its neighboring cells (up, down, left, right, diagonals).
   - Enqueue neighboring open cells into the queue that haven't been visited yet.
   - Repeat this process, exploring cells level by level, until the endpoint "E" is reached.

3. **Breadth-First Nature**:
   - BFS explores cells in a breadth-first manner. It first explores cells at distance 1 from the start cell before moving to cells at distance 2 and so on.
   - This ensures that the shortest path is found when the endpoint "E" is reached.

4. **Stopping Condition**:
   - BFS continues until the endpoint "E" is found or until there are no more cells to explore.

5. **Path Reconstruction (if endpoint found)**:
   - When the endpoint "E" is reached, BFS backtracks from "E" to "S" using the recorded parent information to reconstruct the shortest path.
   - The reconstructed path represents the shortest way to reach "E" from "S" while avoiding obstacles.

### Shortest Path

In this example, if BFS is applied, it will find the following shortest path from "S" to "E":
```
S * . . .
X X * X .
. * . . .
* X X X .
E . . . .

```

Here, "*" characters represent the shortest path. This path was found by BFS, which explored cells in a breadth-first manner, ensuring that the path is indeed the shortest possible path in the maze.
### BFS Algorithm in C++:

Here's a C++ implementation of the BFS algorithm:

```cpp
#include <iostream>
#include <vector>
#include <random>
#include <Windows.h>
#include <conio.h>
#include <queue>
#include <stack>
using namespace std;
int row, col,rowend=-2,colend=-2;
vector<vector<bool>>visted(30, vector<bool>(30, false));
void display_vector(vector<vector<int>>& vec) {
	system("cls");
	for (auto items : vec) {
		for (auto item:items)
		{
			if(item==2){
			
				const char martini[5] = { 0xF0, 0x9F,  0xA4, 0x96, '\0' };
				printf(martini);
				cout << " ";
				
			}
			else if (item == -1) {
				const char martini[5] = { 0xF0, 0x9F,  0xA7, 0xB1, '\0' };
				printf(martini);
				cout << " ";
			}
			else if (item == 3) {
				const char martini[5] = { 0xF0, 0x9F,  0x9B, 0x91, '\0' };
				printf(martini);
				cout << " ";
			}
			else {
				//0xF0 0x9F 0x8C 0xB1
				const char martini[5] = { 0xF0, 0x9F,  0x8C, 0xB1, '\0' };
				printf(martini);
				cout << " ";
			}
			}
		cout << '\n';
	}
}
void display_vectorSelected(vector<vector<int>>& vec, int& x, int& y) {
	system("cls");
	for (int i = 0;i < vec.size();i++) {
		for (int j=0;j<vec.size();j++)
		{
			HANDLE consoleHandle = GetStdHandle(STD_OUTPUT_HANDLE);
			if (i == x && j == y) {

				// Set background color to grey
				SetConsoleTextAttribute(consoleHandle, BACKGROUND_INTENSITY | BACKGROUND_RED | BACKGROUND_GREEN | BACKGROUND_BLUE);

			}
			else {
				SetConsoleTextAttribute(consoleHandle, FOREGROUND_INTENSITY | FOREGROUND_RED | FOREGROUND_GREEN | FOREGROUND_BLUE);

			}
			if (vec[i][j] == 2) {

				const char martini[5] = { 0xF0, 0x9F,  0xA4, 0x96, '\0' };
				printf(martini);
				cout << " ";

			}
			else if (vec[i][j] == -1) {
				const char martini[5] = { 0xF0, 0x9F,  0xA7, 0xB1, '\0' };
				printf(martini);
				cout << " ";
			}
			else if (vec[i][j] == 3) {
				const char martini[5] = { 0xF0, 0x9F,  0x9B, 0x91, '\0' };
				printf(martini);
				cout << " ";
			}
			else {
				const char martini[5] = { 0xF0, 0x9F,  0x8C, 0xB1, '\0' };
				printf(martini);
				cout << " ";
			}
		}
		cout << '\n';
	}
}
void drawTheWay(vector<vector<int>>& vec, stack < pair<int, int>>&st) {
	if (st.empty()) {
		cout << "can not solve it\n";
		return;
	}
	else {
		while (!st.empty()) {
			vec[st.top().first][st.top().second] = 4;
			st.pop();
		}
	}
	for (int i = 0;i < vec.size();i++) {
		for (int j = 0;j < vec.size();j++)
		{
			if (vec[i][j] == 2) {

				const char martini[5] = { 0xF0, 0x9F,  0xA4, 0x96, '\0' };
				printf(martini);
				cout << " ";

			}
			else if (vec[i][j] == -1) {
				const char martini[5] = { 0xF0, 0x9F,  0xA7, 0xB1, '\0' };
				printf(martini);
				cout << " ";
			}
			else if (vec[i][j] == 3) {
				const char martini[5] = { 0xF0, 0x9F,  0x9B, 0x91, '\0' };
				printf(martini);
				cout << " ";
			}
			else if (vec[i][j] == 4) {
				const char martini[5] = { 0xF0, 0x9F,  0x9F, 0xA2, '\0' };
				printf(martini);
				cout << " ";
			}
			else {
				
					const char martini[5] = { 0xF0, 0x9F,  0x8C, 0xB1, '\0' };
					printf(martini);
					cout << " ";
				
				}
		}
		cout << '\n';
	}
}
void addRobot(vector<vector<int>>& vec) {
	// random
	random_device rd;  // Seed the random number generator
	mt19937 gen(rd()); // Mersenne Twister PRNG
	uniform_int_distribution<int> distribution(0, 29); // Generate integers between 1 and 29
	// Generate and print a random number
	int x = distribution(gen);
	int y = distribution(gen);
	vec[x][y] = 2;
	row = x;
	col = y;
	display_vector(vec);

}
void BFS(vector<vector<int>>& vec) {
	queue<pair<int, int>>childqueue;
	int dx[]{ -1,1,0,0,1,-1,1,-1 };
	int dy[]{ 0,0,-1,1,1,-1,-1,1 };
	pair<int, int>root;
	vector<vector<pair<int, int>>>way(30,vector<pair<int,int>>(30));
	root.first = row;
	root.second = col;
	childqueue.push(root);
	visted[row][col] = true;
	stack<pair<int, int>>st;

	bool stop = false;
	while (!childqueue.empty()) {
		pair<int, int>current = childqueue.front();
		childqueue.pop();
		for (int i = 0;i < 8;i++) {
			int new_x=current.first+dx[i];
			int new_y = current.second + dy[i];
			if (new_x <= 29 && new_x >= 0 && new_y <= 29 && new_y >= 0) {
				if (!visted[new_x][new_y]) {
					visted[new_x][new_y]=true;
					pair<int, int>child;
					child.first = new_x;
					child.second = new_y;
					childqueue.push(child);
					way[new_x][new_y] = current;
				
					if (new_x == rowend && new_y == colend) {
						stop = true;
						st.push(child);
						break;
					}
				}
			}
			
		}
		

	
	}
	while (stop) {
		pair<int, int>parent = way[rowend][colend];
		if (parent.first == row && parent.second == col)
		{
			break;
		}
		st.push(parent);
		rowend = parent.first;
		colend = parent.second;

	}
	drawTheWay(vec, st);
}
int main()
{
	SetConsoleOutputCP(CP_UTF8);

	vector<vector<int>>vec(30,vector<int>(30,0));
	display_vector(vec);
	while (true)
	{
		cout<<("enter 1 to add robot random: \n");
		int add;
		cin >> add;
		if (add == 1) {
			addRobot(vec);
			break;
		}
	}
	int x = 0, y = 0;
	while (true)
	{
		char key;
		key = _getch();
		
		switch (key) {
		case 13:
			if (row != x || col != y)
				if (vec[x][y] == -1) {
					vec[x][y] = 0;
					visted[x][y] = false;
				}
				else {
					vec[x][y] = -1;
					visted[x][y] = true;
				}
				display_vectorSelected(vec,x,y);
				break;
			case 72: // Up arrow
				if (x != 0)
					x--;
				display_vectorSelected(vec, x, y);

				break;
			case 80: // Down arrow				
				if (x != 29)
					x++;
				display_vectorSelected(vec, x, y);

				break;
			case 75: // Left arrow
				if (y != 0)
					y--;
				display_vectorSelected(vec, x, y);
				break;
			case 77: // Right arrow
				if (y != 29)
					y++;
				display_vectorSelected(vec, x, y);
				break;
			case 27:
				if (row != x || col != y)
					if (vec[x][y] == 3) {
						rowend = -2;
						colend = -2;
						vec[x][y] = 0;
					}
					else {
						if (rowend == -2) {
							vec[x][y] = 3;
							rowend = x;
							colend = y;
						}
					}
				display_vectorSelected(vec, x, y);

				break;
			default:
				
				break;
			
		
			}
			cout << ("click 0 to Start to solve that:\n");
			if (key == '0')
				if (rowend == -2)
					cout << "add end point:\n";
				else	
					break;
				//break;
	}
	BFS(vec);
	//display_vector(vec);


	
	
}
```
### Explain maze code 

- The maze is represented as a 30x30 grid using a 2D vector called `vec`.
- Each cell in the grid can have one of the following values:
  - `0`: Represents an open space.
  - `-1`: Represents an obstacle or wall.
  - `2`: Indicates the robot's initial position.
  - `3`: Marks the endpoint (goal) within the maze.
  - `4`: Signifies the path from the robot to the endpoint.

### Initialization

- The program initializes the maze grid with all cells set to `0`, indicating open spaces.
- It allows users to add a robot to a random position within the maze.

### User Interaction

- Users can navigate through the maze using arrow keys (Up, Down, Left, Right) to select a cell and toggle it between an obstacle and an open space.
- To add an endpoint (`3`), users press the `Esc` key when selecting a cell. Only one endpoint can be set at a time.
- The program waits for the user to press `0` to initiate the BFS algorithm once the endpoint is set.

### Breadth-First Search (BFS)

- The BFS algorithm is implemented within the `BFS` function.
- It uses a queue (`child queue`) to explore cells in a breadth-first manner.
- Upon completion, BFS traces the path from the endpoint back to the robot's position using a stack (`st`) and marks this path with `4` in the `vec` grid.

### Display Functions

- Several functions (`display_vector` and `display_vectorSelected`) are used to display the maze grid.
- These functions highlight the current position, obstacles, the robot, and the endpoint.
- Another function (`drawTheWay`) is employed to display the final path found by BFS.

### User Input Handling

- The program captures user input for navigation and interaction using `_getch`.

### Miscellaneous

- The code sets the console output to use UTF-8 characters to correctly display Unicode symbols.
![Screenshot 2023-10-01 125646](https://github.com/HeshamHM/Maze/assets/65486855/ec372feb-e11e-465d-9d61-b7c8d23aa3d6)
![Screenshot 2023-10-01 125703](https://github.com/HeshamHM/Maze/assets/65486855/407a9b4a-c3d2-4ea8-a4db-6e4792be5f10)
![Screenshot 2023-10-01 125738](https://github.com/HeshamHM/Maze/assets/65486855/063a27bd-2408-4ac0-b836-4c4b658e5b25)
![Screenshot 2023-10-01 125757](https://github.com/HeshamHM/Maze/assets/65486855/de33c3ba-12f0-48e5-8a1e-69218e3541e5)






### Summary:

- BFS explores nodes level by level, guaranteeing that the first path to a destination is the shortest.
- It uses a queue to maintain the order of exploration.
- BFS can be applied to graphs, trees, or even grid-based problems like maze solving.
- It is useful for finding the shortest paths, connected components, and more.


