###Introduction###

####MongoDB is a <em>document-oriented</em> database####

<ul>
    <li>Not a relational one -> make scaling out easier.</li>
    <li>Replaces the concept of a "row" with a more flexible model, the "document".</li>
    <li>Allowing ebedded documents and arrays -> makes it possible to represent complex hierarchical relationships with a single record -> This fits naturally into the way developers in modern oo languages think about their data.</li>
    <li>No predefined schemas -> adding or removing fields as needed becomes easier.</li>
</ul>

####Easy Scaling####

<ul>
    <li>Dataset sizes for applications are growing at an incredible pace -> even small-scale app</li>
    <li>How should they scale their database ?
        <ul>
            <li>Scaling in : getting a bigger machine - least resistance, but expensive and constraint by physical limit.</li>
            <li>Scaling out : partitioning data across more machines.</li>
        </ul>    
    </li>
    <li>MongoDB was designed to scale out : automatically takes care of <strong>balancing data</strong> and <strong>load across cluster</strong>, <strong>redistributing</strong> documents automatically and <strong>routing</strong> user request to the correct machine
    </li>
</ul>

####Schema Analogy####

<ul>
    <li>A <em>Document</em> is basic unit of data for MongoDB ~ a row in a relational database</li>
    <li>A <em>Collection</em> ~ a Table with a dynamic schema.</li>
    <li>A <em>Special Key</em>, <code>_id</code>, that is unique within a collection.</li>
</ul>
