# Robotics Assignment 1
- Lewis Worton - c1014451

In my project, the bot autonomously maps the maze, finds the shortest path to the exit using breadth-first search and then returns home.

## Bot Process
The bot follows these 3 main steps:

1. Mapping the maze – The bot explores and stores walls and visited locations as it moves through the maze.
2. Finding the exit – It calculates the shortest path from its position after mapping to the exit using BFS.
3. Returning home – After reaching the exit, it calculates and follows the shortest path back to the start position.

## 1. Mapping the Maze (mapMaze(bot))
The bot explores the maze by following the left walls of the maze. The maze can be seen as an 8x8 grid of 250mm squares to allow for a co-ordinates system to be used.
- The bot starts at (0, 0) and turns left.
- If the bot is in an area not visited before, it will check left and right for walls.
- If it encounters a wall straight ahead, it turns right.
- If it encounters another wall, it turns right again.
- If it detects an edge, it does a 180° turn to prevent falling.
- The bot will continue this until the exit is found and ~90% of the maze has been mapped.

The bot stores the following information within its class:
- Visited positions - stored as co-ordinates in a set.
- Walls - stored as pairs of co-ordinates in a set. A pair indicates the bot cannot travel between those 2 co-ordinates to simulate a wall.
- Start and exit location - stored as co-ordinates.
- Directions - an array of possible directions for the bot, stored as co-ordinates where 1 / -1 indicates a movement and 0 indicates no movement. e.g. (0, 1) would be upwards in the maze.
- Current direction - an integer between 0 and 3, allows multiplication by 90 to get a heading.
- Position - current position of the bot as co-ordinates.

Once the bot has returned home, it will then generate a map of the maze using the data it gathered during mapping.
- To allow space for the walls, the map has to be larger than the maze grid.
- Functions transform the original maze co-ordinates and expand them to fit on a 17x17 grid.
- This expansion leaves gaps between the original and transformed co-ordinates which are then filled in by checking adjacent co-ordinates.

One of the mazes has a bug where a wall is not correctly marked as a wall, this means that the bot does not detect it and will pass through it.

### Example maze map:

 ![image](https://github.com/user-attachments/assets/0c8fa310-686a-4656-8bd8-e4f13d3249b8)

 The map will display unvisited locations as walls.

## 2. Calculating the Shortest Path (calcShortestPath(bot, target))
Once mapping is complete, the bot finds the shortest path to the exit using breadth-first search.
- The bot starts from the location it was when it finished mapping.
- The bot attempts to move in all 4 possible directions.
- If a move is not blocked by a wall and has been visited by the bot but not during BFS search, it adds it to the queue.
- It iterates through the queue until a valid path is found or all possible paths are checked.

As the grid the robot uses can be represented as an unweighted graph, BFS is an ideal algorithm for finding the shortest path:
- BFS explores all nodes level by level so if a path exists the first time it reaches the target will be the shortest route.
- By keeping track of which positions have already been visited, it prevents unnecessary revisiting and wasting time on the same paths.
- BFS can be scaled up for larger mazes more efficiently as it explores the shortest paths first.

## 3. Following the Paths and Returning Home (followShortestPath(bot, path))
When an exit path has been found, the bot follows the path and then generates another path from the exit to its start position and follows it home.
- The bot iterates through each position in the path and calculates the difference between its current co-ordinates and the next in the path.
- As the bot only moves 1 square at a time, the difference will be one of the possible directions the bot can move.
- The bot checks that it is a valid direction and converts the co-ordinate difference to a current direction integer stored within it.
- The current direction integer is then multiplied by 90 to get the heading to turn to.
- The bot then moves forward with a simpler function as it is moving through areas it has already mapped.
