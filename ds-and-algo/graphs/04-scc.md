If a group of nodes in a graph G are all reachable from one another, that is, given any node v1 and v2 belonging to that group, there's always a path leading from `v1` to `v2` and a reciprocal path leading from `v2` to `v1`, we say that the nodes are *strongly connected*. The subset of G these nodes belong to is called a *Strongly Connected Component* (SCC) of G.

    [example]

SCCs can be single nodes, which can happen if a node is isolated, or if none of the nodes reachable from it can reach back, or vice versa (it cannot reach any of the vertices that can reach it).

    [example]

If we consider an SCC to be an isolated system, we can conceptually shrink that system and represent it as a single node of a larger system. Similarly shrinking all other SCCs of a directed graph turns its representation into a directed acyclic graph (dag).

    [example]

In a dag, a node that connect to others but to which nothing connects is called a source node, the equivalent SCC is a source SCC. Similarly, a node that does not connect to any other, but to which there are nodes that connect is a sink node and the equivalent SCC is a sink SCC. There can be multiple sources and sinks in a graph.

    [example]

If two SCCs become linked with edges going back and forth, that is they connect to each other, they become a single SCC. 

    [example]

In many graph problems it's useful to identify the different SCCs of the graph, that is the different clumps of nodes that can mutually reach others.

How to find the SCCs of a directed graph?
===
The next section introduces a technique to identify the SCCs in a directed graph. To understand it, however, requires to first grasp a number of properties.

- when traversing a graph G=(V,E) in DFS, regardless of the node we choose to start with, we can traverse the entire graph and discover all other nodes. The algorithm is quite simple, (1) pick any node in G.V to start DFS, then recursively process its adjacent nodes in DFS as well (discovery phase), (2) repeat until you've arrived to all reachable sinks (aka leaf nodes) then, (3) wrap up the DFS by completing the processing of ancestor nodes (backtracking phase). (4) Scan G.V for another node that has not yet been processed and if found repeat (1) else end processing (also see linearization of a dag, i.e. topological sorting algorithm).

- From the properties that allows a node in an SCC to reach all other member nodes, we can observe that if we pick a starting node for DFS at random within an SCC, we will eventually discover all other nodes within the SCC.

- unfortunately, the above properties are not sufficient to know with certainty at which exact moment we've covered an SCC during DFS, since as we traverse the graph from source to sink, the process could go through the entire graph, seamlessly moving from one SCC to the next, without ever hinting at a transition.

    [example]

- Now, consider the particular case of randomly picking a starting node on the entire graph for DFS and it turns out that that node happens to be part of a sink SCC. Since by definition a sink SCC does not link to any other SCC, the traversal phase is guaranteed to stop once every node in the sink is processed.

    [example]

- This hints at a possible strategy to discover at least the sink SCCs. That is, we just have to find a way to detect nodes that we know for sure are located in a sink SCC and explicitly pick them as starting nodes for our DFS traversal and this will ensure that the discovery stops when the sink SCC has been processed. How can we detect such "beacon" nodes?

- Our strategy to do this will make use of two other properties of DFS. So let's briefly revisit them.

- As part of our graph's DFS traversals we can mark each of its vertices with a "start" and "end" discovery timestamps that can be useful when we want to restitute some chronology or hierarchy.

    [example]

- The relationships between timestamps of vertices have some useful properties (see dag). Specifically of interest to our situation is the property that if a non-reciprocal path exists that links a node Y to another node Z in a DFS tree (i.e. as part of the DFS there's a tree-edge, forward-edge, cross-edge or a path that links Y to Z, but no back-edges), Y.end is guaranteed to be greater than Z.end. Y and Z may be selected for DFS in any order, yet this property will hold. Take the case where Y is picked first for DFS, its adjacent and descendant nodes will be processed as part of its own discovery. Z being a descendant, its processing will start after Y's, but will conclude before thus, Z.end will be timestamped before Y.end. On the other hand If Z is selected for processing before Y, its processing will also conclude before that of Y since there are no edges leading from Z to Y, and its Z.end timestamp will again be less than the eventual future Y.end timestamp. In both cases, the property holds. The relationship between "end" timestamps implies something interesting for our goal of finding sink SCCs. If you take the largest of all end timestamps in a DFS tree, let's called it A.end, it stems from the reasonning above that A cannot have a preceding node in the DFS tree, meaning that it's a source node. If a node is a source node of a DFS tree, it logically cannot be a member of a non-source SCC, since a non-source SCC has a predecessor and by extension so do all its nodes.

    [example]

- What we've just found is a way to identify a node that is located in a *source* SCC. But if you remember, what we're actually looking for it's a way to identify a node that is in a *sink* SCC, because DFS only stops for sink SCCs. So why would knowing how to find a source SCC be of any interest? Because there's another property of graph that we can use to our advantage. For now, let's just remember that we have in our arsenal a way to identify a node located in a source SCCs.

- The transpose of a directed graph is an equivalent graph where all the vertices are the same, but the edges are reversed.

    [example]

- It just so happens that the SCCs in the transposed graph are exactly the same as the original graph... with a small twist. All sinks in the original are now sources in the transpose and all sources are sinks.

    [example]

- Tying this with our ability to find a node in the original graph's source SCCs means that we also have a way to identify all its transpose graph's sink SCCs (remember, G's source are GT's sinks). And since a graph's SCCs are the same as its the transpose's, we now actully have a way of identifying G's source SCCs.


- But what about all the other non-source or non-sink SCCs. The ones in between. Well it's pretty straight forward remember that the largest end timestamp in DFS tree is always part of a source SCC. If as we're discovering our transpose's sink SCCs we gradually remove its nodes from the graph, we're also removing the largest timestamp, leaving space for the next largest timestamp, which incidentally is located in the next sink SCC. By further proceeding by elimination like this, we can discover all the SCCs in the transpose graph, thus all the SCC in our original DFS tree.

    [example]

Our recipe to discover SCCs in a DFS tree is thus:
- traverse the tree in DFS, regardless of which node you pick first. Record each node's end timestamps and add it to a maxheap.
- generate the transpose of the original graph
- traverse the transpose in DFS, starting with the node with the largest end timestamp (top of the heap). As you end each node's processing, mark it as processed. 
- As you end an SCC's processing select the next node from the top of the maxheap and if it has not been marked as processed traverse it.
