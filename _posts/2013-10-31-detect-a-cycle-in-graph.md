---
layout: post
title: Detect a cycle in a graph
date: 2013-10-31 06:51
author: farezv
comments: true
categories: [Connectivity (graph theory), programming problem, Time complexity]

---
So I've been applying for internship jobs and in order to prepare for technical interviews, I've scouted a bunch of websites (including <a href="http://glassdoor.com" target="_blank">GlassDoor</a>), read a bunch of interview tips and gathered a list of questions that I think are challenging and worth solving. I enjoy some of these problems so this will be an ongoing blog series. I'm going to use one blog post per question. So let's get started.

<h3>Given a directed and connected graph, find a cycle (or loop) in it, if one exists</h3>

So our input is a simple, directed and connected graph and our output is a boolean (true or false). For simplicity, I'll stick to finding just one cycle in the graph. The problem of finding all cycles in a graph is more complicated and likely deserves a blog post of its own. With these questions, I often find it easy to come up with a simple solution first, even if it's not the most efficient one. Once we have a solution, we can iterate on it and make it better by considering time and space efficiency.

<h3>Adjacency Matrix</h3>

There are two types of cycles within a graph. A strong cycle is one where there is a path from node A to B and from B to A whereas a weakly connected cycle would look like A->B->C->A. Finding strongly connected nodes is relatively easy because you can look for <a href="http://en.wikipedia.org/wiki/Adjacency_matrix#Examples" target="_blank">symmetry</a> in an adjacency matrix. However, weakly connected cycles are tricky and building an adjacency matrix requires O(n<sup>2</sup>) time (where n is the number of nodes). So let's try another approach.

<h3>Traversal</h3>

If we start at some arbitrary node A, and traverse through the graph, we can "flag" each node as "visited." During our traversal, if we arrive at a previously visited node, we've detected a cycle and can return true. Otherwise, we've traversed through the whole graph and can return false. This seems to have a time complexity of O(n). Time to implement it.

```java
public class Node {
	Node next = null;
	//...other fields
	boolean visited = false;

	public Node() {
		//...initialize fields if needed
	}
}

public class Graph {
	Node start = new Node();

	public Graph() {
		//...initialize fields if needed
	}

	public boolean findCycle(Graph g) {
		while(g.start.next != null) { // continue if this isn't the last node

			if(g.start.visited) {	// return if node has been visited
				return true;
			}

			g.start.visited = true; // otherwise, flag as visited
			g.start = g.start.next; // move to the next node
		}
		return false; // otherwise, return because no cycles found
	}
}
```
