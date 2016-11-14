### The rules of index design ###

<p>MongoDB indexs are store in the data structure known as B-tree. This mean each index is store in sorted order on all the field in
the index.</p>

Because of this sorted B-tree structure, then, the following rules will lead to efficient indexes:

<ul>
  <li>Fields that will be queried by equality : should occur first in the index definition</li>
  <li>Fields that will be used to sort : should occur second in the index definition</li>
  <li>Fields that will be queried by range : should occur last in the index definition</i>
</ul>
