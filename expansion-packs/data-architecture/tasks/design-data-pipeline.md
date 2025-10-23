# Design Data Pipeline Task

## Purpose
Design a robust data pipeline for extracting, transforming, and loading (ETL) or extracting, loading, and transforming (ELT) data between systems with proper orchestration, monitoring, and error handling.

## Task Flow

### Step 1: Pipeline Requirements Analysis
Understand pipeline needs:
1. **Data Sources**: Identify all source systems (databases, APIs, files, streams)
2. **Data Destinations**: Target systems (warehouse, lake, analytics platform)
3. **Data Volume**: Records per day/hour, data size, growth rate
4. **Latency Requirements**: Real-time, near-real-time, batch (daily, hourly)
5. **SLA Requirements**: Data freshness, completeness, accuracy
6. **Compliance**: Data privacy, retention, audit requirements

### Step 2: Pipeline Architecture Selection

#### Batch vs Streaming Decision
**Batch Processing** (daily, hourly):
- Use for: Large volume, non-urgent data
- Tools: Apache Airflow, Luigi, AWS Glue, Azure Data Factory
- Benefits: Simpler, cost-effective, easier error recovery
- Drawbacks: Higher latency, resource-intensive

**Stream Processing** (real-time, micro-batch):
- Use for: Low-latency, continuous data flow
- Tools: Apache Kafka, Kinesis, Flink, Spark Streaming
- Benefits: Real-time insights, lower latency
- Drawbacks: More complex, higher operational overhead

**Hybrid Approach**:
- Batch for historical data and heavy transformations
- Streaming for real-time events and critical updates

#### ETL vs ELT Decision
**ETL (Extract, Transform, Load)**:
- Transform data before loading to destination
- Use for: Data warehouses, complex transformations
- Benefits: Validated data, reduced destination load
- Drawbacks: More staging storage, slower processing

**ELT (Extract, Load, Transform)**:
- Load raw data, transform in destination
- Use for: Cloud data warehouses (Snowflake, BigQuery, Redshift)
- Benefits: Faster ingestion, leverage warehouse compute
- Drawbacks: Raw data quality issues, destination resources

### Step 3: Data Extraction Design

#### Source System Integration
1. **Database Extraction**:
   - Full load vs incremental load (CDC, timestamp-based)
   - Query optimization to avoid source overload
   - Connection pooling and timeout configuration
2. **API Extraction**:
   - REST API pagination and rate limiting
   - Authentication and token management
   - Retry logic and circuit breakers
3. **File Extraction**:
   - File formats (CSV, JSON, Parquet, Avro)
   - File monitoring and pickup patterns
   - Compression and encryption handling
4. **Stream Extraction**:
   - Message queue consumption (Kafka, SQS, Pub/Sub)
   - Offset management and checkpointing
   - Backpressure handling

#### Change Data Capture (CDC)
- Use database logs (binary logs, WAL) for incremental changes
- Tools: Debezium, AWS DMS, GoldenGate
- Benefits: Minimal source impact, real-time changes
- Considerations: Schema change handling, log retention

### Step 4: Data Transformation Design

#### Transformation Types
1. **Cleansing**:
   - Remove duplicates
   - Handle null values and defaults
   - Fix formatting inconsistencies
   - Validate data quality rules
2. **Standardization**:
   - Normalize formats (dates, phone numbers, addresses)
   - Convert units and currencies
   - Standardize codes and enumerations
3. **Enrichment**:
   - Join with reference data
   - Calculate derived fields
   - Add metadata (processing timestamp, source system)
4. **Aggregation**:
   - Pre-aggregate for analytics
   - Calculate metrics and KPIs
   - Roll-up hierarchies (daily to monthly)
5. **Business Logic**:
   - Apply business rules
   - Implement slowly changing dimensions (SCD)
   - Handle complex calculations

#### Transformation Tools
- **SQL-based**: dbt, stored procedures, SQL queries
- **Code-based**: Python (Pandas, PySpark), Scala (Spark)
- **Visual ETL**: Informatica, Talend, SSIS
- **Cloud-native**: AWS Glue, Azure Data Factory, Dataflow

### Step 5: Data Loading Design

#### Loading Strategies
1. **Full Load**:
   - Truncate and reload entire dataset
   - Simple but inefficient for large datasets
   - Use for small dimensions or complete refreshes
2. **Incremental Load**:
   - Append new records only
   - Upsert (update existing, insert new)
   - Delete handling (soft delete vs hard delete)
3. **Partition Management**:
   - Replace partitions (e.g., daily partitions)
   - Merge into existing partitions
   - Archive old partitions
4. **Performance Optimization**:
   - Bulk loading (COPY, bulk insert)
   - Parallel loading across partitions
   - Disable indexes during load, rebuild after
   - Batch commits for transaction efficiency

