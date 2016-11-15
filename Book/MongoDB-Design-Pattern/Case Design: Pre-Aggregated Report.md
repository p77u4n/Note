###Pre-Aggregated Reports###

<blockquote>Although getting the event and log data into MongoDB efficiently and querying these log records
is somewhat useful, <em>higher-level aggregation</em> is often much more useful in turining raw data into 
actionable information.</blockquote>

####Solution Overview####

We make the following assumptions about real-time analytics:

<ul>
    <li>You require up-to-the-minute data, or up-to-the-second if possible</li>
    <li>The queries for ranges of data (by time) must be as fast as possible</li>
</ul>

####Schema Design####

<p>Schemas for real-time analytics systems must support <em>simple and fast query 
and update</em> operations. In particular, we need to <em>avoid</em> the following 
performance killers.</p>
<dl>
    <dt><em>Individual documents growing significantly after they are created</em></dt>
    <dd>Document growth forces MongoDB to move the document on disk, slowing things down</dd>
    <dt><em>Collection scans</em></dt>
    <dd>The more documents that MongoDB hast to examine to fulfill a query, the less efficient will be.</dd>
    <dt><em>Documents with a large number (hundreds) of keys</em></dt>
    <dd>This can create wide variability in access time to particular values.</dd>
</dl>

<strong>Solution</strong>
<ul>
    <li><em><s>Intuitive Solution</s></em> : Keeping "hit counts" in individual documents with one document for every unit time.However any query would then need to visit multiple docs for all nontrivial time-rage queries, which can slow overall query performace.</li>
    <li><em>Better Solution</em> : Storing number of aggregate values in a single document, reducing the number of overall docs that the query engine must examine to return its result.</li>
</ul>

#####One document per page per day, flat document#####

<p>Consider the following example schema for a solution that <em>stores all statistics for a single day and page in a single document</em></p>

```python
    {
        _id: "20101010/site-1/apache_pb.gif",
        metadata: {
        date: ISODate("2000-10-10T00:00:00Z"),
        site: "site-1",
        page: "/apache_pb.gif" },
        daily: 5468426,
        hourly: {
            "0": 227850,
            "1": 210231,
            "23": 20457 },
        minute: {
            "0": 3612,
            "1": 3241,
            "1439": 2819 }
    }
```

######Pros######

<ul>
    <li>For every request on the website, you only need to update one document.</li>
    <li>Reports for time periods within the day, for a single page, require fetching a single document.</li>
</ul>

######Issue######
<dl>
    <dt>As you add data into the <code>hourly</code> and <code>monthly</code> fields, the document will grows.
    Althought MongoDB will pad the space allocated to documents, it must still reallocate these documents
    multiple times throughout the day, which degrades performance.</dt>
    <dd><em>Solution :</em> Pre-allocating documents with fields holding 0 value.</dd>
    <dt>MongoDB must scan sequentially to find the minute slot we are actually update, particularly 1439 slots
    must be examined before actually finding the correct one to update</dt>
    <dd><em>Solution :</em> Building hierarchy into the <code>minute</code> property
        ```python
            minute: {
                "0": {
                    "0": 3612,
                    "1": 3241,
                    ...
                    "59": 2130 },
                "1": {
                    "60": ... ,
                },
                ...
                "23": {
                    ...
                "1439": 2819 }
        ```
    </dd>
</dl>

######Separate documents by granularity level######

We still have a problem when querying data for long, multiday periods like months 
or quarters. In such case, storing daily aggregates in a higher-level document can
speed up these queries.

#######Monthly Statistic#######

```python
                {
                    _id: "201010/site-1/apache_pb.gif",
                    metadata: {
                        date: ISODate("2000-10-00T00:00:00Z"),
                        site: "site-1",
                        page: "/apache_pb.gif" },
                    daily: {
                        "1": 5445326,
                        "2": 5214121,
                        ...}
                    }
```

#######Daily Statistic#######

 ```python
            {

                _id: "20101010/site-1/apache_pb.gif",
                metadata: {
                    date: ISODate("2000-10-10T00:00:00Z"),
                    site: "site-1",
                    page: "/apache_pb.gif" },
                hourly: {
                    "0": 227850,
                    "1": 210231,
                    ...
                    "23": 20457 },
                minute: {
                    "0": {
                        "0": 3612,
                        "1": 3241,
                        ...
                        "59": 2130 },
                    "1": {
                        "0": ...,
                    },
                    ...
                    "23": {
                    "59": 2819 }
                    }
            } 
```




