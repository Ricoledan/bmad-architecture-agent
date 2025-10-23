# Graph Patterns and Algorithms

## Overview

Graph patterns define best practices for modeling data and relationships in graph databases. Graph algorithms enable analysis, discovery, and computation over connected data. This guide covers modeling patterns, traversal strategies, and common graph algorithms used in real-world applications.

---

## Graph Data Modeling Patterns

### 1. Entity-Relationship Pattern

**Use Case**: Basic graph modeling
**Pattern**: Entities as nodes, relationships as edges

```cypher
// Nodes
(:Person {name: "Alice", age: 30})
(:Organization {name: "TechCorp"})

// Relationships
(:Person)-[:WORKS_FOR {since: 2020}]->(:Organization)
```

**Best Practices**:
- Use meaningful node labels (Person, Product, Order)
- Store attributes as properties
- Use descriptive relationship types
- Keep property names consistent

---

### 2. Hyperedge Pattern

**Use Case**: N-ary relationships (more than 2 entities)
**Problem**: Edge can only connect 2 nodes
**Solution**: Create intermediate node

**Without Pattern** (not possible in property graphs):
```
Alice -[MEETING]-> Bob
Alice -[MEETING]-> Charlie
Alice -[MEETING]-> David
```

**With Pattern**:
```cypher
(:Person {name: "Alice"})-[:ATTENDED]->(:Meeting {date: "2025-01-15"})<-[:ATTENDED]-(:Person {name: "Bob"})
(:Meeting {date: "2025-01-15"})<-[:ATTENDED]-(:Person {name: "Charlie"})
```

**Use Cases**:
- Meeting participants
- Recipe ingredients (recipe, ingredient, quantity)
- Shipping (order, product, quantity, warehouse)

---

### 3. Reification Pattern

**Use Case**: Add properties to relationships
**Problem**: Need to track metadata about edges

```cypher
// Basic relationship
(:Person)-[:KNOWS]->(:Person)

// Reified with metadata
(:Person)-[:KNOWS {since: 2020, strength: 0.8, source: "Facebook"}]->(:Person)
```

**When to Use**:
- Temporal relationships (when did it start/end?)
- Weighted relationships (strength, confidence)
- Provenance (source, timestamp)
- Qualified relationships (role, context)

---

### 4. Time-Based Versioning Pattern

**Use Case**: Track changes over time
**Pattern**: Create version nodes or temporal properties

**Approach 1: Snapshot Nodes**
```cypher
(:Person {name: "Alice"})-[:VERSION {from: "2020-01-01"}]->
  (:PersonSnapshot {age: 25, title: "Engineer"})

(:Person {name: "Alice"})-[:VERSION {from: "2023-01-01"}]->
  (:PersonSnapshot {age: 28, title: "Senior Engineer"})
```

**Approach 2: Temporal Edges**
```cypher
(:Person {name: "Alice"})-[:WORKS_FOR {
  from: "2020-01-01",
  to: "2023-01-01"
}]->(:Company {name: "OldCorp"})

(:Person {name: "Alice"})-[:WORKS_FOR {
  from: "2023-01-01",
  to: null
}]->(:Company {name: "NewCorp"})
```

**Query Current State**:
```cypher
MATCH (p:Person)-[r:WORKS_FOR]->(c:Company)
WHERE r.to IS NULL OR r.to > date()
RETURN p, c
```

---

### 5. Linked List Pattern

**Use Case**: Ordered sequences
**Pattern**: NEXT relationships

```cypher
(:Step {name: "Step 1", order: 1})-[:NEXT]->
  (:Step {name: "Step 2", order: 2})-[:NEXT]->
  (:Step {name: "Step 3", order: 3})
```

**Use Cases**:
- Workflow steps
- Process sequences
- Event timelines
- Playlists

