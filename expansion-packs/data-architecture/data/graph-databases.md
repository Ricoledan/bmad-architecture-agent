# Graph Databases

## Overview

Graph databases are purpose-built to store and navigate relationships. They use graph structures with nodes (entities), edges (relationships), and properties to represent and store data. Graph databases excel at queries that traverse relationships, making them ideal for connected data use cases.

## Core Concepts

### Nodes (Vertices)
- Represent entities (people, products, locations, etc.)
- Contain properties (key-value pairs)
- Can have labels/types to categorize entities
- Example: Person node with properties {name: "Alice", age: 30}

### Edges (Relationships)
- Connect nodes to represent relationships
- Have direction (from source to target)
- Contain properties describing the relationship
- Have types/labels (FRIENDS_WITH, PURCHASED, WORKS_FOR)
- Example: Alice -[FRIENDS_WITH {since: 2020}]-> Bob

### Properties
- Key-value pairs attached to nodes or edges
- Store attributes and metadata
- Can be indexed for fast lookups
- Support various data types (strings, numbers, dates, arrays)

### Labels/Types
- Categorize nodes and relationships
- Enable efficient filtering and querying
- Support multiple labels per node
- Example: Person, Employee, Customer labels on same node

## Graph Database Technologies

### 1. Neo4j

**Type**: Native property graph database

**Strengths**:
- Most mature and popular graph database
- Native graph storage and processing
- Cypher query language (intuitive, SQL-like)
- ACID transactions
- Rich ecosystem and tooling
- Excellent visualization tools

**Use Cases**:
- Fraud detection
- Real-time recommendations
- Network and IT operations
- Master data management
- Identity and access management
- Social networks

**Query Language**: Cypher
```cypher
MATCH (person:Person)-[:FRIENDS_WITH]->(friend)
WHERE person.name = 'Alice'
RETURN friend.name, friend.age
```

**Deployment Options**:
- Self-hosted (Community/Enterprise)
- Neo4j Aura (fully managed cloud)
- Available on AWS, Azure, GCP

**Pricing Model**:
- Community Edition: Free, limited features
- Enterprise: Starts at $3,000/month
- Aura: Pay-as-you-go, starting at $65/month

**Best For**:
- Complex relationship queries
- Real-time graph analytics
- Applications requiring ACID compliance

---

### 2. Amazon Neptune

**Type**: Fully managed graph database service

**Strengths**:
- Managed service (no infrastructure management)
- Dual graph models (property graph + RDF)
- High availability (6 copies across 3 AZs)
- Read replicas for scaling
- Integration with AWS ecosystem
- Point-in-time recovery
- Encryption at rest and in transit

**Use Cases**:
- Knowledge graphs
- Recommendation engines
- Fraud detection
- Network security
- Life sciences (drug discovery)
- Social networking

**Query Languages**:
- **Gremlin** (Apache TinkerPop) for property graphs
- **SPARQL** for RDF graphs

**Gremlin Example**:
```groovy
g.V().hasLabel('person').has('name', 'Alice')
  .out('friendsWith')
  .values('name')
```

**SPARQL Example**:
```sparql
SELECT ?friendName
WHERE {
  ?alice rdf:type :Person ;
         :name "Alice" ;
         :friendsWith ?friend .
  ?friend :name ?friendName .
}
```

**Deployment**:
- Regional service across multiple AZs
- Available in 20+ AWS regions
- VPC isolation for security

**Pricing Model**:
- Instance-based: $0.10 - $5.76 per hour (varies by instance type)
- Storage: $0.10 per GB/month
- I/O requests: $0.20 per million requests
- Backup storage: $0.021 per GB/month

**Instance Types**:
- db.t3.medium: 2 vCPU, 4 GB RAM - $0.102/hour
- db.r5.large: 2 vCPU, 16 GB RAM - $0.348/hour
- db.r5.8xlarge: 32 vCPU, 256 GB RAM - $5.76/hour

