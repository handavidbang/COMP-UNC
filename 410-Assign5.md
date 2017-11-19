COMP 410 


Basic DiGraph and Topological Sort

 
Summary

In this first part you will design and implement in Java a basic directed graph data structure. You will then compute a topological sort on that graph.

You will build a graph "ADT" class that contains the data structures for representing graph structure and content, and provides methods to efficiently build and manipulate that structure and content
The graph has directed edges.
We will hand out the Oracle about halfway through the assignment (via sakai). It is expected that you test yourself before and after the Oracle comes out!

What to Hand-in

Hand in your code as per the instructions under Assignment 5 in Sakai. You will find details telling you how and what to zip, naming, etc.


Specifications

Build your code to these specifications so that we may properly examine it:

(1) Interface DiGraph_Interface

package DiGraph_A5;
/**
 * COMP 410
 *
 * Make your class and its methods public!
 * Don't modify this file!
 * Begin by creating a class that implements this interface.
 *
*/

public interface DiGraphInterface {
  /*
    Interface: A DIGRAPH will provide this collection of operations:

    addNode
      in: unique id number of the node (0 or greater)
          string for name
            you might want to generate the unique number automatically
            but this operation allows you to specify any integer
            both id number and label must be unique
      return: boolean
                returns false if node number is not unique, or less than 0
                returns false if label is not unique (or is null)
                returns true if node is successfully added 
    addEdge
      in: unique id number for the new edge, 
          label of source node,
          label of destination node,
          weight for new edge (use 1 by default)
          label for the new edge (allow null)
      return: boolean
                returns false if edge number is not unique or less than 0
                returns false if source node is not in graph
                returns false if destination node is not in graph
                returns false is there already is an edge between these 2 nodes
                returns true if edge is successfully added 
            
    delNode
      in: string 
            label for the node to remove
      out: boolean
             return false if the node does not exist
             return true if the node is found and successfully removed
    delEdge
      in: string label for source node
          string label for destination node
      out: boolean
             return false if the edge does not exist
             return true if the edge is found and successfully removed
    numNodes
      in: nothing
      return: integer 0 or greater
                reports how many nodes are in the graph
    numEdges
      in: nothing
      return: integer 0 or greater
                reports how many edges are in the graph
    topoSort:
      in: nothing
      return: array of node labels (strings)
                if there is no topo sort (a cycle) return null for the array
                if there is a topo sort, return an array containing the node
                  labels in order
  */

  // ADT operations

  boolean addNode(long idNum, String label);
  boolean addEdge(long idNum, String sLabel, String dLabel, long weight, String eLabel);
  boolean delNode(String label);
  boolean delEdge(String sLabel, String dLabel);
  long numNodes();
  long numEdges();
  String[] topoSort();
}
(2) Class DiGraph

package DiGraph_A5;
public class DiGraph implements DiGraphInterface {

  // in here go all your data and methods for the graph
  // and the topo sort operation

  public DiGraph ( ) { // default constructor
    // explicitly include this
    // we need to have the default constructor
    // if you then write others, this one will still be there
  }
  
  // rest of your code to implement the various operations
}
(3) Class DiGraphPlayground

package DiGraph_A5;

public class DiGraphPlayground {

  public static void main (String[] args) {
  
      // thorough testing is your responsibility
      //
      // you may wish to create methods like 
      //    -- print
      //    -- sort
      //    -- random fill
      //    -- etc.
      // in order to convince yourself your code is producing
      // the correct behavior
    exTest();
    }
  
    public static void exTest(){
      DiGraph d = new DiGraph();
      d.addNode(1, "f");
      d.addNode(3, "s");
      d.addNode(7, "t");
      d.addNode(0, "fo");
      d.addNode(4, "fi");
      d.addNode(6, "si");
      d.addEdge(0, "f", "s", 0, null);
      d.addEdge(1, "f", "si", 0, null);
      d.addEdge(2, "s", "t", 0, null);
      d.addEdge(3, "fo", "fi", 0, null);
      d.addEdge(4, "fi", "si", 0, null);
      System.out.println("numEdges: "+d.numEdges());
      System.out.println("numNodes: "+d.numNodes());
      printTOPO(d.topoSort());
      
    }
    public static void printTOPO(String[] toPrint){
      System.out.print("TOPO Sort: ");
      for (String string : toPrint) {
      System.out.print(string+" ");
    }
      System.out.println();
    }

}
Class RunTests

package DiGraph_A5;
import gradingTools.comp410f17.assignment5.testcases.Assignment5Suite;

public class RunTests {
  public static void main(String[] args){ //runs Assignment 5 oracle tests
    Assignment5Suite.main(args);
  }
}
Notes on operations

addNode:
We are serial numbering the nodes so that each has a unique id. Make sure the id number passed in is 0 or greater. We will require the name/label to be unique as well (so we can hash the string and get one node back). Return true if it was added successfully; return false if it is already in the graph (name not unique, id not unique).

