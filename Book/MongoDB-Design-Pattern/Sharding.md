###Sharding (Horizontal Scaling)###

<blockquote>Sharding is a method for distributing data across multiple machines. MongoDB uses sharding to support deployments with
very large data sets and high throughput operations.
There are two methods for addressing system growth : vertical scaling (capicity of single server) and horizontal scaling (dividing resource). MongoDB supports <em>horizontal scaling</em> through sharding.
</blockquote>

####Shareded Cluster####

A MongoDB shared cluster consists of the following components :

<ul>
    <li><strong>shard</strong>: each shard contains a subset of the shared data. Each shard can be deployed as a replica set</li>
    <li><strong>mongos</strong>: the <strong>mongos</strong> acts as a query router,providing an interface between client applications and the shared cluster.</li>
    <li><strong>config servers</strong>: Config servers store metadata and configuration settings for the cluster.</li>
</ul>

![The interaction of components within a sharded cluster]
(https://docs.mongodb.com/manual/_images/sharded-cluster-production-architecture.png)

####Shard Keys####

<p>To distribute the documents in a collection. MongoDB partitions the collection using <strong>shard key</strong>.
The shard key consis of an immutable fields that exist in every document in the target collection. </p>

<p><em>The choice of shard key affects the performance, efficiency, and scalability of a sharded cluster. A cluster with
the best possible hardware and infrastructure can be bottlenecked by the choice shard key</em></p>

Ideally, your shard key should have two characteristic : 

<ol>
    <li>Insertions are <em>balanced</em> between shards</li>
    <li>Most queries can be <em>routed (routable)</em> to a subset of the shards to be satisfied</li>
</ol>
####Chunks####

<p> MongoDB partitiosn sharded data into <strong>chunks</strong>. Each chunk has an inclusive lower and exclusive upper
range based on the <em>shard key</em></p>

####Shared and Non-Sharded Collections####

<p>A database can have a mixture of shared and unshared collections.</p> 
<ul>
    <li>Shared collections are partitioned and distributed across the shareds in the cluster.</li>
    <li>Unshared collections are stored on a <em>primary shard</em>. Each database has its own primary shard.</li>
</ul>

![Shared and Non-Sharded Collections]
(https://docs.mongodb.com/manual/_images/sharded-cluster-primary-shard.png)

####Connecting to a Shared Cluster####

<p>You must connect to a <strong>mongos</strong> router to interact with any collection in the shared cluster. This includes
sharded and unsharded collections.</p>
<p>You can connect to a <strong>mongos</strong> the same way you connect to a mongod, such as using the 
<a href="https://docs.mongodb.com/manual/reference/program/mongo/#bin.mongo">mongo shell</a> or a 
<a href="https://docs.mongodb.com/ecosystem/drivers?jump=docs">MongoDB driver</a></p>

####Sharding Strategy####

<blockquote>MongoDB supports two sharding strategies for distributing data across shared cluster.</blockquote>

#####Hashed Sharding#####
<p>Hashed Sharding involves computing a hash of the shard key field's value. Each chunk is then assigned a <em>range</em>
based on the hashed shard key values</p>
<blockquote background-color="green">MongoDB automatically computes the hashes when resolving queries using hashed indexes.
Application do not need to compute hashes</blockquote>
![Hashed Sharding Diagram]
(https://docs.mongodb.com/manual/_images/sharding-hash-based.png)
<ul>
    <li>While a range of shard keys may be "close", their hashed values are unlikely to be on the same chunk.</li>
    <li>Data distribution based on hashed values facilitates more even data distribution.</li>
    <li>Hashed distribution meas that ranges-based quries on the shard key are less likely to target a single shard, resulting in more cluster wide <a href=https://docs.mongodb.com/manual/core/sharded-cluster-query-router/#sharding-mongos-broadcast>broadcast operation</a>.</li>
</ul>
<strong>Cons</strong>
<ul>
    <li>The shard key, and the index on the key, will consume additional space in the database.</li>
    <li>Queries must run in parallel all shards,which may lead to degraded performance (broadcast operation)</li>
</ul>
#####Ranged Sharding#####
<p>Ranged sharding involves dividing data into ranges based on the shard key values. Each chunk is then assigned
a range based on the shard key values.</p>
![]
(https://docs.mongodb.com/manual/_images/sharding-range-based.png)

<ul>
    <li>A range of shard keys whose values are "close" are more likely to reside on the same chunk. <strong>this allow for target operations</strong> (as a mongos can route the query to only the shards that contain the required data).</li>
    <li>The efficient of ranged sharding depends on the shard key chosen. (Poorly considered shard keys can result in uneven distribution of data. See <a href="https://docs.mongodb.com/manual/core/ranged-sharding/#sharding-ranged-shard-key">shard key selection</a> for ranged sharding) </li>
</ul>

#####Tag Aware Sharding#####
<p>You can tag specific ranges of the shard key and associate those tags with a shard or subset of shards. MongoDB routes reads and writes that fall into
a targed rage only to those shards assigned that tag</p>.

<ul>
    <li>Each tag has a rage that consists of an (pair) [inclusive lower bound, exclusive upper bound].</li>
    <li>Administrators can assign one or more tags to each shard in the shared cluster.</li>
</ul>

A tag range must use fields from the shard key, respecting the order of the fields for compound shard keys.
See <a href="https://docs.mongodb.com/manual/core/tag-aware-sharding/#tag-aware-sharding-shard-key">shard keys
in tag aware sharding</a>


<strong><u>References</u></strong>
<ol>
    <li><a href="https://docs.mongodb.com/manual/sharding/">mongdoDB Documentation about Sharding</a></li>
    <li>MongoDB Applied Design Pattern, Chapter 4, p37-63</li>
</ol>