**Best For**:
- AWS-native applications
- Need for both property graph and RDF
- High availability requirements
- Managed service preference

---

### 3. Azure Cosmos DB (Gremlin API)

**Type**: Globally distributed multi-model database with graph support

**Strengths**:
- Global distribution (turnkey replication)
- Multiple consistency levels (5 options)
- Multi-model (document, graph, key-value, table)
- Guaranteed low latency (<10ms reads, <15ms writes)
- Automatic indexing
- Elastic scalability
- 99.999% availability SLA

**Use Cases**:
- IoT and telemetry
- Real-time recommendations
- Social graphs
- Fraud detection
- Content management with relationships

**Query Language**: Gremlin (Apache TinkerPop)

**Gremlin Example**:
```groovy
g.V().hasLabel('person').has('name', 'Alice')
  .outE('friendsWith')
  .inV()
  .values('name')
```

**Deployment**:
- 60+ Azure regions worldwide
- Multi-region writes
- Automatic failover

**Pricing Models**:
- **Provisioned throughput**: $0.008 per 100 RU/s per hour
- **Serverless**: $0.25 per million RUs consumed
- **Autoscale**: $0.012 per 100 RU/s per hour
- Storage: $0.25 per GB/month

**Consistency Levels**:
1. Strong: Linearizability guarantee
2. Bounded staleness: Consistent prefix with lag bound
3. Session: Consistent within user session
4. Consistent prefix: Reads never see out-of-order writes
5. Eventual: No ordering guarantee

**Best For**:
- Azure-native applications
- Global distribution requirements
- Multi-model data (graph + document)
- Flexible consistency needs

---

### 4. Google Cloud Spanner (Graph Support - Preview)

**Type**: Distributed relational database with graph capabilities

**Note**: Graph features in preview as of 2025

**Strengths**:
- Global distribution with strong consistency
- Horizontal scalability
- ACID transactions
- SQL interface with graph extensions
- Integrates with BigQuery for analytics

**Use Cases**:
- Financial applications requiring ACID + graphs
- Global applications with graph needs
- Combining relational and graph data

**Query Language**: SQL with Graph extensions (GQL)

**Deployment**:
- Multi-region configurations
- Regional configurations
- Available in 30+ locations

**Pricing Model**:
- Node configuration: $0.90 - $9.00 per node/hour
- Processing units: $0.10 per 100 PU/hour
- Storage: $0.30 per GB/month
- Network: Standard egress rates

**Best For**:
- Enterprises already on GCP
- Need both relational and graph
- Strong consistency requirements

---

### 5. TigerGraph

**Type**: Native parallel graph database

**Strengths**:
- Real-time deep link analytics
- Parallel graph computation
- GSQL query language (SQL-like for graphs)
- Supports graphs with trillions of edges
- In-database machine learning

**Use Cases**:
- Deep fraud detection (multi-hop patterns)
- Supply chain optimization
- Cybersecurity threat detection
- 360° customer view
- Real-time recommendation engines

**Query Language**: GSQL

**GSQL Example**:
```sql
CREATE QUERY findFriends(VERTEX<Person> inputPerson) {
  Start = {inputPerson};
  Result = SELECT tgt
           FROM Start:s -(friendsWith:e)- Person:tgt
           WHERE tgt.age > 25;
  PRINT Result;
}
```

**Deployment Options**:
- Self-hosted
- TigerGraph Cloud (AWS, Azure, GCP)
- Docker containers

**Pricing Model**:
- Enterprise Edition: Custom pricing
- Developer Edition: Free
- Cloud: Starting at $0.65/hour

**Best For**:
- Multi-hop graph analytics (3+ hops)
- Real-time pattern detection
- Graph algorithms at scale

---

### 6. ArangoDB

**Type**: Native multi-model database (document, graph, key-value)

**Strengths**:
- True multi-model in single core
- AQL query language (unifies all models)
- Flexible graph traversals
- Horizontal scalability
- Native search capabilities
- Microservices-friendly

