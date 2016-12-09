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
        



