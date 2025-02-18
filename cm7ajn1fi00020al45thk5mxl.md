---
title: "Breadth-First Search (BFS) in Graphs"
datePublished: Tue Feb 18 2025 13:52:15 GMT+0000 (Coordinated Universal Time)
cuid: cm7ajn1fi00020al45thk5mxl
slug: breadth-first-search-bfs-in-graphs
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/npxXWgQ33ZQ/upload/b5966445feed15ae54e7114f2d338f99.jpeg
tags: computer-science, data-structure-and-algorithms

---

Breadth-First Search (BFS) is a fundamental graph traversal algorithm that explores nodes level by level. It is commonly used for finding the shortest path, network broadcasting, and solving various graph-related problems.

## Understanding BFS

BFS starts from a given node (source) and explores all its neighbors before moving to the next level. It uses a **queue** to track nodes to be visited and a **set** to prevent revisiting nodes.

### BFS Implementation

Here’s a correct implementation of BFS:

```javascript
function bfs(adjacencyList) {
    let visitedQueue = [];  // for BFS traversal
    let resultQueue  = []; // store the result
    let visited = new Set(); // avoid revisits

    let nodes = Object.keys(adjacencyList);
    let startNode = nodes[0];

    visitedQueue.push(startNode);
    visited.add(startNode);

    while (visitedQueue.length > 0) {
        let currentNode = visitedQueue.shift();
        resultQueue.push(currentNode);

        adjacencyList[currentNode].forEach(neighbour => {
            if (!visited.has(neighbour)) {
                visitedQueue.push(neighbour);
                visited.add(neighbour);
            }
        });
    }

    console.log(resultQueue);
}
```

---

## LeetCode Problem: Find If Path Exists in Graph

### Problem Statement

Given `n` nodes labeled from `0` to `n - 1` and a list of `edges`, check if there is a valid path between `source` and `destination`.

### Approach 1: Using an Object-Based Adjacency List

This approach creates an adjacency list using an object and performs BFS to find a path.

```javascript
var validPath = function (n, edges, source, destination) {
    let adjList = {};
    if (source === destination) return true;

    for (let [from, to] of edges) {
        if (!(from in adjList)) adjList[from] = [];
        if (!(to in adjList)) adjList[to] = [];
        adjList[from].push(to);
        adjList[to].push(from);
    }

    let visited = new Set();
    let visitedQueue = [source];
    visited.add(source);

    while (visitedQueue.length > 0) {
        let currentNode = visitedQueue.shift();
        if (currentNode === destination) return true;

        adjList[currentNode]?.forEach(neighbour => {
            if (!visited.has(neighbour)) {
                visited.add(neighbour);
                visitedQueue.push(neighbour);
            }
        });
    }

    return false;
};
```

### Approach 2: Optimized Using `Map`

Instead of using an object, we use a `Map` for better performance in large datasets.

```javascript
var validPath = function (n, edges, source, destination) {
    if (source === destination) return true;

    let adjList = new Map();
    for (let [from, to] of edges) {
        if (!adjList.has(from)) adjList.set(from, []);
        if (!adjList.has(to)) adjList.set(to, []);
        adjList.get(from).push(to);
        adjList.get(to).push(from);
    }

    let visited = new Set();
    let visitedQueue = [source];
    visited.add(source);

    while (visitedQueue.length > 0) {
        let currentNode = visitedQueue.shift();
        if (currentNode === destination) return true;

        for (let neighbor of (adjList.get(currentNode) || [])) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                visitedQueue.push(neighbor);
            }
        }
    }

    return false;
};
```

---

### Summary

* **BFS explores nodes level by level**, making it ideal for shortest path problems.
    
* We use a **queue** to process nodes and a **set** to avoid revisiting.
    
* **Adjacency lists** are efficient for representing graphs, and using `Map` can optimize performance in some cases.
    

Thank you for reading! I hope this gave you a clear understanding of BFS. In the next blog, we’ll dive into graph traversal technique DFS.

💬 **Let’s connect!**

[**Twitter**](https://twitter.com/karthik_coder)  
[**Instagram**](https://www.instagram.com/coding_nemo)

![final](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbTd1c2I3NGFrYmxvYnB3ZGJqMnphYWM4eDhrODd2NHp0bHk0dms3MiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/lOJKLVYkNDWN8GoPoA/giphy.gif align="left")