#### Data Quality Checks
1. **Pre-Load Validation**:
   - Schema validation
   - Data type checking
   - Referential integrity validation
   - Business rule validation
2. **Post-Load Validation**:
   - Record count reconciliation
   - Data completeness checks
   - Statistical anomaly detection
   - Comparison with source checksums

### Step 6: Pipeline Orchestration

#### Workflow Design (DAG)
1. **Task Dependencies**: Define execution order and dependencies
2. **Parallel Execution**: Identify independent tasks for concurrency
3. **Conditional Logic**: Branching based on data conditions
4. **Retry Logic**: Configure retries for transient failures
5. **Alerting**: Set up notifications for failures

#### Orchestration Tools
- **Apache Airflow**: Python-based, flexible, open-source
- **Prefect**: Modern Python orchestration
- **Dagster**: Software-defined assets, data observability
- **Cloud-native**: AWS Step Functions, Azure Data Factory, GCP Composer

#### Scheduling
- **Time-based**: Cron schedules (daily at 2 AM, hourly)
- **Event-based**: Trigger on file arrival, data availability
- **Dependency-based**: Wait for upstream data completion
- **Manual**: On-demand execution for backfills

### Step 7: Error Handling and Recovery

#### Error Handling Strategy
1. **Transient Errors**: Automatic retry with exponential backoff
2. **Data Quality Errors**: Quarantine bad records, alert data team
3. **System Errors**: Circuit breakers, fallback mechanisms
4. **Partial Failures**: Continue processing, flag incomplete runs

#### Recovery Mechanisms
1. **Idempotency**: Design for safe re-execution
2. **Checkpointing**: Save progress, resume from last checkpoint
3. **Backfill**: Reprocess historical data windows
4. **Dead Letter Queue**: Store failed records for manual review

### Step 8: Monitoring and Observability

#### Pipeline Monitoring
1. **Execution Metrics**:
   - Run duration and success rate
   - Records processed, failed, skipped
   - Data volume and throughput
2. **Data Quality Metrics**:
   - Completeness, accuracy, timeliness
   - Schema drift detection
   - Anomaly detection (unexpected spikes/drops)
3. **Infrastructure Metrics**:
   - CPU, memory, disk usage
   - Network throughput
   - Database connection pool usage

#### Alerting Strategy
1. **Critical Alerts**: Pipeline failures, SLA breaches
2. **Warning Alerts**: Data quality issues, performance degradation
3. **Info Alerts**: Successful completions, summary reports

#### Observability Tools
- Logs: Centralized logging (ELK, CloudWatch, Datadog)
- Metrics: Time-series metrics (Prometheus, Grafana, CloudWatch)
- Traces: Distributed tracing (Jaeger, X-Ray)
- Data Quality: Great Expectations, Soda, dbt tests

### Step 9: Data Lineage and Metadata

#### Lineage Tracking
1. **Source-to-Target Mapping**: Document data flows
2. **Transformation Logic**: Track business rules applied
3. **Impact Analysis**: Understand downstream dependencies
4. **Compliance**: Audit trails for regulatory requirements

#### Metadata Management
- **Technical Metadata**: Schemas, data types, formats
- **Business Metadata**: Definitions, ownership, glossary
- **Operational Metadata**: Run times, data volumes, quality scores
- **Tools**: Apache Atlas, AWS Glue Data Catalog, Alation

### Step 10: Testing Strategy

#### Testing Levels
1. **Unit Tests**: Test individual transformation functions
2. **Integration Tests**: Test end-to-end pipeline flows
3. **Data Quality Tests**: Validate output data quality
4. **Performance Tests**: Load testing with production volumes
5. **Regression Tests**: Ensure changes don't break existing pipelines

## Output

Create the following deliverables:

1. **Pipeline Architecture Diagram**: Visual representation of data flows
2. **Pipeline Specification Document**:
   - Source and destination details
   - Transformation logic and business rules
   - Scheduling and orchestration design
   - Error handling and recovery procedures
3. **Data Flow Diagrams**: Detailed data lineage
4. **Orchestration DAG**: Task dependencies and execution flow
5. **Monitoring Dashboard**: Metrics and alerts configuration
6. **Runbook**: Operational procedures for pipeline management
7. **Testing Plan**: Test scenarios and validation criteria

## Quality Criteria

Pipeline design must meet:
- **Reliability**: Handle failures gracefully with retries
- **Scalability**: Process growing data volumes efficiently
- **Maintainability**: Clear code, modular design, documented
- **Observability**: Comprehensive monitoring and logging
- **Data Quality**: Validation at every stage
- **Performance**: Meet SLA for data freshness
- **Idempotency**: Safe to re-run without side effects

## Handoff

Provide to:
- **Platform Engineer**: For infrastructure provisioning
- **Data Engineer**: For pipeline implementation
- **Data Analyst**: For data availability and quality SLAs
- **Operations Team**: For monitoring and support