**Query**:
```cypher
// Find all steps in order
MATCH path = (first:Step)-[:NEXT*]->(last:Step)
WHERE NOT (:Step)-[:NEXT]->(first)
RETURN [node IN nodes(path) | node.name] AS sequence
```

---

### 6. Tree/Hierarchy Pattern

**Use Case**: Parent-child hierarchies
**Pattern**: PARENT_OF or CHILD_OF relationships

```cypher
(:Category {name: "Electronics"})<-[:PARENT_OF]-
  (:Category {name: "Computers"})<-[:PARENT_OF]-
  (:Category {name: "Laptops"})
```

**Use Cases**:
- Organization charts
- Category taxonomies
- File systems
- Bill of materials

**Query - Find all ancestors**:
```cypher
MATCH (child:Category {name: "Laptops"})-[:PARENT_OF*]->(ancestor)
RETURN ancestor.name
```

**Query - Find all descendants**:
```cypher
MATCH (parent:Category {name: "Electronics"})<-[:PARENT_OF*]-(descendant)
RETURN descendant.name
```

---

### 7. Graph of Graphs Pattern

**Use Case**: Meta-graphs, nested structures
**Pattern**: Nodes that represent subgraphs

```cypher
(:Project {name: "ProjectA"})-[:CONTAINS]->(:Team {name: "BackendTeam"})
(:Team {name: "BackendTeam"})-[:HAS_MEMBER]->(:Person {name: "Alice"})
(:Team {name: "BackendTeam"})-[:HAS_MEMBER]->(:Person {name: "Bob"})
```

**Use Cases**:
- Projects containing teams
- Networks of networks
- Modules and components
- Aggregated views

---

### 8. Supernode Anti-Pattern (and Solutions)

**Problem**: Node with millions of edges (hub node)
**Examples**:
- Popular user in social network
- Common product category
- Frequently used tag

**Why It's Bad**:
- Slow traversals
- Memory issues
- Write contention
- Poor cache performance

**Solution 1: Bucketing**
```cypher
// Instead of:
(:PopularUser)-[:FOLLOWS]->(:User1)
(:PopularUser)-[:FOLLOWS]->(:User2)
// ... millions more

// Use buckets:
(:PopularUser)-[:HAS_FOLLOWERS]->(:FollowerBucket {id: 1})
(:FollowerBucket {id: 1})-[:CONTAINS]->(:User1)
(:FollowerBucket {id: 1})-[:CONTAINS]->(:User2)
```

**Solution 2: Fanout on Write**
```cypher
// Instead of single popular node, duplicate data
(:User {name: "Alice"})-[:INTERESTED_IN]->(:Topic {name: "GraphDB"})
// Don't traverse from Topic to all interested users
// Denormalize: store user interests directly
```

**Solution 3: Sampling**
```cypher
// Only keep top N relationships
MATCH (popular:User)-[r:FOLLOWS]->(follower)
WITH popular, r, follower
ORDER BY r.timestamp DESC
LIMIT 1000
RETURN popular, follower
```

---

### 9. Qualified Relationships Pattern

**Use Case**: Relationships with context/role
**Pattern**: Add role or context properties

```cypher
(:Person {name: "Alice"})-[:INVOLVED_IN {
  role: "Director",
  contribution: "Led development"
}]->(:Project {name: "ProjectX"})

(:Person {name: "Bob"})-[:INVOLVED_IN {
  role: "Developer",
  contribution: "Backend implementation"
}]->(:Project {name: "ProjectX"})
```

**Use Cases**:
- Movie credits (actor, role in film)
- Project roles
- Access permissions with context

---

### 10. Materialized Path Pattern

**Use Case**: Fast ancestor/descendant queries
**Pattern**: Store full path as property

```cypher
(:Category {
  name: "Laptops",
  path: "/Electronics/Computers/Laptops"
})
```

**Query - All descendants**:
```cypher
MATCH (c:Category)
WHERE c.path STARTS WITH "/Electronics/Computers"
RETURN c.name
```

