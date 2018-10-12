# Knowledge_graph

## Query:

// Load data from csv db.csv

load csv with headers from "file:///db.csv" as csvoutline
merge(word1:Word{name:csvoutline.source})
merge(word2:Word{name:csvoutline.destination})
merge (word1)-[:RELATED{weight:csvoutline.distance}]-(word2)
