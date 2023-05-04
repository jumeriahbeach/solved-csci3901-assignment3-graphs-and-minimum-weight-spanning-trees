Download Link: https://assignmentchef.com/product/solved-csci3901-assignment3-graphs-and-minimum-weight-spanning-trees
<br>
Work with graphs and minimum weight spanning trees.

Background

A common task in machine learning and big data is clustering data. Clustering data means that we have many initial data points and we want to group the data points so that all of the points in the same cluster are similar in some way and points in different clusters are deemed sufficiently different from one another. There are many ways to do clustering. We are going to approximate one of them.

We are going to think about grouping documents around common topics. Given a set of documents, we will compare each pair of documents and will create a number that reflects how similar the two documents are to one another.

The basic idea of the clustering is to start with each document as its own cluster and to repeatedly merge clusters until we reach a balance point. How we merge the clusters is the point of this assignment.

<h2>Problem</h2>

You will have a method that lets us build an <strong>undirected</strong>, <strong>weighted graph </strong>between vertices. Each vertex has a string name/identifier (like a document name). The weight on each edge is a similarity measure between the vertices. For this assignment, that measure will be a positive integer, with a lower value meaning that the two vertices are very similar.

We will want to identify sets of vertices that are all similar. We call these sets <em>clusters</em>. For each cluster, we remember the weight of the heaviest edge that was used to create this cluster from previous ones (the maximum weight starts at 1 for clusters of a single vertex).

We begin with each vertex as its own cluster. We then proceed with merges in a way similar to making a minimum weight spanning tree, using Kruskal’s algorithm<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>. Find the edge <em>e </em>with the smallest weight that connects two different clusters <em>C </em>and <em>D</em>. If there are several edges of the same low weight then take the edge with the smallest-named vertex at the end of an edge. Let the heavy edge that we’re tracking for clusters <em>C </em>and <em>D </em>be edges <em>c </em>and <em>d</em>. Here is where we differ from Kruskal a bit: if the ratio

(weight of <em>e</em>) / min(weight of <em>c</em>, weight of <em>d</em>)

is less than the given tolerance value, then merge clusters <em>C </em>and <em>D</em>. Otherwise, don’t consider edge <em>e </em>to merge clusters in the future. Stop when none of the clusters can be merged.

Figure 1: Sample graph

Your task is to create a class called VertexCluster. The class has at least 2 methods:

boolean addEdge(String vertex1, String vertex2, int weight) Record an edge going between the two vertices with the given weight. Return true if the edge was added. Return false if some error prevented you from adding the edge.

Set&lt;Set&lt;String&gt;&gt; clusterVertices (float tolerance) Return the set of clusters that you get from the current graph, using the given tolerance parameter for deciding if an edge is good enough to merge two clusters (as described earlier). Return null if some error happened while calculating the clusters.

<h2>Example</h2>

Consider the weighted graph of fig. 1, created with the following sequence of addEdge commands:

<table width="181">

 <tbody>

  <tr>

   <td width="153">addEdge( ”A” , ”B” ,</td>

   <td width="28">3);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”A” , ”H” ,</td>

   <td width="28">1);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”I” , ”H” ,</td>

   <td width="28">1);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”I” , ”B” ,</td>

   <td width="28">4);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”B” , ”C” ,</td>

   <td width="28">7);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”I” , ”C” ,</td>

   <td width="28">8);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”G” , ”H” ,</td>

   <td width="28">7);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”H” , ”J” ,</td>

   <td width="28">10);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”D” , ”I” ,</td>

   <td width="28">12);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”D” , ”C” ,</td>

   <td width="28">14);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”D” , ”E” ,</td>

   <td width="28">8);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”J” , ”F” ,</td>

   <td width="28">3);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”E” , ”F” ,</td>

   <td width="28">2);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”F” , ”G” ,</td>

   <td width="28">7);</td>

  </tr>

  <tr>

   <td width="153">addEdge( ”J” , ”D” ,</td>

   <td width="28">1);</td>

  </tr>

 </tbody>

</table>

In the algorithm, we process the edges in the following order of increasing weight and then tiebreaker. We use a maximum ratio of 3 for the calculation on whether or not to merge the components.