**Trade-offs**:
- Faster reads
- Slower writes (need to update paths)
- More storage
- Good for read-heavy, stable hierarchies

---

## Graph Traversal Patterns

### 1. Breadth-First Search (BFS)

**Use Case**: Find shortest path, closest neighbors
**Pattern**: Explore all neighbors before going deeper

```cypher
// Find shortest path
MATCH path = shortestPath((start:Person {name: "Alice"})-[:KNOWS*]-(end:Person {name: "David"}))
RETURN path, length(path) AS hops
```

**Applications**:
- Social network degrees of separation
- Network routing
- Dependency resolution

---

### 2. Depth-First Search (DFS)

**Use Case**: Explore branches fully
**Pattern**: Follow one path to the end before backtracking

```cypher
// Find all paths (depth-first exploration)
MATCH path = (start:Person {name: "Alice"})-[:KNOWS*..5]-(end:Person {name: "David"})
RETURN path
```

**Applications**:
- Detecting cycles
- Topological sorting
- Maze solving

---

### 3. Variable-Length Paths

**Use Case**: Find connections within N hops
**Pattern**: Use `*` with min/max bounds

```cypher
// Friends within 3 degrees
MATCH (alice:Person {name: "Alice"})-[:KNOWS*1..3]-(friend)
RETURN DISTINCT friend.name

// Exactly 2 hops away
MATCH (alice:Person {name: "Alice"})-[:KNOWS*2]-(friend)
RETURN friend.name
```

---

### 4. All Shortest Paths

**Use Case**: Find all shortest paths between nodes
**Pattern**: allShortestPaths function

```cypher
MATCH paths = allShortestPaths(
  (start:Person {name: "Alice"})-[:KNOWS*]-(end:Person {name: "David"})
)
RETURN paths
```

**Use Cases**:
- Network redundancy
- Alternative routes
- Resilience analysis

---

### 5. Path Filtering

**Use Case**: Find paths matching criteria
**Pattern**: Filter on relationships or properties

```cypher
// Find trust chain (all connections have trust > 0.5)
MATCH path = (alice:Person {name: "Alice"})-[r:KNOWS*]-(bob:Person {name: "Bob"})
WHERE ALL(rel IN relationships(path) WHERE rel.trust > 0.5)
RETURN path
```

**Applications**:
- Weighted paths
- Conditional traversals
- Quality filters

---

### 6. Conditional Traversal

**Use Case**: Follow different edge types based on conditions
**Pattern**: Use relationship type alternatives

```cypher
// Follow KNOWS or WORKS_WITH relationships
MATCH (alice:Person {name: "Alice"})-[:KNOWS|WORKS_WITH*1..3]-(connected)
RETURN connected.name
```

---

## Graph Algorithms

### Centrality Algorithms

#### 1. Degree Centrality

**Measures**: Number of direct connections
**Use Case**: Identify influential nodes

```cypher
// Count connections
MATCH (n:Person)-[r:KNOWS]-()
RETURN n.name, COUNT(r) AS degree
ORDER BY degree DESC
LIMIT 10
```

**Applications**:
- Social influence
- Network hubs
- Popular items

#### 2. Betweenness Centrality

**Measures**: Number of shortest paths passing through node
**Use Case**: Identify bridges/bottlenecks

**Interpretation**:
- High betweenness = critical connector
- Removal would disconnect graph
- Broker position

**Applications**:
- Bridge personalities in social networks
- Critical infrastructure nodes
- Information flow bottlenecks

#### 3. Closeness Centrality

**Measures**: Average distance to all other nodes
**Use Case**: Find nodes that can reach others quickly

**Formula**: 1 / (average shortest path length)

**Applications**:
- Optimal facility location
- Fast information spread
- Central coordination points

#### 4. PageRank

**Measures**: Importance based on incoming links
**Use Case**: Rank nodes by authority

**Formula**: Iterative calculation considering link structure

