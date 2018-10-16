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


// Query to match the words
MATCH (n:Word)-[r]->(n2:Word) where n.name='fawn' RETURN n2,r.weight order by r.weight desc limit 5


// neighbor nodes and total distance to each query words
match(n:Word)-[r]->(n1:Word)-[r2]->(n2:Word) 
where n1.name='pilot' AND n.name in ['captain','chair']
return n.name as source,n2.name as destination,sum(r.weight+r2.weight) as total
order by destination,total 

// Minumum Spanning  Tree
match (n:Word)-[r]->() where n.name in ['sofa','fawn']
call algo.spanningTree.minimum('Word','RELATED','weight',id(n),{write:true, writeProperty:"MINST"})
yield effectiveNodeCount
return effectiveNodeCount;


// Read MST Relationship
match path = (n:Word)-[:MINST]->() where n.name in ['sofa','fawn']
with relationships(path) as rels
unwind rels as rel
with distinct rel as rel
Return startNode(rel).name as source, 
endNode(rel).name as destination, rel.weight as cost

// delete relationship
MATCH p=()-[r:MINST]->() delete r


