- a tree is a connected (all nodes are reachable), acyclic graph. 
- in a tree of K nodes there are K-1 edges.

- the starting node, regardless of which is selected, is the symbolic tree. As such it is not connected to anything. Its adjacent vertices are connected to it, and their adjacent vertices are in turn connected to them.
 
- thus each node is considered to be connected to the tree by a single edge (if it is connected to multiple other vertices, those vertices are the ones that are considered connected to it).

- when a node is processed during BFS it is considered part of the tree at the end of its processing.

- since the graph is traversed in BFS and we want to influence discovery by going through the lightest path, we make use of a priority queue (usually implemented using a heap structure). This ensures that a node is not processed at a time when the edge it uses to connect to the tree is not the lightest. 

    Example: 
    - imagine a starting vertex C (the root of the tree, connected to itself with an edge of weight 0)
    - 5 other nodes are connected to C as such:
        node    :   weight
        A       :   6
        B       :   1
        D       :   2
        E       :   5
        F       :   3

    - if we were to process these nodes based on the normal BFS flow A would be connected to C by an edge weighing 6. But consider that the edge that connects A to D weighs 4, if there was a way to make A part of the BFS discovery of D, we'd ensure that at the time of its discovery it's associated with the lightest edge that can connect it to the tree. The way to do this is to process nodes, by priority order of edge weight.

    - by using a priority queue we grow the tree edge by edge, node by node, from the starting node we picked, but we do so while ensuring that the path chosen always has the lightest edge.

    - No cycle is possible since a node is never processed twice. As the tree is growing outwardly, if a node is chosen for processing (i.e. for inclusion in the tree) it means that among all the edges currently discovered, the node's lightest edge should be selected (since it connects it to the growing side of the tree). Once thus connected to the tree, the node becomes part of the tree. Any other edge that might henceforth connect to it and that does not create a cycle, is actually the edge connecting another outer node to the tree.

