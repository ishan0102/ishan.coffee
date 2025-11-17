---
title: Code Snippets
date: 2022-05-05
tags:
  - evergreen
enableToc: true
---
On this page I plan on documenting all the code snippets I use frequently. I'll use `T` for generic types, `K` for generic keys, and `V` for generic values when necessary. All of this code is written in `Java` since I think it's the most user-friendly.

## Maps

### Frequency Map
```java
public Map<T, Integer> frequencyMap(T[] nums) {
    Map<T, Integer> map = new HashMap<>();
    for (T num : nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }
    return map;
}
```

### Map Iteration
```java
public void iterateMap(HashMap<K, V> map) {
    for (Map.Entry<K, V> entry : map.entrySet()) {
        System.out.println(entry.getKey());
        System.out.println(entry.getValue());
    }
}
```

## Trees

### In-Order Traversal
```java
public void inOrder(Node root) {
    if (root == null) {
        return;
    }

    inOrder(root.left);
    System.out.println(root.val);
    inOrder(root.right);
}
```

### Pre-Order Traversal
```java
public void preOrder(Node root) {
    if (root == null) {
        return;
    }

    System.out.println(root.val);
    preOrder(root.left);
    preOrder(root.right);
}
```

### Post-Order Traversal
```java
public void postOrder(Node root) {
    if (root == null) {
        return;
    }

    postOrder(root.left);
    postOrder(root.right);
    System.out.println(root.val);
}
```

### Level-Order Traversal
```java
public void levelOrder(Node root) {
    Queue<Node> queue = new LinkedList<Node>();
    queue.add(root);

    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            Node curr = queue.poll();
            System.out.println(curr.val);
            queue.add(curr.left);
            queue.add(curr.right);
        }
    }
}
```

## Graphs

### Depth-First Search
```java
public void dfs(HashSet<Node> visited, Node root) {
    if (root == null) {
        return;
    }

    visited.add(root);
    for (Node child : root.children) {
        if (!visited.contains(child)) {
            dfs(visited, child);
        }
    }
}

```
*This can be done iteratively with a stack instead of recursively.*

### Breadth-First Search
```java
public void bfs(Node root) {
    HashSet<Node> visited = new HashSet<>();
    Queue<Node> queue = new LinkedList<Node>();
    queue.add(root);

    while (!queue.isEmpty()) {
        Node curr = queue.poll();
        for (Node child : curr.children) {
            if (!visited.contains(child)) {
                queue.add(child);
                visited.add(child);
            }
        }
    }
}
```

### Level-Order Traversal
```java
public void levelOrder(Node root) {
    HashSet<Node> visited = new HashSet<>();
    Queue<Node> queue = new LinkedList<Node>();
    queue.add(root);

    int level = 0;
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            Node curr = queue.poll();
            for (Node child : curr.children) {
                if (!visited.contains(child)) {
                    queue.add(child);
                    visited.add(child);
                }
            }
        }
        level++;
    }
}
```
*Level-order traversals differ from BFS because they keep track of what "step" of the BFS we're currently on. This can be handy for problems where we want to know the length of the shortest path, like [Word Ladder](https://leetcode.com/problems/word-ladder/) or [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/).*

## Matrices
*Note that matrices are a subset of graphs.*

### Number of Islands
```java
public void numberOfIslands(int[][] grid) {
    int numIslands = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == 1) {
                numIslands++;
                dfs(grid, i, j);
            }
        }
    }
}

public void dfs(int[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == 0) {
        return;
    }

    grid[i][j] = 0;
    dfs(grid, i + 1, j);
    dfs(grid, i - 1, j);
    dfs(grid, i, j + 1);
    dfs(grid, i, j - 1);
}
```
*This is one of my favorite paradigms since it applies to so many different problems.*


## Cool Stuff
### YouTube Video Downloader
```python
import pytube

video_url = 'https://www.youtube.com/watch?v=dQw4w9WgXcQ'
pytube.YouTube(video_url).streams.first().download()
```
