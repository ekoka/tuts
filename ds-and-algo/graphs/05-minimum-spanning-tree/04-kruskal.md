Kruskal's algorithm proceeds by collecting all edges from a graph from lightest to heaviest, as long as the latest edge collected does not create a cycle with the edges collected so far.

- since the edges are selected from light to heavy, we're guaranteed that any edge that creates a cycle (and is thus excluded from the selection) is *at least* as heavy as the sibling edges in the cycle that preceded (and superceded) it in the selection.

- the approach of the algorithm:
1. first, place each node in the graph in a separate set. 
2. as each edge {A, B} (the edge connecting node A to node B) is collected in increasing weight order, locate the sets from which node A and node B are members, let's call these sets S1 and S2.  
3. a) if S1 and  S2 are different sets, it means that so far no path exists that connects them. This edge will be that link. Record the edge and merge the two sets. After the merging, all members of S1 and S2 should belong to the same set and only that set. How this is done is an implementation detail (see discussion on set strategies). The next time any of the nodes belonging to the previous two sets is queried for its parent set, it will return this new larger set. As edges are thus collected their nodes are gradually aggregated into the same set. Within that set all nodes is connected to any other node by the path formed by the selected edges.
    [illustration]

Any future edge, that connects any two members that already belong to the same set, is considered redundant and would be discarded by step 3b. If there are more edges to process, go back to step 2, else move on to step 4.
3. b) else if it turns out that S1 and S2 are, in fact, one and the same, it means that the edges previously collected already formed a path that can connect A and B (see step 3a). If this new edge was added it would create an additional path that connecting A and B and would therefore result in a cycle. Ignore this edge and move on to the next (that is, back to step 2).

4. return all edges collected in steps 2 through 3.
    
    def kruskal(nodes, edges):
        # the return value is a minimum spanning tree
        mst = []

        # put each node in a set of its own
        sets = [set([n]) for n in nodes]

        # edges are sorted from lighter to heavier 
        edges = sort_edges(edges)

        for (n1, n2) in edges:
            # find nodes in sets 
            s1 = findset(n1, sets) 
            s2 = findset(n2, sets) 

            # if both nodes are already in the same set
            # skip the edge, otherwise it'll add a cycle
            if s1 is s2:
                continue
            # merge the two sets, that is empty one
            # and add its element to the other
            s1.difference_update(s2)
            s2.clear()
            mst.append((n1,n2))

Set strategy
===

union by rank
---
- Each node has a single parent.

- Once a root node is merged to another root node as a child, its rank stops increasing (any other node that would normally be merged to it in the future will be merged to its parent instead). It thus follows that only root nodes' ranks increase.

* property 1: For any x, x.rank < x.parent.rank


A root node with rank k is created by two nodes with rank k-1

    root.rank = child_node.rank + 1

    rank:   =>  sum of nodes
    -------------------------
    n = 0   =>  sum(nodes)==1
    n = 1   =>  sum(nodes)>=2 
    n = 2   =>  sum(nodes)>=2 * 2
    n = 3   =>  sum(nodes)>=2 * 2 * 2
    ...
    n = k   =>  sum(nodes)>=2^k


property 2: Any root node of rank k has at least 2 nodes in its tree.

- since a node's parent's rank is node.rank + 1 and a node has a single parent (which in turn has a single ancestor), it follows that each node's ancestor's rank is unique in the lineage.

- from the above it stems that two nodes of the same rank cannot have a common descendant.

property 3 if there are n nodes overall and since the resulting root node has a ranking of at most 2^k, then there can be at most n/2^k nodes of ranking k.

    nodes   =>  ranking =>  nodes
    20      =>  1       =>  floor(20/2) = 10
    20      =>  2       =>  floor(20/4) = 5 
    20      =>  3       =>  floor(20/8) = 2 
    20      =>  4       =>  floor(20/16) = 1

- the above implies that the maximum rank is Log N, thus all trees have height <= Log N, which is the upper bound on the running time to find a node.
