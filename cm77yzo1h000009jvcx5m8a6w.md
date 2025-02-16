---
title: "Understanding Graphs in JavaScript: Adjacency Matrix vs. Adjacency List"
datePublished: Sun Feb 16 2025 18:38:40 GMT+0000 (Coordinated Universal Time)
cuid: cm77yzo1h000009jvcx5m8a6w
slug: understanding-graphs-in-javascript-adjacency-matrix-vs-adjacency-list
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/q10VITrVYUM/upload/19deb1cd2123c6dbfb905b7e2585bd55.jpeg
tags: computer-science, datastructure, datastructuresandalogrithm

---

## Introduction

Graphs are one of the most fundamental data structures in computer science. They consist of nodes (also called vertices) connected by edges. Graphs are widely used in various real-world applications, such as social networks, navigation systems, and recommendation engines.

In this post, we'll explore two common ways to represent graphs: **Adjacency Matrix** and **Adjacency List**. We'll also implement both representations in JavaScript and compare their efficiency.

## Types of Graph Representations

### 1\. Adjacency Matrix

An adjacency matrix is a 2D array (or matrix) that represents connections between nodes. If there is an edge between node `i` and node `j`, the matrix at position `[i][j]` contains `1` (or the weight of the edge, in case of weighted graphs). Otherwise, it contains `0`.

#### Implementation in JavaScript

```javascript
class Graph {
    constructor(numNodes) {
        this.matrix = [];
        for (let i = 0; i < numNodes; i++) {
            this.matrix.push(new Array(numNodes).fill(0));
        }
    }

    addEdge(to, from) {
        if (!this.isEdge(to, from) && !this.isEdge(from, to)) {
            this.matrix[to][from] = 1;
            this.matrix[from][to] = 1;
        } else {
            console.log("Edge already exists", to, from);
        }
    }

    removeEdge(from, to) {
        this.matrix[from][to] = 0;
        this.matrix[to][from] = 0;
    }

    isEdge(from, to) {
        return this.matrix[from][to] === 1;
    }
}

const graph = new Graph(3);
graph.addEdge(0, 1);
graph.addEdge(0, 2);
console.log(graph.matrix);

/*
Output : 
[
  [0, 1, 1],
  [1, 0, 0],
  [1, 0, 0]
]
*/
```

### 2\. Adjacency List

An adjacency list represents a graph using a **hash map (object in JavaScript)** where each key (node) stores an array of connected nodes.

#### Implementation in JavaScript

```javascript
// adjacency list ( unweighted and undirected graph )
/**
 * {
  1: [2, 3],
  2: [1],
  3: [1]
}
 */

class AdjacencyList {
    constructor() {
        this.list = {}
    }

    addNode(node) {
        if(this.list[node]){
            console.log(`${node} already exists`);
            return false; 
        }
        this.list[node] = []
        return true
    }

    addEdge(from, to) {
        if (!this.list[from] || !this.list[to]) {
            console.log(`One or both nodes (${from}, ${to}) do not exist.`);
            return;
        }

        if(!this.isEdge(from, to)){
            this.list[from].push(to);
            this.list[to].push(from);
        }
    }

    removeEdge(from, to) {
        if (this.list[from]) {
            this.list[from] = this.list[from].filter(node => node !== to)
        }

        if (this.list[to]) {
            this.list[to] = this.list[to].filter(node => node !== from)
        }
    }

    isEdge(from, to) {
        //return this.list[from]?.includes(to) || false;
        return this.list[from] ? this.list[from].indexOf(to) !== -1 : false
    }
}

const newGraph = new AdjacencyList();
newGraph.addNode(1)
newGraph.addNode(2)
newGraph.addNode(3)

newGraph.addEdge(1,2)
newGraph.addEdge(1,3)

console.log(newGraph)
```

## Adjacency Matrix vs. Adjacency List - Edge Checking Differences

### **Adjacency Matrix (2D Array)**

```js
if (!this.isEdge(from, to) || !this.isEdge(to, from))
```

* The matrix is a **fixed-size 2D array** where we manually set both `matrix[from][to]` and `matrix[to][from]` to `1`.
    
* Since we store edges explicitly in **both directions**, checking both is necessary to ensure **symmetry** in an undirected graph.
    
* **Example Matrix** for 3 nodes:
    
    ```plaintext
      0  1  2
    0 [0, 1, 1]
    1 [1, 0, 0]
    2 [1, 0, 0]
    ```
    
    * If `(0,1)` exists, then `(1,0)` **must** also exist.
        
    * The check ensures both positions are updated consistently.
        

---

### **Adjacency List (Object with Arrays)**

```js
if (!this.isEdge(from, to)) { 
```

* The adjacency list stores edges in a **single-directional way** (we manually push both `from → to` and `to → from`).
    
* If `from` contains `to`, then `to` will **automatically** contain `from`, because we add both at the same time.
    
* **Example List**:
    
    ```js
    {
        0: [1, 2],
        1: [0],
        2: [0]
    }
    ```
    
    * If `0 → 1` exists, then `1 → 0` is **already guaranteed**.
        
    * **So, checking just one direction is enough.**
        

## Comparison: Adjacency Matrix vs. Adjacency List

| Feature | Adjacency Matrix | Adjacency List |
| --- | --- | --- |
| Space Complexity | O(V^2) | O(V + E) |
| Checking an Edge | O(1) | O(V) |
| Adding an Edge | O(1) | O(1) |
| Removing an Edge | O(1) | O(V) |
| Best for Dense Graphs | ✅ | ❌ |
| Best for Sparse Graphs | ❌ | ✅ |

* **Adjacency Matrix** is great when the graph is **dense** (many edges) since edge lookups are O(1).
    
* **Adjacency List** is ideal for **sparse** graphs as it requires less memory (O(V + E)).
    

## Real-World Applications of Graphs

* **Social Networks** (Facebook friends, LinkedIn connections)
    
* **Google Maps** (Finding the shortest route)
    
* **Recommendation Systems** (Netflix, Amazon product suggestions)
    

## Conclusion

Both **Adjacency Matrix** and **Adjacency List** have their pros and cons. The choice depends on the use case:

* Use **Adjacency Matrix** for **dense graphs** where edge lookup speed is crucial.
    
* Use **Adjacency List** for **sparse graphs** where memory optimization matters.
    

Which approach do you prefer? Let me know in the comments! 🚀

Thank you for reading! I hope this gave you a clear understanding of adjacency matrices and lists. In the next blog, we’ll dive into graph traversal techniques like BFS and DFS.

💬 **Let’s connect!**

[**Twitter**](https://twitter.com/karthik_coder)  
[**Instagram**](https://www.instagram.com/coding_nemo)

![final](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbTd1c2I3NGFrYmxvYnB3ZGJqMnphYWM4eDhrODd2NHp0bHk0dms3MiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/lOJKLVYkNDWN8GoPoA/giphy.gif align="left")