# Knowledge_graph

## Database version:
Neo4j 3.4.5

## Query:

// Load data from csv db.csv with a single relationship

load csv with headers from "file:///db.csv" as csvoutline
merge(word1:Word{name:csvoutline.source})
merge(word2:Word{name:csvoutline.destination})
merge (word1)-[:RELATED{weight:csvoutline.distance}]-(word2)


// load data with multople relationship and weight

load csv with headers from "file:///db.csv" as csvoutline 
merge(word1:Word{name:csvoutline.source}) 
merge(word2:Word{name:csvoutline.destination})
with word1, word2, csvoutline
CALL apoc.create.relationship(word1, csvoutline.relationshipType,{weight:csvoutline.distance}, word2) yield rel
Return count(word1)