```cypher
// Simplified PageRank concept
MATCH (n:Page)<-[r:LINKS_TO]-(m:Page)
WITH n, COUNT(r) AS inlinks,
     SUM(1.0 / SIZE((m)-[:LINKS_TO]->())) AS score
RETURN n.url, score
ORDER BY score DESC
```

**Applications**:
- Search engine ranking (Google)
- Academic paper citation importance
- Social influence measurement
- Recommendation systems

---

### Community Detection

#### 1. Label Propagation

**Concept**: Nodes adopt label of majority neighbors
**Use Case**: Find clusters/communities

**Process**:
1. Assign unique label to each node
2. Each node adopts most frequent neighbor label
3. Repeat until convergence

**Applications**:
- Social communities
- Customer segmentation
- Protein interaction groups

#### 2. Louvain Modularity

**Concept**: Optimize modularity score for communities
**Use Case**: Hierarchical community detection

**Advantages**:
- Fast on large graphs
- Hierarchical structure
- High-quality communities

**Applications**:
- Social network analysis
- Biological networks
- Market segmentation

#### 3. Connected Components

**Concept**: Find disconnected subgraphs
**Use Case**: Identify isolated clusters

```cypher
// Find connected components
MATCH (n:Person)
OPTIONAL MATCH (n)-[r:KNOWS*]-(connected)
WITH n, COLLECT(DISTINCT connected) AS component
RETURN component, SIZE(component) AS componentSize
ORDER BY componentSize DESC
```

**Applications**:
- Network segmentation
- Isolated groups
- Data quality (orphaned records)

---

### Path Finding Algorithms

#### 1. Shortest Path (Dijkstra)

**Use Case**: Find minimal cost path
**Pattern**: Weighted shortest path

```cypher
// Shortest path with distance weights
MATCH (start:City {name: "New York"}), (end:City {name: "Los Angeles"})
CALL apoc.algo.dijkstra(start, end, 'CONNECTS_TO', 'distance')
YIELD path, weight
RETURN path, weight
```

**Applications**:
- GPS navigation
- Network routing
- Supply chain optimization

#### 2. A* (A-Star)

**Use Case**: Shortest path with heuristic
**Advantage**: Faster than Dijkstra with good heuristic

**Heuristic**: Estimate remaining distance (e.g., straight-line)

**Applications**:
- Game AI pathfinding
- Robotics navigation
- Map routing

#### 3. All Pairs Shortest Paths

**Use Case**: Precompute distances between all nodes
**Algorithm**: Floyd-Warshall

**Trade-off**:
- O(n³) computation
- Fast lookups after precomputation

**Applications**:
- Network analysis
- Distance matrices
- Graph metrics

---

### Link Analysis

#### 1. Jaccard Similarity

**Measures**: Overlap between neighbor sets
**Formula**: |A ∩ B| / |A ∪ B|

```cypher
// Find similar users based on common connections
MATCH (alice:Person {name: "Alice"})-[:KNOWS]-(mutual)-[:KNOWS]-(bob:Person)
WHERE alice <> bob
WITH alice, bob, COUNT(DISTINCT mutual) AS common
MATCH (alice)-[:KNOWS]-(aFriend)
WITH alice, bob, common, COUNT(DISTINCT aFriend) AS aliceCount
MATCH (bob)-[:KNOWS]-(bFriend)
WITH alice, bob, common, aliceCount, COUNT(DISTINCT bFriend) AS bobCount
RETURN bob.name,
       common * 1.0 / (aliceCount + bobCount - common) AS jaccardSimilarity
ORDER BY jaccardSimilarity DESC
```

**Applications**:
- Friend recommendations
- Product similarity
- Collaborative filtering

#### 2. Adamic-Adar Index

**Measures**: Weighted common neighbors (rare connections count more)
**Formula**: Sum of 1/log(degree) for common neighbors

**Use Case**: Link prediction (predict future connections)