**Use Cases**:
- Applications requiring multiple data models
- Content management systems
- Recommendation engines
- Network topology analysis

**Query Language**: AQL (ArangoDB Query Language)

**AQL Example**:
```sql
FOR person IN persons
  FILTER person.name == "Alice"
  FOR friend IN 1..1 OUTBOUND person friendsWith
  RETURN friend.name
```

**Deployment Options**:
- Self-hosted (Community/Enterprise)
- ArangoDB Oasis (managed cloud)
- Kubernetes operator

**Pricing Model**:
- Community Edition: Free
- Enterprise: Custom pricing
- Oasis: Pay-as-you-go, starting at $0.10/hour

**Best For**:
- Multi-model requirements
- Flexible schema needs
- Cloud-native architectures

---

### 7. JanusGraph

**Type**: Distributed open-source graph database

**Strengths**:
- Open source (Apache 2.0 license)
- Highly scalable (billions of nodes/edges)
- Pluggable storage backends (Cassandra, HBase, BerkeleyDB)
- Elastic indexing (Elasticsearch, Solr, Lucene)
- Support for geo and numeric search
- Gremlin query language

**Use Cases**:
- Large-scale graph analytics
- Time-series graph data
- IoT device networks
- Custom graph solutions

**Query Language**: Gremlin

**Storage Backends**:
- Apache Cassandra (recommended for distributed)
- Apache HBase
- BerkeleyDB (single machine)
- Google Cloud Bigtable

**Deployment Options**:
- Self-hosted
- Docker/Kubernetes
- Available on all cloud providers

**Pricing Model**:
- Free (open source)
- Pay for infrastructure only

**Best For**:
- Open-source requirements
- Custom storage/indexing needs
- Budget-conscious projects
- Massive scale requirements

---

## Query Language Comparison

### Cypher (Neo4j)
- **Syntax**: ASCII-art style, intuitive
- **Readability**: High (SQL-like declarative)
- **Learning curve**: Low
- **Pattern matching**: Excellent
- **Example**:
```cypher
MATCH (a:Person)-[:KNOWS*1..3]->(b:Person)
WHERE a.name = 'Alice'
RETURN b.name, length(path) AS degrees
```

### Gremlin (Neptune, Cosmos DB, JanusGraph)
- **Syntax**: Traversal-based, functional style
- **Readability**: Medium (more programmatic)
- **Learning curve**: Medium
- **Flexibility**: High
- **Example**:
```groovy
g.V().has('person', 'name', 'Alice')
  .repeat(out('knows')).times(3)
  .values('name')
```

### GSQL (TigerGraph)
- **Syntax**: SQL-like with graph extensions
- **Readability**: High (familiar to SQL developers)
- **Learning curve**: Low (if know SQL)
- **Performance**: Optimized for parallel execution
- **Example**:
```sql
SELECT friend.name
FROM Person:p -(knows:e)-> Person:friend
WHERE p.name == "Alice"
```

### AQL (ArangoDB)
- **Syntax**: SQL-like, multi-model
- **Readability**: High
- **Learning curve**: Low
- **Flexibility**: Works across document/graph/key-value
- **Example**:
```sql
FOR p IN Person
  FILTER p.name == "Alice"
  FOR friend IN 1..3 OUTBOUND p knows
  RETURN DISTINCT friend.name
```

### SPARQL (Neptune RDF)
- **Syntax**: RDF-specific, triple patterns
- **Readability**: Medium
- **Learning curve**: High (requires RDF understanding)
- **Semantic web**: Native support
- **Example**:
```sparql
SELECT ?friendName
WHERE {
  ?alice :name "Alice" .
  ?alice :knows+ ?friend .
  ?friend :name ?friendName .
}
```

---

## Graph Database Selection Guide

### Choose Neo4j When:
- Need mature, production-proven graph database
- Want intuitive Cypher query language
- Require ACID transactions
- Need rich visualization and tooling
- Pattern matching is primary use case
- Budget allows for enterprise license