<table width="589">

 <tbody>

  <tr>

   <td width="212">Edge</td>

   <td width="57">Weight</td>

   <td width="204">Calculation</td>

   <td width="117">Outcome</td>

  </tr>

  <tr>

   <td width="212">(<em>A,H</em>)</td>

   <td width="57">1</td>

   <td width="204">1<em>/</em>min(1<em>,</em>1) = 1</td>

   <td width="117">Merge components</td>

  </tr>

  <tr>

   <td width="212">(<em>D,J</em>)</td>

   <td width="57">1</td>

   <td width="204">1<em>/</em>min(1<em>,</em>1) = 1</td>

   <td width="117">Merge components</td>

  </tr>

  <tr>

   <td width="212">(<em>H,I</em>)</td>

   <td width="57">1</td>

   <td width="204">1<em>/</em>min(1<em>,</em>1) = 1</td>

   <td width="117">Merge components</td>

  </tr>

  <tr>

   <td width="212">(<em>E,F</em>)</td>

   <td width="57">2</td>

   <td width="204">2<em>/</em>min(1<em>,</em>1) = 2</td>

   <td width="117">Merge components</td>

  </tr>

  <tr>

   <td width="212">(<em>A,B</em>)</td>

   <td width="57">3</td>

   <td width="204">3<em>/</em>min(1<em>,</em>1) = 3</td>

   <td width="117">Merge components</td>

  </tr>

  <tr>

   <td width="212">(<em>F,J</em>)</td>

   <td width="57">3</td>

   <td width="204">3<em>/</em>min(1<em>,</em>1) = 3</td>

   <td width="117">Merge components</td>

  </tr>

  <tr>

   <td width="212">(<em>B,I</em>)</td>

   <td width="57">4</td>

   <td width="204">Vertices in same component</td>

   <td width="117">Disregard edge</td>

  </tr>

  <tr>

   <td width="212">(<em>B,C</em>)</td>

   <td width="57">7</td>

   <td width="204">7<em>/</em>min(3<em>,</em>1) = 7</td>

   <td width="117">Don’t merge</td>

  </tr>

  <tr>

   <td width="212">(<em>F,G</em>)</td>

   <td width="57">7</td>

   <td width="204">7<em>/</em>min(3<em>,</em>1) = 7</td>

   <td width="117">Don’t merge</td>

  </tr>

  <tr>

   <td width="212">(<em>G,H</em>)</td>

   <td width="57">7</td>

   <td width="204">7<em>/</em>min(3<em>,</em>1) = 7</td>

   <td width="117">Don’t merge</td>

  </tr>

  <tr>

   <td width="212">(<em>C,I</em>)</td>

   <td width="57">8</td>

   <td width="204">8<em>/</em>min(3<em>,</em>1) = 8</td>

   <td width="117">Don’t merge</td>

  </tr>

  <tr>

   <td width="212">(<em>D,E</em>)</td>

   <td width="57">8</td>

   <td width="204">Vertices in same component</td>

   <td width="117">Disregard edge</td>

  </tr>

  <tr>

   <td width="212">(<em>H,J</em>)</td>

   <td width="57">10</td>

   <td width="204">10<em>/</em>min(3<em>,</em>3) = 3<em>.</em>3</td>

   <td width="117">Don’t merge</td>

  </tr>

  <tr>

   <td width="212">(<em>D,I</em>)</td>

   <td width="57">12</td>

   <td width="204">12<em>/</em>min(3<em>,</em>3) = 4</td>

   <td width="117">Don’t merge</td>

  </tr>

  <tr>

   <td width="212">(<em>C,D</em>)</td>

   <td width="57">14</td>

   <td width="204">14<em>/</em>min(3<em>,</em>1) = 14</td>

   <td width="117">Don’t merge</td>

  </tr>

 </tbody>

</table>

We end with the 4 components:

<ul>

 <li><em>A</em>, <em>B</em>, <em>H</em>, <em>I</em></li>

 <li><em>C</em></li>

 <li><em>D</em>, <em>E</em>, <em>F</em>, <em>J</em></li>

 <li><em>G</em></li>

</ul>

<strong>Inputs</strong>

All the inputs will be determined by the parameters used in calling your methods.

<h2>Assumptions</h2>

You may (but don’t need to) assume that

<ul>

 <li>You do not need to worry about round off error in the computations.</li>

 <li>If two vertex strings are different in any case-invariant way then they are different vertices.</li>

 <li>Not all vertices will necessarily be connected to one another in the graph.</li>

</ul>

<h2>Constraints</h2>

<ul>

 <li>You <strong>may </strong>use any data structures from the Java Collection Framework.</li>

 <li>You <strong>may not </strong>use a library package to for your graph or for the algorithm to do matchings on your graph.</li>

 <li>If in doubt for testing, I will be running your program on cs.dal.ca. Correct operation of your program shouldn’t rely on any packages that aren’t available on that system.</li>

</ul>

<h2>Notes</h2>

<ul>

 <li>You do not need to implement and use a disjoint-set data structure to store the components, though it may be a fun exercise to do so if you choose.</li>

 <li>You will need an internal class to store each vertex of the graph. That class will want to store some representation of the cluster that the vertex belongs to. One option is to use an integer for the cluster number. Start each vertex with its own unique cluster number. When clusters merge, give everyone the cluster number that is the smaller number of the clusters being combined.</li>

 <li>You will need to choose whether to store the graph as an adjacency matrix, as an adjacency list, or as an incidence matrix. Your best bet will likely be an adjacency list.</li>

 <li>You will need to decide on how to store the weights of edges.</li>

 <li>Rather than find the smallest edge between clusters, you might consider running through all edges and ignoring the edges that join vertices in the same cluster (which you can tell by them having the same cluster number in their endpoint vertices).</li>

 <li>You might consider writing your own method to print the graph. It might help simplify your debugging.</li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> <a href="https://en.wikipedia.org/wiki/Kruskal%27s_algorithm">https://en.wikipedia.org/wiki/Kruskal%27s</a> <a href="https://en.wikipedia.org/wiki/Kruskal%27s_algorithm">algorithm</a>