**Applications**:
- Social network link prediction
- Recommendation systems
- Missing link inference

#### 3. Preferential Attachment

**Concept**: Rich get richer (popular nodes attract more connections)
**Use Case**: Model network growth

**Applications**:
- Social network evolution
- Citation networks
- Economic networks

---

### Graph Machine Learning

#### 1. Node Embeddings (Node2Vec, DeepWalk)

**Concept**: Represent nodes as vectors
**Use Case**: ML features from graph structure

**Process**:
1. Generate random walks from each node
2. Treat walks as sentences
3. Use Word2Vec-style embedding

**Applications**:
- Node classification
- Link prediction
- Anomaly detection

#### 2. Graph Neural Networks (GNN)

**Concept**: Neural networks on graph structure
**Use Case**: Learn patterns in graph data

**Types**:
- Graph Convolutional Networks (GCN)
- Graph Attention Networks (GAT)
- GraphSAGE

**Applications**:
- Molecular property prediction
- Fraud detection
- Traffic prediction
- Recommendation systems

#### 3. Graph Classification

**Use Case**: Classify entire graphs
**Examples**:
- Molecule classification
- Social network categorization
- Protein function prediction

---

## Anti-Patterns to Avoid

### 1. Dense Node Anti-Pattern

**Problem**: Node with millions of relationships
**Solution**: Use buckets, sampling, or denormalization (see Supernode pattern above)

### 2. Property as Relationship Anti-Pattern

**Problem**: Storing relationship data as node property

```cypher
// Bad: String concatenation of friends
(:Person {name: "Alice", friends: "Bob,Charlie,David"})

// Good: Use relationships
(:Person {name: "Alice"})-[:KNOWS]->(:Person {name: "Bob"})
```

### 3. Over-Indexing Anti-Pattern

**Problem**: Indexing every property
**Solution**: Index only frequently queried properties

### 4. Cartesian Product Anti-Pattern

**Problem**: Unfiltered cross products in queries

```cypher
// Bad: Creates cartesian product
MATCH (p:Person), (c:Company)
RETURN p.name, c.name

// Good: Use relationships
MATCH (p:Person)-[:WORKS_FOR]->(c:Company)
RETURN p.name, c.name
```

### 5. Deep Traversal Anti-Pattern

**Problem**: Unbounded traversals
**Solution**: Always limit traversal depth

```cypher
// Bad: Can traverse entire graph
MATCH (alice:Person {name: "Alice"})-[:KNOWS*]-(anyone)
RETURN anyone

// Good: Limit depth
MATCH (alice:Person {name: "Alice"})-[:KNOWS*1..3]-(friend)
RETURN friend
```

---

## Performance Optimization Patterns

### 1. Index Strategy

**Primary Key Indexes**:
```cypher
CREATE CONSTRAINT ON (p:Person) ASSERT p.id IS UNIQUE;
```

**Property Indexes**:
```cypher
CREATE INDEX person_name FOR (p:Person) ON (p.name);
```

**Composite Indexes**:
```cypher
CREATE INDEX person_name_age FOR (p:Person) ON (p.name, p.age);
```

### 2. Query Optimization

**Use PROFILE to analyze**:
```cypher
PROFILE
MATCH (p:Person {name: "Alice"})-[:KNOWS*2]-(friend)
RETURN friend.name
```

**Eager Loading**:
```cypher
// Load related data in one query
MATCH (p:Person {name: "Alice"})
OPTIONAL MATCH (p)-[:KNOWS]->(friend)
OPTIONAL MATCH (p)-[:WORKS_FOR]->(company)
RETURN p, COLLECT(friend) AS friends, company
```

### 3. Caching Pattern

**Materialize expensive queries**:
```cypher
// Cache friend-of-friend recommendations
MATCH (person:Person)-[:KNOWS]-(friend)-[:KNOWS]-(fof)
WHERE NOT (person)-[:KNOWS]-(fof) AND person <> fof
WITH person, fof, COUNT(*) AS mutualFriends
MERGE (person)-[r:RECOMMENDED_FRIEND]->(fof)
SET r.mutualFriends = mutualFriends
```