### Choose Amazon Neptune When:
- Building on AWS infrastructure
- Need fully managed service
- Require high availability (multi-AZ)
- Want both property graph and RDF
- Integration with AWS services is important
- Can tolerate vendor lock-in

### Choose Azure Cosmos DB (Gremlin) When:
- Building on Azure infrastructure
- Need global distribution
- Require multiple consistency levels
- Want multi-model capabilities
- Low latency is critical (<10ms)
- Applications are globally distributed

### Choose TigerGraph When:
- Need deep multi-hop analytics (3+ hops)
- Require real-time pattern detection
- Working with massive graphs (trillions of edges)
- Performance is critical
- Need in-database ML capabilities

### Choose ArangoDB When:
- Need multiple data models in one database
- Want flexible schema
- Building microservices
- Need native search capabilities
- Prefer SQL-like query language

### Choose JanusGraph When:
- Open source is requirement
- Need massive scalability
- Want pluggable storage backends
- Have existing Cassandra/HBase infrastructure
- Budget is constrained

---

## Performance Considerations

### Indexing
- Index frequently queried properties
- Composite indexes for multi-property lookups
- Full-text indexes for search
- Trade-off: Faster reads, slower writes

### Data Modeling
- Avoid over-connecting (too many relationships)
- Use relationship properties to filter
- Denormalize when appropriate
- Model for your query patterns

### Query Optimization
- Limit traversal depth when possible
- Use index hints
- Filter early in traversal
- Avoid Cartesian products

### Scaling Strategies
- **Read replicas**: Scale read-heavy workloads
- **Sharding/Partitioning**: Distribute large graphs
- **Caching**: Cache frequent traversals
- **Denormalization**: Pre-compute common paths

---

## Integration Patterns

### Graph + Relational
- Use RDBMS for transactional data
- Use graph for relationship-heavy queries
- Sync via ETL or CDC (Change Data Capture)
- Example: User profiles in PostgreSQL, social connections in Neo4j

### Graph + Document Store
- Document store for flexible schemas
- Graph for relationships between documents
- Example: MongoDB for product data, Neptune for recommendations

### Graph + Data Warehouse
- Graph for operational queries
- Warehouse for analytics and BI
- Example: Neo4j for real-time fraud detection, Snowflake for historical analysis

### Graph + Search
- Graph for traversals
- Search engine for full-text
- Example: Neptune + Elasticsearch for semantic search

---

## Common Use Cases

### 1. Fraud Detection
**Pattern**: Identify suspicious relationship patterns
**Example**: Multiple accounts sharing phone numbers, addresses, devices
**Queries**:
- Find rings of connected accounts
- Detect velocity of transactions across network
- Identify mule accounts

### 2. Recommendation Engines
**Pattern**: Collaborative filtering via graph traversal
**Example**: "Users who liked X also liked Y"
**Queries**:
- Friend-of-friend recommendations
- Item similarity based on co-purchases
- Personalized content suggestions

### 3. Network and IT Operations
**Pattern**: Model infrastructure dependencies
**Example**: Service dependencies, root cause analysis
**Queries**:
- Impact analysis (what breaks if this fails?)
- Shortest path between services
- Identify single points of failure

### 4. Knowledge Graphs
**Pattern**: Semantic relationships between entities
**Example**: Wikipedia-style knowledge base
**Queries**:
- Find related concepts
- Reasoning and inference
- Question answering

### 5. Identity and Access Management
**Pattern**: Complex permission hierarchies
**Example**: Role-based access control (RBAC)
**Queries**:
- What can user X access?
- Who has access to resource Y?
- Inherited permissions via groups

### 6. Master Data Management
**Pattern**: Link and deduplicate entities
**Example**: Customer 360 view
**Queries**:
- Find all records for same person
- Merge duplicate entities
- Track data lineage

---

## Cloud Provider Comparison

