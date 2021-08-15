
# Graph Data Science Retail Demo

**November 17, 2020**

This repo contains the data, load scripts, and queries that were demonstrated during the [Connections: Graph Data Science](https://neo4j.com/connections/graph-data-science/) presentation “What is Graph Data Science, and Neo4j’s GDS Library” presentation.

**Database setup:** To replicate the demo, create an empty Neo4j 4.0 database and install the [1.4 release](https://github.com/neo4j/graph-data-science/releases/tag/1.4.0) of the Graph Data Science library. Copy the .csv files from the /data folder into your plugins directory, and execute the queries in load_data.cypher.     
Or to install directly from this repository execute the queries in load_data_online.cypher

**Running the algorithms:** Each of the other folders (Customer Segmentation, Item Similarity, Item Centrality, and Exploratory Queries) contains the procedure calls and queries demonstrated during the presentation. 

**GraphSAGE**: I've updated this repo with a quick demonstration of how to use GraphSAGE to learn an embedding, predict embeddings for data in your graph, and use the KNN algorithm to build a similarity graph, along with a Bloom perspective.

**Bloom:** The Bloom perspective presented is available in GDS_Retail.json -- to use it, import the perspective into Bloom 1.3.0
when connected to the Retail database.

**_Note_**: You _may_ need to change the Community IDs specified in the Item Similarity, Ite, Centrality, and Exploratory Queries to match the community IDs in your database. They are assigned based on seeds pulled from your internal node IDs, so they may be different in your database.

**Logics:**
1. Native load customer graph with ['Customer','Item'] nodes
2. Mutate (add a relationship 'Similar' with score) with Jaccard similairty to the customer nodes based on the items that they are buying
3. Segment the customers with Louvian community using score as weight, this will add a coummunity id to the (customer) node
4. Check similairty using Jaccard similairty within community by writing an edge with weight 'score'
5. Project a co-purchasing graph in a specific community `(i1:Item)<--(c:Customer)-->(i2:Item)`
6. Run closeness centrailty in a the co-purchasing graph for the specific community, this will generate a centrailty score as a new property for the `item` nodes. This will generate a list of similar items that'd be brought together.
7. Run page rank algorithm in a the co-purchasing graph for the specific community, this will generate a pageRank score as a new property for the `item` nodes. This will generate a list of influencing items that'd be brought together. Example of influencing items means buying a birthday cake would influence the purchase of candles.
8. Exploration queries:
   - Items that are similar in one community but not another
   - Items that have different page ranks
   - Items that have high centrailty but low pageRank
   - items that have low centrailty but high pageRank
