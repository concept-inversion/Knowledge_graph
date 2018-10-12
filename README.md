# Knowledge_graph

## Query:

// Load data from csv db.csv with a single relationship

load csv with headers from "file:///db.csv" as csvoutline
merge(word1:Word{name:csvoutline.source})
merge(word2:Word{name:csvoutline.destination})
merge (word1)-[:RELATED{weight:csvoutline.distance}]-(word2)


// Load data from csv db.csv with multiple relationship

load csv with headers from "file:///db.csv" as csvoutline
merge(word1:Word{name:csvoutline.source})
merge(word2:Word{name:csvoutline.destination})
CALL apoc.create.relationship(word1, csvoutline.relationshipType, {}, word2)