### 4. Batch Processing Pattern

**Process in batches for large imports**:
```cypher
// Using APOC periodic commit
CALL apoc.periodic.iterate(
  "LOAD CSV WITH HEADERS FROM 'file:///data.csv' AS row RETURN row",
  "CREATE (p:Person {name: row.name, age: toInteger(row.age)})",
  {batchSize: 1000, parallel: false}
)
```

---

## Real-World Use Case Patterns

### Fraud Detection Pattern

**Pattern**: Multi-hop ring detection

```cypher
// Find fraud rings (accounts sharing identifiers)
MATCH ring = (a1:Account)-[:SHARES_PHONE|SHARES_ADDRESS|SHARES_DEVICE*2..4]-(a2:Account)
WHERE a1.id < a2.id
WITH ring,
     [node IN nodes(ring) | node.suspiciousActivity] AS activities
WHERE ANY(activity IN activities WHERE activity = true)
RETURN ring, LENGTH(ring) AS ringSize
ORDER BY ringSize DESC
```

### Recommendation Pattern

**Pattern**: Collaborative filtering via graph

```cypher
// Recommend products based on similar users
MATCH (user:User {id: $userId})-[:PURCHASED]->(p:Product)<-[:PURCHASED]-(similar:User)
MATCH (similar)-[:PURCHASED]->(rec:Product)
WHERE NOT (user)-[:PURCHASED]->(rec)
WITH rec, COUNT(DISTINCT similar) AS commonUsers
RETURN rec.name, commonUsers
ORDER BY commonUsers DESC
LIMIT 10
```

### Impact Analysis Pattern

**Pattern**: Dependency traversal

```cypher
// Find all services affected by database change
MATCH (db:Database {name: "UserDB"})<-[:DEPENDS_ON*]-(affected)
RETURN affected.name, affected.type
```

### Access Control Pattern

**Pattern**: Hierarchical permissions

```cypher
// Check if user has access (direct or inherited)
MATCH (user:User {id: $userId})-[:MEMBER_OF*0..5]->(group:Group)
      -[:HAS_PERMISSION]->(resource:Resource {id: $resourceId})
RETURN COUNT(resource) > 0 AS hasAccess
```

---

## Resources

### Books
- "Graph Algorithms" by Needham & Hodler (O'Reilly)
- "Graph Databases" by Robinson, Webber & Eifrem (O'Reilly)
- "Network Science" by Barabási

### Libraries
- **Neo4j Graph Data Science**: Algorithms library for Neo4j
- **NetworkX** (Python): Graph algorithms and analysis
- **igraph** (R/Python/C): Fast graph library
- **Apache TinkerPop**: Graph computing framework

### Online Resources
- [Neo4j Graph Academy](https://graphacademy.neo4j.com/)
- [Graph Algorithms Playground](https://neo4j.com/sandbox/)
- [Awesome Graph](https://github.com/jbmusso/awesome-graph) - Curated resources

---

## Summary

**Key Takeaways**:

1. **Model for your queries** - Design graph structure based on access patterns
2. **Avoid supernodes** - Use bucketing or denormalization
3. **Limit traversal depth** - Always bound variable-length paths
4. **Index strategically** - Index frequently queried properties
5. **Leverage algorithms** - Use proven graph algorithms for analysis
6. **Test at scale** - Performance patterns that work small may fail large
7. **Monitor and optimize** - Profile queries and iterate

**Choosing Patterns**:
- Simple relationships → Entity-Relationship pattern
- N-ary relationships → Hyperedge pattern
- Temporal data → Time-based versioning
- Hierarchies → Tree pattern with materialized paths
- Sequences → Linked list pattern
- High-degree nodes → Bucketing or fanout patterns
