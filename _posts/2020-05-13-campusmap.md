---
title: "UW Campus Map"
date: 2020-05-12
tags: [software development, Java, React, Spark-Java]
header:
  image: "/images/campusmap/campusMap.png"
excerpt: "Implemented a graph from scratch, utilized Dijkstra's Algorithmto find Shortest path between two locations at UW"
mathjax: "true"
---

# [Campus Map Project](https://github.com/mulepati/campusMap)

## Overview

UW campus map was made with a variety of modular components each of which were tested with a thorough test suite. These components included a generic graph class, campus map class, back end java spark server, and finally the front end components in react. Each java component includes specification documentation for modularity. The functionality of this program is rather simple, it allows the user to select two different buildings on campus or near campus. Once the two buildings are selected hitting the generate path button will find the shortest path between those two buildings and draw a line on the map. The reset button will reset all parts of the GUI to their initial state and clear all markings made on the map. The application allows all this while handling bogus inputs from the user so that there are no server errors.

### Generic Graph

**DirectedLabeledGraph** is an mutable representation of a directed graph made up of generic nodes and generic labeled edges between nodes. Generic graph can be represented as G = (V, E) where V vertices is a set of all the current vertices in the graph G, and E is a list of directed edges currently in the graph.

An edge can be in terms of (p, c, label) where p is the parent vertex pointing to c child vertex, with the edge labeled by label. While the nodes/vertices are represented as a unique value type in the graph.

An edge is invalid if either p or c is not in V or if the label for the edge is not defined, an edge will also have to be unique. So there cannot be two edges (p, c, label) and (p1, c1, label1), where p = p1, c = c1, and label = label1.
 
An example of a DirectedLabeledGraph would be G(String, String) = (V, E), where V = [A, B, C, D, E, F, G] and E = [(A, B, "Label 1"), (B, G, "Label 2"), (A, E, "Label r")]
 
The graph itself is implemented with an adjacency list, with a list of operations that can be done to it. The edge is represented by a private class that includes the start node
and the end node.
```    private HashMap<N, HashSet<Edge>> e;
```

List of Operations:

```
addNode(N s)
removeNode(N s)
addEdge(N p, N c, E label)
removeEdge(N p, N c)
removeEdge(N p, N c, E label)
listChildren(N p)
listLabels(N p, N c)
listNodes()
containsNode(N v)
containsEdge(N p, N c, E label)
containsEdge(N p, N c)
```

### Campus map

### React

