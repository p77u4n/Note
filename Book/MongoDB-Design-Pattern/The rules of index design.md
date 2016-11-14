### The rules of index design ###

<p>MongoDB indexs are store in the data structure known as B-tree. This mean each index is store in sorted order on all the field in
the index.</p>

Because of this sorted B-tree structure, then, the following rules will lead to efficient indexes:

<ul>
  <li>Fields that will be queried by equality : should occur first in the index definition</li>
  <li>Fields that will be used to sort : should occur second in the index definition</li>
  <li>Fields that will be queried by range : should occur last in the index definition</li>
</ul>

<p>This leads to some unfornately circumstances where our index cannot be used optimally </p>

<ul>
  <li> Range Query on two or more properties, the can not be used effectively in the index</li>
  <li> Range Query combined with Sort on a different property, less effcient than Range Query and
  Sort on the same property set.
</ul>

<em>Hint() method</em> : If you discover that the MongoDB query optimizer is making a bad choice of index (perhaps choosing to reduce the number of entries scanned at the expense of doing a large in-memory sort, for instance), you can also use the hint() method to tell it which index to use.