| Feature | AWS Neptune | Azure Cosmos DB | GCP (Spanner Graph) |
|---------|-------------|-----------------|---------------------|
| **Graph Model** | Property Graph + RDF | Property Graph | Property Graph |
| **Query Language** | Gremlin, SPARQL | Gremlin | SQL/GQL |
| **Deployment** | Regional (multi-AZ) | Global | Multi-region |
| **Consistency** | Eventual | 5 levels | Strong |
| **Scaling** | Vertical + Read replicas | Horizontal (automatic) | Horizontal |
| **Pricing** | Instance-based | Throughput-based | Node/PU-based |
| **Starting Cost** | ~$100/month | ~$25/month (serverless) | ~$200/month |
| **High Availability** | 99.9% SLA | 99.999% SLA | 99.999% SLA |
| **Backup** | Automated, PITR | Automatic | Automatic |
| **Best For** | AWS ecosystem | Global apps | Relational + graph |

---

## Migration Strategies

### From Relational to Graph

1. **Identify relationships** that are foreign keys
2. **Convert tables to nodes** (entities become nodes)
3. **Convert join tables to edges** (many-to-many relationships)
4. **Model hierarchies** as edges (parent-child, org charts)
5. **Migrate data** in batches with validation
6. **Optimize queries** to leverage traversals

### From Document Store to Graph

1. **Extract relationships** from embedded documents
2. **Create edges** for references between documents
3. **Preserve document properties** as node properties
4. **Use hybrid approach** (documents + graph) if needed

### From One Graph DB to Another

1. **Export in universal format** (GraphML, JSON, CSV)
2. **Map query languages** (Cypher → Gremlin, etc.)
3. **Test performance** with representative queries
4. **Migrate in phases** with dual-write period

---

## Best Practices

### Data Modeling
- Model relationships explicitly (not as properties)
- Use meaningful relationship types
- Keep node/edge properties simple
- Avoid super-nodes (nodes with millions of edges)
- Model for your queries, not for normalization

### Query Performance
- Use parameters in queries (prevent injection + caching)
- Index properties used in WHERE clauses
- Limit traversal depth
- Profile and optimize slow queries
- Consider denormalization for hot paths

### Security
- Implement role-based access control
- Encrypt data at rest and in transit
- Use VPC/private networks
- Audit all access
- Follow principle of least privilege

### Monitoring
- Track query performance
- Monitor memory usage
- Alert on slow traversals
- Measure cache hit rates
- Monitor replication lag (if using replicas)

### Maintenance
- Regular backups with point-in-time recovery
- Test disaster recovery procedures
- Keep statistics up to date
- Monitor and compact storage
- Plan for version upgrades

---

## Resources

### Neo4j
- [Neo4j Graph Database](https://neo4j.com/)
- [Cypher Query Language](https://neo4j.com/developer/cypher/)
- [Neo4j Aura Cloud](https://neo4j.com/cloud/aura/)

### Amazon Neptune
- [AWS Neptune Documentation](https://docs.aws.amazon.com/neptune/)
- [Gremlin Documentation](https://tinkerpop.apache.org/docs/current/reference/)
- [SPARQL Documentation](https://www.w3.org/TR/sparql11-query/)

### Azure Cosmos DB
- [Cosmos DB Gremlin API](https://docs.microsoft.com/en-us/azure/cosmos-db/graph-introduction)
- [Gremlin in Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/gremlin-support)

### TigerGraph
- [TigerGraph Documentation](https://docs.tigergraph.com/)
- [GSQL Language](https://docs.tigergraph.com/gsql-ref/current/intro/)

### ArangoDB
- [ArangoDB Documentation](https://www.arangodb.com/docs/)
- [AQL Query Language](https://www.arangodb.com/docs/stable/aql/)

### JanusGraph
- [JanusGraph Documentation](https://docs.janusgraph.org/)
- [Gremlin Documentation](https://tinkerpop.apache.org/)
