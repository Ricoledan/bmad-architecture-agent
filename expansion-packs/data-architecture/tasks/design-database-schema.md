# Design Database Schema Task

## Purpose
Design a comprehensive database schema with proper normalization, indexing, and performance optimization for the application requirements.

## Task Flow

### Step 1: Understand Requirements
Gather information about:
1. **Entities**: What are the main business objects (users, products, orders, etc.)?
2. **Relationships**: How do entities relate to each other (one-to-one, one-to-many, many-to-many)?
3. **Access Patterns**: What are the most common queries and operations?
4. **Data Volume**: Expected record counts and growth rate
5. **Performance Requirements**: Query response time targets, throughput needs
6. **Transactional Requirements**: ACID guarantees, isolation levels needed

### Step 2: Create Conceptual Model
Design the high-level data model:
1. Identify all entities and their attributes
2. Define relationships and cardinality
3. Identify business rules and constraints
4. Create initial Entity-Relationship Diagram (ERD)
5. Validate with stakeholders

### Step 3: Logical Data Model
Design the normalized schema:
1. **Define Tables**: Convert entities to tables
2. **Define Columns**: List all attributes with data types
3. **Primary Keys**: Define unique identifiers for each table
4. **Foreign Keys**: Establish referential integrity
5. **Normalization**: Apply 3NF or BCNF to eliminate redundancy
6. **Constraints**: Add unique, not null, check constraints
7. **Indexes**: Plan indexes for common queries

#### Normalization Guidelines
- **1NF**: Eliminate repeating groups, atomic values
- **2NF**: Remove partial dependencies on composite keys
- **3NF**: Remove transitive dependencies
- **BCNF**: Every determinant is a candidate key
- **Denormalization**: Consider for read-heavy analytical workloads

### Step 4: Physical Data Model
Translate logical model to physical implementation:
1. **Database Selection**: Choose appropriate database technology
2. **Data Types**: Select optimal data types for each column
   - Use appropriate numeric types (INT, BIGINT, DECIMAL)
   - Choose string types wisely (VARCHAR vs TEXT)
   - Use date/time types correctly (DATE, TIMESTAMP, TIMESTAMPTZ)
   - Consider JSON/JSONB for flexible schemas
3. **Storage Optimization**:
   - Define tablespaces or filegroups
   - Configure compression where appropriate
   - Plan for large object storage (BLOBs, CLOBs)

### Step 5: Indexing Strategy
Design indexes for performance:
1. **Primary Indexes**: Clustered index on primary key
2. **Secondary Indexes**: B-tree indexes on frequently queried columns
3. **Composite Indexes**: Multi-column indexes for complex queries
4. **Unique Indexes**: Enforce uniqueness constraints
5. **Full-Text Indexes**: For text search capabilities
6. **Covering Indexes**: Include columns to avoid table lookups
7. **Index Considerations**:
   - Balance query performance vs write overhead
   - Monitor index usage and remove unused indexes
   - Consider partial indexes for filtered queries

### Step 6: Partitioning Strategy
Plan partitioning for large tables:
1. **Range Partitioning**: By date ranges (common for time-series)
2. **List Partitioning**: By discrete values (region, category)
3. **Hash Partitioning**: For even distribution
4. **Composite Partitioning**: Combination of strategies
5. **Benefits**: Improved query performance, easier maintenance, parallel processing

### Step 7: Scalability and High Availability
Design for scale:
1. **Replication**: Configure primary-replica for read scaling
2. **Sharding**: Horizontal partitioning across databases
3. **Connection Pooling**: Configure appropriate pool sizes
4. **Caching**: Design caching strategy (Redis, Memcached)
5. **Backup Strategy**: Regular backups, point-in-time recovery
6. **Disaster Recovery**: Multi-region replication if needed

### Step 8: Data Integrity and Constraints
Ensure data quality:
1. **Referential Integrity**: Foreign key constraints
2. **Check Constraints**: Validate data ranges and formats
3. **Unique Constraints**: Enforce uniqueness beyond primary keys
4. **Default Values**: Set appropriate defaults
5. **Triggers**: Implement business logic (use sparingly)
6. **Audit Columns**: created_at, updated_at, created_by, updated_by

### Step 9: Security Design
Implement data security:
1. **Access Control**: Define roles and permissions
2. **Encryption at Rest**: Enable database encryption
3. **Encryption in Transit**: Require TLS connections
4. **Row-Level Security**: Filter data by user context
5. **Column-Level Encryption**: Encrypt sensitive fields
6. **Audit Logging**: Track data access and modifications

### Step 10: Documentation
Create comprehensive documentation:
1. **ERD Diagram**: Visual representation of schema
2. **Data Dictionary**: Tables, columns, types, descriptions
3. **Index Documentation**: Purpose and usage of each index
4. **Relationship Documentation**: Foreign keys and cardinality
5. **Business Rules**: Constraints and validation logic
6. **Access Patterns**: Common queries and their optimization

## Output

Create the following deliverables:

1. **Entity-Relationship Diagram (ERD)**
2. **Data Dictionary** with:
   - Table names and descriptions
   - Column definitions (name, type, nullable, default)
   - Primary keys and foreign keys
   - Indexes and constraints
   - Relationships and cardinality
3. **DDL Scripts**:
   - CREATE TABLE statements
   - CREATE INDEX statements
   - ALTER TABLE for constraints
   - Migration scripts for versioning
4. **Performance Analysis**:
   - Expected query patterns
   - Index usage justification
   - Partitioning strategy rationale
5. **Implementation Notes**:
   - Database configuration recommendations
   - Capacity planning estimates
   - Monitoring and alerting recommendations

## Quality Criteria

Schema design must meet:
- **Normalization**: Proper 3NF (or justified denormalization)
- **Performance**: Indexed for common queries
- **Integrity**: Referential integrity enforced
- **Scalability**: Can grow with data volume
- **Maintainability**: Clear naming, well-documented
- **Security**: Access controls and encryption
- **Testability**: Can validate data quality

## Handoff

Provide to:
- **Integration Architect**: For API data models
- **Platform Engineer**: For database provisioning
- **Application Developers**: For ORM mapping and queries
- **QA Team**: For data validation testing
