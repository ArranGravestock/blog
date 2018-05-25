---
layout:     post
title:      JavaScript Canvas A* Path Finding 
date:       2018-05-19
summary:    AI techniques in Games using JavaScript
categories: ai javascript canvas a* prim algorithm maze generation 
---

# TL;DR Algorithm
The algorithm I used is based on the pseudo code on [wikipedia](https://en.wikipedia.org/wiki/A*_search_algorithm), particular parts of the algorithm are tedious in JavaScript comparing to Python. For example, searching through an array for the lowest value requires checking every element. It is possible to use another data structure such as heap to solve this issue, using ‘.pop()’ instead. A* requires a start and finish node in order to calculate a path. An open and closed set contain the values that have not been evaluated and evaluated respectively, where only the start node is evaluated originally. Every neighbour to the node (north, east, south and west, no diagonals included) is added to the open set and evaluated by adding the distance from the start node and end node (known as f heuristic) provided it is not in the contained set. The current position is then compared to the lowest value of f, if it is lower then the node is closer to the goal. This is repeated until the goal has been found.

# Choosing the Right Algorithm
There are many path finding techniques that are used which all have their uses, such as breadth-first search, best first search & backtracking. Between A* and best-first I had chosen to implement the A* algorithm. A* is simple to implement, efficient & fast but hard to tweek - making it important to ensure that changes are made as early as possible. The more complex the program gets, the harder it is to ensure test conditions are the same and that other factors affect the search algorithm. Most search techniques base their implementation on a grid search.

# JavaScript Implementation

## Initialising
For A* to calculate a path the start node (where the AI's current position is) and the end or goal node (target, usually the player). As well as this, the grid size also helps to ensure that the path is being calculated within the barriers of the world we have created. 

```javascript
let world_width = world[0].length
let world_height = world.length
let world_size = world_width * world_height
let node
let walkable = 0
```

## Choosing your heuristic
Depending on how you want your AI to move the restriction is made by the heuristic, in the example below I use the ```Manhattan Distance``` calculation to analyse the heuristic - this algorithm only allows movement in NESW - no diagonal movement is allowed. Other options include Euclidean & diagonal only. 
```javascript
function ManhattanDistance(start, end) {
        return (Math.abs(start.x - end.x) + Math.abs(start.y - end.y))
}
```

## Finding a node's neighbours
As each node around the neighbour has to be evaluated, a function can be used to retrieve all the nodes associated in the particular distance (can be modified to include diagonals if using another heuristic). Each position is tested around the node, whilst ensuring it belongs to the grid.

```javascript
function getNeighbours(node, max) {
    let neighbours = []
    let x = node.x
    let y = node.y
    let value = node.value

    if (y-1 >= 0 && world[x][y-1].value < max) {
        neighbours.push(world[x][y-1])
    }
    if (y+1 < world_height && world[x][y+1].value < max) {
        neighbours.push(world[x][y+1])
    }
    if (x-1 >= 0 && world[x-1][y].value < max ) {
        neighbours.push(world[x-1][y])
    }
    if (x+1 < world_width && world[x+1][y].value < max) {
        neighbours.push(world[x+1][y])
    }
        
    return neighbours
}
```

## Finding the path
The assumption is that your start node's heuristic evalulation is lowest (infinite) as this the path it is currently on - furtest away from the player. Two sets, an open and closed are used to evaluate and store the node weight. The open is the nodes which have not been explored whilst the closed are the set in which has been evaluated. 

```javascript
function findPath (start, end, grid) {
    let start_x = start.x
    let start_y = start.y
    let end_x = end.x
    let end_y = end.y

    let closed_set = []
    let open_set = []
        
    let end_node = new Node(end_x, end_y, false, 0) //create a new node with the end position
    let start_node = new Node(start_x, start_y, false, 0) //create a new node with the start position

    start_node.g = 0 //set the start evaluation to infinite
    start_node.f = 0
        
    open_set.push(start_node) //push the starting position onto the open set to be evaluated
        
    //while set is not empty
       //...
       
    return path [] //path is not found return empty result
}
```

### Comparing heuristics
Following on, now the open set needs to evaluated whilst there is always a result. The lowest f (combination of the distance from start node and distance from end node) is evaluated in the list first (as this is the best move possible at the moment), and as it is now being evaluated it can be removed from the open list and added to the closed. The code for this uses native JS data structures but would be extremely more efficient if using a heap or priority queue (each node in the list is iterated until found). If the current position is the end (goal) then the path has been found, return the reverse of the path list (as .push reverses the order of the stack). Otherwise, the evaluated node's (provided they have not already been evaluated) potential path positions are added to the open set, so that they can be evaluated. The g is now calculated for this node, and the parent is the previous node (used so that the path can be stored) using Manhattan Distance.

```javascript
while (open_set.length > 0) {
     
     //dummy the first node to compare
     let node = open_set[0]
     
     //find the lowest f in the set
     for (let tile in open_set) {
           node = (open_set[tile].f < node.f) ? open_set[tile] : node
     }
     
     //remove the lowest f from the openset
     for (let i = 0; i < open_set.length; i++) {
          if (node == open_set[i]) {
               open_set.splice(i, 1)
               break
          }
     }

     //add the lowest node to the closedset
     closed_set.push(node)

     //if matching paths then target found
     if (node == world[end_x][end_y]) {
          let path = [{x: node.x, y: node.y}]
          while (node.parent) {
               node = node.parent
               path.push({x: node.x, y: node.y})
          }
          return path.reverse()
     }

     //get the neighbours for the node
     let neighbours = getNeighbours(node, 1)

     //for every neighbour
     for (let i = 0; i < neighbours.length; i++) {
     
     let neighbour = neighbours[i]

     if (closed_set.includes(neighbour)) {
         continue
     }

     let x = neighbour.x
     let y = neighbour.y

     let g = node.g + node.value * ManhattanDistance(neighbour, end_node)

     if (!open_set.includes(neighbour) || g < neighbour.g) {
                    
         neighbour.g = g
         neighbour.h =  neighbour.value * ManhattanDistance(neighbour, end_node)
         neighbour.f = neighbour.g + neighbour.h
         neighbour.parent = node

         if (!open_set.includes(neighbour)) {
             open_set.push(neighbour)
         } else {
             open_set[i] = neighbour
         }
    }
}
```

That's it! An implementation of this can be found on my (github)[https://github.com/ArranGravestock/path-finding-aiie/]. This is a basic implementation of A* path finding, however it can be modified to suit your needs depending on the efficiency. I would recommend reading (cooperative path finding by David Silver)[http://www0.cs.ucl.ac.uk/staff/D.Silver/web/Publications_files/coop-path-AIWisdom.pdf] as an it provides techniques that help to save resources and increase the 'intelligence' of your enemies, through means such as path sharing, limiting depth search & choosing the right heuristic. 
