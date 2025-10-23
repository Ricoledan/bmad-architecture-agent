# Database Design Patterns

## Schema Design Patterns

### 1. Normalized Schema (OLTP)
**Use Case**: Transactional systems, high write volume
**Pattern**: 3NF or BCNF normalization
**Benefits**: Data integrity, no redundancy, efficient updates
**Considerations**: May require joins for complex queries

### 2. Denormalized Schema (OLAP)
**Use Case**: Analytics, reporting, data warehouses
**Pattern**: Star schema, snowflake schema
**Benefits**: Fast query performance, simplified queries
**Considerations**: Data redundancy, complex updates

### 3. Star Schema
**Use Case**: Data warehouses, BI
**Pattern**: Central fact table surrounded by dimension tables
**Benefits**: Simple queries, fast aggregations
**Example**: Sales fact with product, customer, time dimensions

### 4. Snowflake Schema
**Use Case**: Complex dimensional hierarchies
**Pattern**: Normalized dimensions
**Benefits**: Reduced redundancy in dimensions
**Considerations**: More joins required

### 5. Single Table Design (NoSQL)
**Use Case**: DynamoDB, key-value stores
**Pattern**: All entities in one table with composite keys
**Benefits**: Fast lookups, no joins
**Considerations**: Complex access patterns, careful key design

### 6. Event Sourcing
**Use Case**: Audit trails, temporal data
**Pattern**: Store all changes as immutable events
**Benefits**: Complete history, time travel queries
**Considerations**: Storage overhead, query complexity

### 7. CQRS (Command Query Responsibility Segregation)
**Use Case**: High read/write workloads
**Pattern**: Separate read and write data models
**Benefits**: Independent scaling, optimized for use case
**Considerations**: Eventual consistency, complexity

## Performance Patterns

### 1. Read Replicas
**Pattern**: Primary for writes, replicas for reads
**Benefits**: Scale read traffic, geographic distribution
**Use Case**: Read-heavy workloads

### 2. Sharding
**Pattern**: Horizontal partitioning across databases
**Benefits**: Scale beyond single database limits
**Strategies**: Range, hash, geo-based sharding

### 3. Caching Layer
**Pattern**: Redis/Memcached in front of database
**Benefits**: Reduce database load, faster response times
**Patterns**: Cache-aside, write-through, write-behind

### 4. Materialized Views
**Pattern**: Pre-computed query results
**Benefits**: Fast complex queries
**Use Case**: Expensive aggregations, reporting

## Data Integrity Patterns

### 1. Soft Delete
**Pattern**: Mark records as deleted, don't physically remove
**Benefits**: Audit trail, recovery possible
**Implementation**: deleted_at timestamp column

### 2. Optimistic Locking
**Pattern**: Version number or timestamp for concurrency
**Benefits**: Better performance than pessimistic locking
**Use Case**: Low contention scenarios

### 3. Audit Columns
**Pattern**: created_at, updated_at, created_by, updated_by
**Benefits**: Track data changes
**Standard**: Include in every table
