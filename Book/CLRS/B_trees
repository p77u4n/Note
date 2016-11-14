B-trees

<h1>1. Introduction</h1>

<p>B-trees are balanced search trees designed to work well on disks or other direct-access secondary storage devices.</p>


<p>rees differ from Red-black trees in that B-tree nodes may have many children, from few to thousands.That is, the "branching
factor" of a B-tree can be quite large, although it usually depends on characteristic of the disk unit used.</p>

<p>rees are similar to red-black trees in that every n-node B-tree has height O(lg n). The exact height of a B-tree can be
considerably less than that of a red-black tree, however, because its branching factor, and hence the base of the logarithm that
expresses its height, can be much larger.</p>


<h2>2. Data structure on secondary storage</h2>

Secondary storage : The drive consist of one or more platters, which rotate at a constant speed around a common spindle . The
drive reads and writes each platter by a head at the end of an arm . When a given head is stationary, the surface that passes
underneath it is called a track. 

The mechanical motion has two components : platter rotation and arm movement. (Slow)

Often, accessing a page of information and reading it from a disk takes longer than examining all the information read. We
lookseparately at the two principal components of the running time :

* The number of disk accesses, and
* The CPU (computing) time. 

We measure the number of disk accseses in termsof the number of pages of information that need to be read from or written to the
disk.

B-tree algorithms keep only a constant number of pages into main memory at any time; thus, the size of main memory does not
limit the size of B-trees can be handled.

We model disk operations in our pseudocode as follows : 

x = a pointer to some object
DISK-READ(x)
operation that access and/or modify that attributes of x
DISK-WRITE(x)
other operations that access but do not modify attributes of x

The running time of a B-tree algorithm depends primarily on the number of DISK-READ and DISK-WRITE operations it performs, we
typically want each of these operations to read or write as much information as possible.

A large branching factor dramatically reduces both the height of the tree and the number of disk accesses required to find 
any key.

<h1>3. Definition of B-trees</h1>

To keep things simple, we assume, as we have for binary tree and red-black tree, that any "satellite information" associated 
with the key resides in the same node as the key.

A common variant on a B-tree, known as a B+-tree, stores all the satellite information in the leaves and stores only keys and
child pointers in the internal nodes, thus maximizing the branching factor of the internal nodes.

A B-tree T is a rooted tree (T.root) having the following properties : 
 1. Every node has the following attributes
  a. x.n - the number of keys that x currently stores
  b. the x.n keys themselves, stored in nondecreasing order x.k_1 <= x.k_2 <= ... <= x.k_n
  c. x.leaf, a boolean value that it is TRUE if x is leaf, and FALSE vice versa
 2. Each internal node also contains x.n + 1 pointers x.c_1, ... , x.c_{x.n + 1} to its children. Leaf nodes have no children
 3. The keys x.k_i separate the ranges of keys stored in each subtree: if v_i is anykey stored in the subtree with root x.c_i , 
 then : v_1 < x.k_1 < .... < x.k_n < v_{x.n + 1}
 4. All leaves has the same height.
 5. Minimum Degree of B-tree t determines the lower bound and upper bound on the number of keys the can contains
  * Lower Bound : t-1 keys
  * Upper Bound : 2t-1 keys
 
 The simplest B-tree occurs when t = 2. Every internal node then has either 2, 3, or 4 children, and we have a 2-3-4 tree. 