addEdge:
We will serial number an edge so that each has a unique integer id. Make sure the id number is 0 or greater. Since node labels are unique we will specify an edge by naming the source and destination nodes. The label for the edge is optional (meaning the user may pass the null string; use null label as default. Edge labels also do not have to be unique. Edge weight is an integer (it might be negative); use 1 as the default weight. We are only allowing one edge from any node M to node N; however since edges are directed, we might have one edge from M to N, and another edge from N to M. To further complicate things, we also may have an edge from M to M (itself). Return true if the edge is added; return false if it is not added (id num or label problems, edge already there).

delNode:
We identify the node to remove by label (since node names are unique). Find the node and remove it from the structure(s) of the graph. Removing a node will require also removing the edges into and out of that node. Return true if the node was found and successfully removed; return false if the node was not in the graph.

delEdge:
We identify the edge to remove by passing in labels for the source and destination nodes (since we are allowing only one such edge). Removing an edge will not require removing any nodes (remember that a set of nodes with no edges is a valid graph... not connected, but a graph nonetheless). Return true if the edge is found and removed; return false otherwise.

topoSort:
As we will see in class, a topological sort is a list of the nodes in a graph in a specific order. Therefore, this operations is returning an array and the elements of the array are node labels. The order will be first node in the sort in element 0, and then on up sequentially through the subscripts.

Topological sorting is only possible on graphs with no cycles, and your algorithm will detect if the graph has a cycle. In this case you return null for the array.

print: (optional)
Note that a print function is not required, but we highly recommend it! If you were to do so, we would recommend this structure, but feel free to make it look how you like. Produce a readable form of the graph structure and contents. Include the node names and numbers, the edge numbers (and labels if they are there), edge weights, and a visual indication of what the source and destination of each edge is. Make it like this:

  (0)NewYork 
    (0)--1--> Chicago
    (1)--prop,3--> Washington

  (1)Washington
    (3)--3--> NewYork
    (2)--2--> Denver

  (2)Chicago
    (4)--1--> Denver
    (5)--prop,1--> Washington

  (3)Denver
    (6)--meal,2--> Washington
Here the node number and edge number are in parentheses, the names/labels after. The arrow is from the node it is under to the node at the end of the "--->". For example, the first three lines tell us that :
  node 0 is named NewYork, 
  and edge 0 (no label, weight 1) goes from node NewYork 
                                       to node Chicago,
  and edge 1 (label "prop", weight 3) goes from node NewYork 
                                           to node Washington
Example Data

Here is an example of a graph and a topo sort for it:

  source       destination   weight   label
============================================
  Raleigh      Durham        14       .
  Durham       Hillsborough  9        .
  Chapel_hill  Graham        25       .
  Graham       Hillsborough  12       .
  Graham       Los_Angeles   3021     .
  Chapel_hill  Carrboro      1        .
  Carrboro     Cary          32       .
  Carrboro     Pittsboro     15       .
  Cary         Raleigh       3        .
  Pittsboro    Cary          19       .
  Pittsboro    Sanford       15       .
  Sanford      Los_angeles   3007     .
This format shows an edge from a node "Raleigh" to a node "Durham" with weight 14 and "." for edge label. For topo sorting we dont used the edge labels or weights (but your digraph should still allow and represent them).

A valid topological sort for this is: 
Chapel_hill, Carrboro, Graham, Pittsboro, Cary, Sanford, Raleigh, Los_angeles, Durham, Hillsborough 
To check this, we just look at each edge in the graph and see if the source node for the edge appears before the destination node in the topo sort. For example, the graph shows

  Raleigh      Durham        14       .
Is Raleigh before Durham in the sort? YES 
The graph shows
  Carrboro     Pittsboro     15       .
  Graham       Hillsborough  12       .
Is Carrboro before Pittsboro in the sort? YES 
Is Graham before Hillsborough in the sort? YES 
etc.
General Implementation Considerations

For this assignment, you may use the various data structures we have studied from the Java collections library. You may need Lists, HashMaps, Stacks, Heaps, etc. so feel free to use what Java provides. Write your own graph code (Java does not have a built in graph data structure or library implementation). Do not search up someone else's code/library.

Consider storing the vertex-names in a sorted array, or hashmap, or something similar; that way, translating from these names to the internal representation is more efficient. This hashmap would supplement the adjacency list structure that represents the basic nodes and arcs of the graph (nodes in a list, each node with a list of edges attached to it). If you need to get a single node, you can hash it in constant time rather than search a list of all nodes in linear time.

However, we do still have need to systematically access every node in (such as doing a print). So you will need some way to efficiently get all nodes one at a time in sequence. With a hashmap you can get an iterator over all the keys (node names), but the order will be arbitrary. So in designing consider if there might be reason to keep a node list of your own, where you could control the order if needed.

Efficiency: Be sure to implement your DiGraph in an efficent manner, as described in the book and in lecture. While you may have a working DiGraph and you pass the Oracle Tests, in actual grading we will look at your use of data structures and manually grade part of your assignment based on this.

To check your efficiency, try building something like a million-node graph (or half-million), and link each node to it's next neighbor (or to it's next two neighbors, etc.). You can do this is your playground class, by creating nodes in a for loop. In each loop body, create a new node, and link the previous node to the new one. If your code is inefficient (say, if you are doing an N^2 algorithm someplace in inserting nodes or edges), then you will find that small graphs build fine but your million node graph takes a looooooong time.

It would probably be a good idea to make yourself Edge and Vertex Classes to make all of the data management a bit easier to manage.
