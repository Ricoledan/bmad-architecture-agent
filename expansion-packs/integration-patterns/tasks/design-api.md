# Design API Task

## Purpose
Design a comprehensive API (REST, GraphQL, or gRPC) with proper resource modeling, security, versioning, and documentation.

## Task Flow

### Step 1: Requirements Gathering
1. Identify API consumers (web, mobile, partners, internal services)
2. Define use cases and user stories
3. List required operations (CRUD, search, bulk operations)
4. Define performance requirements (latency, throughput)
5. Identify security and compliance needs

### Step 2: Protocol Selection
**REST API**: Best for resource-based operations, caching, wide compatibility
**GraphQL**: Best for flexible queries, reducing over-fetching, type safety
**gRPC**: Best for high-performance service-to-service, streaming, strong typing

### Step 3: Resource Design (REST)
1. Identify resources (nouns: users, orders, products)
2. Define URL structure: `/api/v1/resources/{id}`
3. Use HTTP methods correctly:
   - GET: Retrieve resources
   - POST: Create new resources
   - PUT: Full update
   - PATCH: Partial update
   - DELETE: Remove resources
4. Design relationships: `/users/{id}/orders`
5. Plan for pagination, filtering, sorting, field selection

### Step 4: Request/Response Schema
1. Define request body schemas (JSON, Protocol Buffers)
2. Define response formats
3. Use consistent naming conventions (camelCase, snake_case)
4. Include metadata (timestamps, IDs, versions)
5. Design error response format (RFC 7807 Problem Details)

### Step 5: Authentication & Authorization
1. Choose auth method: OAuth 2.0, JWT, API keys
2. Design token flow and refresh strategy
3. Implement RBAC or ABAC
4. Plan for API key rotation
5. Enable HTTPS/TLS

### Step 6: Rate Limiting & Throttling
1. Define rate limits per API key/user
2. Return rate limit headers (X-RateLimit-*)
3. Implement throttling for expensive operations
4. Design quota management

### Step 7: Versioning Strategy
1. Choose versioning approach (URL, header, query param)
2. Plan backward compatibility
3. Define deprecation policy and timeline
4. Communicate breaking changes to consumers

### Step 8: Error Handling
1. Use standard HTTP status codes
2. Return structured error responses
3. Include error codes, messages, and details
4. Provide troubleshooting guidance
5. Log errors for debugging

### Step 9: Documentation
1. Create OpenAPI 3.0 specification
2. Generate interactive docs (Swagger UI, Redoc)
3. Write integration guide with examples
4. Provide SDKs or code samples
5. Document authentication flows

## Output
1. API Specification (OpenAPI 3.0 YAML/JSON)
2. API Design Document with resource models
3. Authentication and security design
4. Rate limiting and versioning strategy
5. Interactive API documentation
6. Integration guide for consumers

## Quality Criteria
- RESTful principles followed (or GraphQL/gRPC best practices)
- Comprehensive error handling
- Secure by default
- Well documented with examples
- Versioned and backward compatible
- Performance requirements met
