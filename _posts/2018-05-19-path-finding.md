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

