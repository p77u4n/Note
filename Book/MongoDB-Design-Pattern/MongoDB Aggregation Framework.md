####MongoDB Aggregation Framework####

<blockquote>MongoDB Aggregation FrameWork help us selecting, processing and aggerating from a large number of documents for powerful adhoc query</blockquote>

<h5>Example : We`ll count the number of requests per source perday in last month</h5>

To use the aggregation framework, we need to set up a <strong>pipeline of operations</strong>.

```python
  result = db.command('aggregate', 'events', pipeline=[
    { '$match': {
        '$gte': datetime(2000,10,1),
        '$lt': datetime(2000,11,1) } } },
    { '$project': {
        'path': 1,
        'date': {
            'y': { '$year': '$time' },
            'm': { '$month': '$time' },
            'd': { '$dayOfMonth': '$time' } } } },
    { '$group': {
        '_id': {
            'p':'$path',
            'y': '$date.y',
            'm': '$date.m',
            'd': '$date.d' },
        'hits': { '$sum': 1 } } },
])
```
This command <em>aggregates</em> documents from the events collection with a <em>pipeline</em> that:

<ol>
  <li><strong>$match</strong> operation : similar to find() query. This operation selects all documents where the value of time filed represents
  a date is on or after ($gte) 2000-10-1 but before ($lt) 2000-10-11</li>
  <li><strong>$project</strong> operation : limit the data continues through the pipeline</li>
  <li><strong>$group operation</strong> : createnew computed documents. The documents take the following form : 
     <ul>
      <li>The <strong>_id</strong> = <strong>path</strong> field (from orginal doc) + <strong>date</strong> field (from
      the <strong>$project</strong>)</li>
      <li>The <strong>hits</strong> field uses <strong>$sum</strong> to increment a counter for every document in the group</li>
     </ul>
  </li>
</ol>
![Pipeline Visualization]
(http://i.imgur.com/rM9H5O2.png)
