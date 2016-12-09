###Chapter 1 : To Embed or Reference###

1. <strong>MongoDB unlike relational database</strong> : stores its data in structured <em>documents</em> rather than the 
 fixed <em>tables</em>.

2. <strong>Relational Data Modelling</strong>
 * Data modeling : as series of <em>tables</em>, consisting of <em>rows</em> and <em>columns</em>
 * Ways of putting data : <em>Normal Forms</em>
   * First normal form (1NF) : each row-column intersection ("cell") containing exactly one value.
     * Cons : Introduce <em>redundancy</em> -> <em>inconsistency</em>
    * Third normal form 
 * Reducing the Redundancy in 1NF : <em>Normalize</em>
   * Pros : Allows update table without redundancy
    * Cons : To get data from normalization tables we need JOINT operation -> <em>random seek</em> (__enemy to performance of spiral disk__)
 * Denormalizing : Reduce number of JOINTs operation -> can increase the complexity of application.

3. <strong>MongoDB : Schemaless Data Modelling</strong>
 * Data modelling : Data is stored in <em>document</em> - allow you to store an <em>arrays</em> of value if you desire.
 * **No JOINT operation available** : Joint operations is less expensive than implementing multiple rounds trip on different
 document, so if you usually query more than two type of informations, it is better to joint them in only one document, rather
 than separate them into two different documents.
 * Embedding :
    * Embedding for _Locality_ : MongoDB stores your documents contiguously on disk , putting your data in a document means
    that you are never more one seek away from everything you need
     * Embedding for _Atomicity and Isolation_ : (all transaction is entirely succeeds or fails, never having "partial succeeds",
      in MongoDB if you "normalize" documents, that means you also detriment Atomicity and Isolation of you datab
 * Referencing (Nomalizing in Schemaless Database): In some situations, a more  nomarlized models works better
    * Referencing for _Flexibility_ : On reason you might consider normalizing your datas into multiple collections, is the increased flexibility it gives you in performing queries. 
      * If the query patterns is well known and datas tends to be access in only one way -> Embedding is good one.
      * If the query patterns is strange or diverse, a more "normalized" approach might be better. 
     * Referencing for Potetially High-Arity Relationships : The Situations in which you have one-to-many relationships with 
     unpredictable high-arity.
       * Problem with Ebedding in this situation : 
          * The large document is, the more RAM it uses. -> RAM page fault problem -> Random Disk IO
          * Growing documents must eventually be copied to larger spaces.
          * MongoDB documents have a hard size limit of 16mb.
      * Referencing for Many-to-Many Relationship : For instance, suppose we have an commerce system storing products and 
      categories 
        
####Reference####
[1] : <https://docs.mongodb.com/v3.0/data-modeling/>
    
  * Data Model Patterns 
       * <a href="https://docs.mongodb.com/v3.0/applications/data-models-relationships/">Models Relationships between Documents </a>
       * <a href="https://docs.mongodb.com/v3.0/applications/data-models-tree-structures/"> Models Tree Structures</a>
       * <a href="https://docs.mongodb.com/v3.0/applications/data-models-applications/">Models Specific Application Contexts </a>
  * Data Model Reference 
       * <a href="https://docs.mongodb.com/v3.0/reference/database-references/"> Database References</a> 
          * <a href="https://docs.mongodb.com/v3.0/reference/database-references/#manual-references">Manual References</a> 
          * <a href="https://docs.mongodb.com/v3.0/reference/database-references/#dbrefs">DBRefs</a>
       * <a href="https://docs.mongodb.com/v3.0/reference/gridfs/">Files References - GridFS</a>
  * Data Modeling Concept
       * <a href="https://docs.mongodb.com/v3.0/core/data-model-operations/">Operation Factor and Data Model</a> : These factors are operational or address requirements that arise outside of the application but impact the performance of MongoDB based applications. When developing a data model, analyze all of your application’s read operations and write operations in conjunction with the following considerations.
           * <a href="https://docs.mongodb.com/v3.0/core/data-model-operations/#document-growth">Document Growth</a> : if your   applications require updates that will frequently cause document growth to exceeds the current power of 2 allocation, you may want to refactor your data model to use references between data in distinct documents rather than a denormalized data model. You may also use a <a href="https://docs.mongodb.com/ecosystem/use-cases/pre-aggregated-reports-mmapv1/">pre-allocation strategy</a> to explicitly avoid document growth.
           *<a href="https://docs.mongodb.com/v3.0/core/data-model-operations/#atomicity">Atomicity</a> : In MongoDB, operations are atomic at the document level. No __single write__ operation can change more than one document. Operations that modify more than a single document in a collection still operate on one document at a time. [1] Ensure that your application stores all fields with atomic dependency requirements in the same document. If the application can tolerate non-atomic updates for two pieces of data, you can store these data in separate documents. <a href="https://docs.mongodb.com/v3.0/tutorial/model-data-for-atomic-operations/#data-modeling-atomic-operation">Example</a> 
           *<a href="https://docs.mongodb.com/v3.0/core/data-model-operations/#sharding">Sharding</a> : See <a href="https://docs.mongodb.com/v3.0/core/sharding-introduction/">Sharding Introduction</a> and <a href="https://docs.mongodb.com/v3.0/core/sharding-shard-key/">Shard Keys</a> for more information.
           *<a href="https://docs.mongodb.com/v3.0/core/data-model-operations/#indexes">Indexing</a>Use indexes to improve performance for common queries. Build indexes on fields that appear often in queries and for all operations that return sorted results. It is requirement for understanding the behaviors of indexing in this above link.
              
