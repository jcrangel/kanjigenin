"# kanjigenin" 

# Neo4j Database creation 

First we add the kanjis from this project: https://medium.com/neo4j/learn-japanese-characters-using-neo4j-483585abc5b8

```
UNWIND ["5", "4", "3", "2", "1"] AS level
LOAD CSV FROM "https://raw.githubusercontent.com/jimmycrequer/roth-2019/master/neo4j/data/vocabulary_6501" + level + ".csv" AS row
MERGE (k:Kanji {value: row[0]})

WITH row, k, level
MERGE (l:Level {value: "N" + level})
WITH row, k, l
MERGE (k)-[:HAS_LEVEL]-(l)

WITH row, k
UNWIND split(row[1], "・") AS reading
MERGE (r:Reading {value: reading})
WITH row, k, r
MERGE (k)-[:HAS_READING]->(r)

WITH row, k
UNWIND split(row[2], "; ") AS meaning
MERGE (m:Meaning {value: meaning})
WITH row, k, m
MERGE (k)-[:HAS_MEANING]->(m)
```


Adding my version of radicals:

```
LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/jcrangel/kanjigenin/master/data/radical2kanji.csv" AS row
UNWIND split(row.kanjilist, "") AS kanji
MERGE (k:Kanji {value: kanji})

WITH row, k
MERGE (r:Radical {value: row.radical})

WITH k, r
MERGE (k)-[:HAS_RADICAL]->(r)
```

This append any new kanji and radicals and relationships. 


