# Knowledge Graphs

## Overview

Knowledge graphs are a structured representation of real-world entities, their properties, and the relationships between them. They combine the flexibility of graphs with semantic technologies to enable reasoning, inference, and knowledge discovery. Knowledge graphs power search engines, virtual assistants, recommendation systems, and enterprise data integration.

---

## Core Concepts

### What is a Knowledge Graph?

A knowledge graph is a graph-based data structure that:
- **Represents entities** as nodes (people, places, concepts, things)
- **Describes relationships** as edges (semantic connections)
- **Encodes meaning** through ontologies and schemas
- **Enables reasoning** to infer new knowledge
- **Integrates data** from multiple heterogeneous sources

### Key Characteristics

1. **Semantic Richness**: Relationships have meaning beyond simple connections
2. **Schema Flexibility**: Can evolve without breaking existing data
3. **Entity-Centric**: Organized around real-world entities
4. **Interconnected**: Dense network of meaningful relationships
5. **Queryable**: Support complex queries and traversals
6. **Machine-Readable**: Structured for automated reasoning

---

## Knowledge Graph vs Property Graph

| Aspect | Knowledge Graph | Property Graph |
|--------|-----------------|----------------|
| **Data Model** | RDF triples (subject-predicate-object) | Nodes with properties + edges |
| **Schema** | Ontology-driven (OWL, RDFS) | Flexible, optional schema |
| **Standards** | W3C standards (RDF, OWL, SPARQL) | Various (Cypher, Gremlin, etc.) |
| **Reasoning** | Native support (inference) | Limited, application-level |
| **Semantics** | Explicit via ontologies | Implicit in application logic |
| **Query Language** | SPARQL | Cypher, Gremlin, etc. |
| **Use Cases** | Knowledge management, semantic search | Fraud detection, recommendations |
| **Examples** | DBpedia, Wikidata, Google KG | Neo4j, Amazon Neptune (property) |

**When to Use Knowledge Graphs**:
- Need semantic reasoning and inference
- Integrating heterogeneous data sources
- Building enterprise knowledge bases
- Semantic search and question answering
- Compliance with semantic web standards

**When to Use Property Graphs**:
- Performance-critical graph traversals
- Pattern matching and path finding
- Real-time operational applications
- Simpler data model requirements
- Developer-friendly query languages

---

## RDF (Resource Description Framework)

### Triple Model

RDF represents data as **triples**: (Subject, Predicate, Object)

```turtle
# Example: Alice knows Bob
<http://example.org/person/Alice> <http://example.org/knows> <http://example.org/person/Bob> .

# With namespaces
@prefix ex: <http://example.org/> .
ex:Alice ex:knows ex:Bob .
```

**Components**:
- **Subject**: The entity being described (IRI or blank node)
- **Predicate**: The property or relationship (IRI)
- **Object**: The value or related entity (IRI, blank node, or literal)

### IRIs (Internationalized Resource Identifiers)

IRIs uniquely identify resources in RDF:
- `http://dbpedia.org/resource/Paris` - DBpedia entity for Paris
- `http://schema.org/Person` - Schema.org Person type
- `http://xmlns.com/foaf/0.1/knows` - FOAF knows relationship

### Literals

Literals represent values (strings, numbers, dates):

```turtle
ex:Alice ex:name "Alice Smith" .
ex:Alice ex:age 30 .
ex:Alice ex:birthDate "1994-03-15"^^xsd:date .
ex:Alice ex:email "alice@example.com"^^xsd:string .
```

### Blank Nodes

Anonymous nodes without IRIs:

```turtle
ex:Alice ex:address [
  ex:street "123 Main St" ;
  ex:city "Springfield" ;
  ex:zipCode "12345"
] .
```

---

## RDF Serialization Formats

### Turtle (Terse RDF Triple Language)

**Pros**: Human-readable, concise
**Use**: Manual editing, examples, documentation

```turtle
@prefix ex: <http://example.org/> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

ex:Alice a foaf:Person ;
    foaf:name "Alice Smith" ;
    foaf:age 30 ;
    foaf:knows ex:Bob .

ex:Bob a foaf:Person ;
    foaf:name "Bob Jones" .
```

### RDF/XML

**Pros**: XML-based, widely supported
**Use**: Legacy systems, XML tooling

```xml
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
         xmlns:ex="http://example.org/"
         xmlns:foaf="http://xmlns.com/foaf/0.1/">
  <foaf:Person rdf:about="http://example.org/Alice">
    <foaf:name>Alice Smith</foaf:name>
    <foaf:age>30</foaf:age>
    <foaf:knows rdf:resource="http://example.org/Bob"/>
  </foaf:Person>
</rdf:RDF>
```

### JSON-LD (JSON for Linked Data)

**Pros**: JSON-based, developer-friendly, web standards
**Use**: Web APIs, modern applications

```json
{
  "@context": "http://schema.org/",
  "@type": "Person",
  "@id": "http://example.org/Alice",
  "name": "Alice Smith",
  "age": 30,
  "knows": {
    "@type": "Person",
    "@id": "http://example.org/Bob",
    "name": "Bob Jones"
  }
}
```

### N-Triples

**Pros**: Simple, line-based, easy to parse
**Use**: Large datasets, streaming, ETL

```
<http://example.org/Alice> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://xmlns.com/foaf/0.1/Person> .
<http://example.org/Alice> <http://xmlns.com/foaf/0.1/name> "Alice Smith" .
<http://example.org/Alice> <http://xmlns.com/foaf/0.1/age> "30"^^<http://www.w3.org/2001/XMLSchema#integer> .
```

---

## Ontologies

### What is an Ontology?

An ontology formally defines:
- **Classes** (types of entities): Person, Organization, Event
- **Properties** (attributes and relationships): name, knows, worksFor
- **Constraints** (rules and axioms): domain, range, cardinality
- **Hierarchies** (inheritance): Employee subclass of Person

### RDFS (RDF Schema)

Basic vocabulary for defining RDF schemas:

```turtle
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix ex: <http://example.org/> .

# Define classes
ex:Person a rdfs:Class ;
    rdfs:label "Person" ;
    rdfs:comment "A human being" .

ex:Employee a rdfs:Class ;
    rdfs:subClassOf ex:Person ;
    rdfs:label "Employee" .

# Define properties
ex:knows a rdf:Property ;
    rdfs:domain ex:Person ;
    rdfs:range ex:Person ;
    rdfs:label "knows" .

ex:worksFor a rdf:Property ;
    rdfs:domain ex:Employee ;
    rdfs:range ex:Organization ;
    rdfs:label "works for" .
```

### OWL (Web Ontology Language)

More expressive than RDFS, enables reasoning:

```turtle
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix ex: <http://example.org/> .

# Class definitions with restrictions
ex:Person a owl:Class .

ex:Employee a owl:Class ;
    owl:equivalentClass [
        a owl:Restriction ;
        owl:onProperty ex:worksFor ;
        owl:someValuesFrom ex:Organization
    ] .

# Property characteristics
ex:knows a owl:SymmetricProperty .  # If A knows B, then B knows A
ex:hasChild a owl:AsymmetricProperty .  # Parent-child is not symmetric
ex:marriedTo a owl:FunctionalProperty .  # A person has at most one spouse

# Inverse properties
ex:hasEmployee owl:inverseOf ex:worksFor .

# Disjoint classes
ex:Person owl:disjointWith ex:Organization .
```

**OWL Features**:
- **Class hierarchy**: SubClassOf, equivalentClass
- **Property characteristics**: Functional, Symmetric, Transitive
- **Cardinality**: minCardinality, maxCardinality, exactCardinality
- **Logical operators**: unionOf, intersectionOf, complementOf
- **Reasoning**: Infer new triples based on axioms

---

## SPARQL Query Language

SPARQL is the standard query language for RDF data.

### Basic Queries

**Select specific properties**:
```sparql
PREFIX ex: <http://example.org/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?name ?age
WHERE {
  ex:Alice foaf:name ?name .
  ex:Alice foaf:age ?age .
}
```

**Find all people and their names**:
```sparql
SELECT ?person ?name
WHERE {
  ?person a foaf:Person .
  ?person foaf:name ?name .
}
```

### Pattern Matching

**Find friends of friends**:
```sparql
SELECT ?friend ?friendOfFriend
WHERE {
  ex:Alice foaf:knows ?friend .
  ?friend foaf:knows ?friendOfFriend .
  FILTER(?friendOfFriend != ex:Alice)
}
```

**Optional patterns**:
```sparql
SELECT ?person ?name ?email
WHERE {
  ?person a foaf:Person .
  ?person foaf:name ?name .
  OPTIONAL { ?person foaf:email ?email }
}
```

### Property Paths

**Transitive relationships** (friends of friends, any depth):
```sparql
SELECT ?person
WHERE {
  ex:Alice foaf:knows+ ?person .
}
```

**One or more hops**:
```sparql
# Find all supervisors (direct and indirect)
SELECT ?supervisor
WHERE {
  ex:Alice ex:reportsTo+ ?supervisor .
}
```

**Alternative paths**:
```sparql
# Person is connected via knows OR workedWith
SELECT ?connected
WHERE {
  ex:Alice (foaf:knows|ex:workedWith) ?connected .
}
```

### Aggregation

**Count, sum, average**:
```sparql
SELECT (COUNT(?person) AS ?personCount)
WHERE {
  ?person a foaf:Person .
}
```

**Group by**:
```sparql
SELECT ?organization (COUNT(?employee) AS ?empCount)
WHERE {
  ?employee ex:worksFor ?organization .
}
GROUP BY ?organization
ORDER BY DESC(?empCount)
```

### Federation

**Query multiple SPARQL endpoints**:
```sparql
SELECT ?person ?name ?dbpediaInfo
WHERE {
  # Query local data
  ?person a foaf:Person ;
          foaf:name ?name ;
          owl:sameAs ?dbpediaURI .

  # Query remote DBpedia endpoint
  SERVICE <http://dbpedia.org/sparql> {
    ?dbpediaURI rdfs:label ?dbpediaInfo .
    FILTER(LANG(?dbpediaInfo) = "en")
  }
}
```

---

## Popular Ontologies and Vocabularies

### FOAF (Friend of a Friend)

Describes people and social networks:

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

ex:Alice a foaf:Person ;
    foaf:name "Alice Smith" ;
    foaf:mbox <mailto:alice@example.com> ;
    foaf:knows ex:Bob ;
    foaf:homepage <http://alice.example.org> .
```

**Classes**: Person, Organization, Document, Project
**Properties**: name, knows, member, homepage, depiction

### Schema.org

Widely used for web content markup:

```json-ld
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Alice Smith",
  "jobTitle": "Software Engineer",
  "worksFor": {
    "@type": "Organization",
    "name": "Tech Corp"
  }
}
```

**Use Cases**: SEO, rich snippets, structured data
**Coverage**: People, places, events, products, creative works

### Dublin Core

Metadata for digital resources:

```turtle
@prefix dc: <http://purl.org/dc/elements/1.1/> .

ex:Document123 a foaf:Document ;
    dc:title "Knowledge Graphs Introduction" ;
    dc:creator "Alice Smith" ;
    dc:date "2025-01-15" ;
    dc:subject "Knowledge Graphs" .
```

**Properties**: title, creator, subject, description, publisher, date

### SKOS (Simple Knowledge Organization System)

Taxonomies, thesauri, classification schemes:

```turtle
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .

ex:DataScience a skos:Concept ;
    skos:prefLabel "Data Science"@en ;
    skos:altLabel "DS"@en ;
    skos:broader ex:ComputerScience ;
    skos:narrower ex:MachineLearning .
```

**Use Cases**: Taxonomies, controlled vocabularies, concept hierarchies

### PROV (Provenance)

Data lineage and provenance:

```turtle
@prefix prov: <http://www.w3.org/ns/prov#> .

ex:Dataset1 a prov:Entity ;
    prov:wasGeneratedBy ex:ETLJob123 ;
    prov:wasDerivedFrom ex:SourceData ;
    prov:wasAttributedTo ex:Alice .

ex:ETLJob123 a prov:Activity ;
    prov:startedAtTime "2025-01-15T10:00:00Z"^^xsd:dateTime ;
    prov:endedAtTime "2025-01-15T10:30:00Z"^^xsd:dateTime .
```

---

## Triple Stores (RDF Databases)

### 1. Amazon Neptune (RDF Mode)

**Features**:
- Managed RDF graph database
- SPARQL 1.1 support
- High availability (multi-AZ)
- Scales to billions of triples

**Use Cases**:
- Enterprise knowledge graphs
- Life sciences (drug discovery)
- Regulatory compliance
- Semantic data integration

**Example Connection**:
```python
from SPARQLWrapper import SPARQLWrapper, JSON

sparql = SPARQLWrapper("https://your-neptune-endpoint:8182/sparql")
sparql.setQuery("""
    PREFIX foaf: <http://xmlns.com/foaf/0.1/>
    SELECT ?name WHERE {
        ?person a foaf:Person .
        ?person foaf:name ?name .
    }
""")
sparql.setReturnFormat(JSON)
results = sparql.query().convert()
```

### 2. GraphDB (Ontotext)

**Features**:
- High-performance RDF store
- OWL reasoning support
- Full-text search
- Visual graph explorer

**Editions**:
- Free: Up to 100M triples
- Standard: Unlimited triples
- Enterprise: Clustering, replication

**Use Cases**:
- Publishing and media
- Life sciences research
- Enterprise data catalogs

### 3. Stardog

**Features**:
- ACID transactions
- SQL over RDF (Stardog Virtual Graphs)
- Reasoning and inference
- Machine learning integration

**Use Cases**:
- Enterprise knowledge graphs
- Data virtualization
- Financial services
- Healthcare

### 4. Apache Jena

**Features**:
- Open-source RDF framework
- TDB (native RDF store)
- SPARQL engine (ARQ)
- Fuseki (SPARQL server)

**Deployment**:
- Embedded in Java applications
- Standalone server (Fuseki)
- Scalable with TDB2

### 5. Virtuoso

**Features**:
- Multi-model (RDF + SQL)
- High performance (billions of triples)
- Linked data platform
- Full-text search

**Use Cases**:
- DBpedia (powers Wikipedia structured data)
- Linked open data
- Semantic web applications

---

## Building a Knowledge Graph

### Step 1: Define the Domain

**Identify**:
- Key entities (Person, Product, Organization, Event)
- Important relationships (knows, purchased, employs, occurred)
- Attributes to capture (names, dates, descriptions)

**Questions to Answer**:
- What business questions need answering?
- What data sources exist?
- What are the use cases?

### Step 2: Design the Ontology

**Create classes**:
```turtle
ex:Person a owl:Class .
ex:Product a owl:Class .
ex:Organization a owl:Class .
```

**Define properties**:
```turtle
ex:knows a owl:ObjectProperty ;
    rdfs:domain ex:Person ;
    rdfs:range ex:Person .

ex:worksFor a owl:ObjectProperty ;
    rdfs:domain ex:Person ;
    rdfs:range ex:Organization .

ex:purchased a owl:ObjectProperty ;
    rdfs:domain ex:Person ;
    rdfs:range ex:Product .
```

**Add constraints**:
```turtle
ex:age a owl:DatatypeProperty ;
    rdfs:domain ex:Person ;
    rdfs:range xsd:integer ;
    owl:minInclusive 0 ;
    owl:maxInclusive 150 .
```

### Step 3: Extract and Transform Data

**From Relational Database**:
```python
# Pseudo-code for RDB to RDF transformation
for person in database.query("SELECT * FROM persons"):
    person_uri = f"ex:person/{person.id}"
    add_triple(person_uri, "rdf:type", "ex:Person")
    add_triple(person_uri, "ex:name", person.name)
    add_triple(person_uri, "ex:age", person.age)

    if person.employer_id:
        org_uri = f"ex:org/{person.employer_id}"
        add_triple(person_uri, "ex:worksFor", org_uri)
```

**From JSON API**:
```python
import json
from rdflib import Graph, Namespace, Literal, URIRef

g = Graph()
EX = Namespace("http://example.org/")

data = json.loads(api_response)
person_uri = EX[f"person/{data['id']}"]
g.add((person_uri, RDF.type, EX.Person))
g.add((person_uri, EX.name, Literal(data['name'])))
```

### Step 4: Link Entities

**Entity Resolution** - Match entities across sources:
- String similarity (Levenshtein distance)
- Phonetic matching (Soundex)
- Probabilistic matching
- Machine learning classifiers

**Linking**:
```turtle
ex:Person123 owl:sameAs dbr:Alice_Smith .
ex:Person123 owl:sameAs wikidata:Q12345 .
```

### Step 5: Enrich with External Data

**Link to public knowledge graphs**:
- DBpedia: Structured Wikipedia data
- Wikidata: Collaborative knowledge base
- Schema.org: Web markup vocabulary
- GeoNames: Geographic entities

**Example**:
```turtle
ex:Paris owl:sameAs dbr:Paris .
ex:Paris owl:sameAs <http://sws.geonames.org/2988507/> .
ex:Paris owl:sameAs wd:Q90 .  # Wikidata Paris
```

### Step 6: Reasoning and Inference

**Infer new knowledge**:
```turtle
# Ontology: worksFor is transitive through subsidiaries
ex:Alice ex:worksFor ex:SubsidiaryA .
ex:SubsidiaryA ex:ownedBy ex:ParentCorp .

# Reasoning infers:
# ex:Alice ex:employedBy ex:ParentCorp .
```

**SPARQL inference**:
```sparql
# Find implicit friends (friends of friends who share interests)
SELECT ?person ?potentialFriend
WHERE {
  ?person foaf:knows ?friend .
  ?friend foaf:knows ?potentialFriend .
  ?person ex:hasInterest ?interest .
  ?potentialFriend ex:hasInterest ?interest .
  FILTER(?person != ?potentialFriend)
}
```

### Step 7: Validation and Quality

**SHACL (Shapes Constraint Language)**:
```turtle
ex:PersonShape a sh:NodeShape ;
    sh:targetClass ex:Person ;
    sh:property [
        sh:path ex:name ;
        sh:datatype xsd:string ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path ex:age ;
        sh:datatype xsd:integer ;
        sh:minInclusive 0 ;
    ] .
```

**Quality Checks**:
- Completeness (required properties present)
- Consistency (no contradictions)
- Accuracy (correct values)
- Duplication (entity resolution)

---

## Use Cases

### 1. Enterprise Knowledge Management

**Problem**: Siloed data across departments
**Solution**: Unified knowledge graph

**Example**:
- Integrate customer data (CRM)
- Product information (PIM)
- Transaction history (ERP)
- Support tickets (Helpdesk)
- Employee directory (HR)

**Queries**:
- "Who are the customers of Product X in Region Y?"
- "Which support engineer has experience with similar issues?"
- "What is the complete history of Customer Z?"

### 2. Semantic Search and Question Answering

**Problem**: Keyword search misses semantic meaning
**Solution**: Knowledge graph-powered search

**Example - Google Knowledge Graph**:
```
Query: "Who is the CEO of Apple?"
KG: Apple -> hasExecutive -> Tim Cook
     Tim Cook -> hasRole -> CEO
Answer: "Tim Cook"
```

**Benefits**:
- Understand intent, not just keywords
- Provide direct answers
- Surface related entities
- Enable conversational AI

### 3. Drug Discovery and Life Sciences

**Problem**: Complex relationships between genes, proteins, diseases
**Solution**: Biomedical knowledge graph

**Example**:
```turtle
ex:Gene_BRCA1 ex:associatedWith ex:Disease_BreastCancer .
ex:Drug_Tamoxifen ex:treats ex:Disease_BreastCancer .
ex:Drug_Tamoxifen ex:inhibits ex:Protein_EstrogenReceptor .
```

**Queries**:
- "Which drugs target proteins associated with this disease?"
- "What are the side effects of drugs in this pathway?"
- "Are there drug repurposing opportunities?"

### 4. Financial Compliance and Risk

**Problem**: Complex ownership structures, regulatory requirements
**Solution**: Financial knowledge graph

**Example**:
```turtle
ex:PersonA ex:beneficialOwnerOf ex:CompanyB .
ex:CompanyB ex:subsidiary ex:CompanyC .
ex:CompanyC ex:hasTransaction ex:Transaction123 .
ex:Transaction123 ex:amount 1000000 ;
                  ex:suspiciousActivity true .
```

**Use Cases**:
- Anti-money laundering (AML)
- Know Your Customer (KYC)
- Beneficial ownership tracking
- Sanctions screening

### 5. Content Recommendation

**Problem**: Shallow collaborative filtering
**Solution**: Rich semantic relationships

**Example**:
```turtle
ex:UserAlice ex:watched ex:Movie_Inception .
ex:Movie_Inception ex:directedBy ex:ChristopherNolan .
ex:Movie_Inception ex:hasGenre ex:SciFi .
ex:Movie_Interstellar ex:directedBy ex:ChristopherNolan .
ex:Movie_Interstellar ex:hasGenre ex:SciFi .
```

**Recommendations**:
- "Because you liked movies directed by Christopher Nolan"
- "Other sci-fi movies with similar themes"
- "Actors who appeared in similar films"

---

## Tools and Frameworks

### RDFLib (Python)

```python
from rdflib import Graph, Namespace, Literal, URIRef
from rdflib.namespace import RDF, FOAF

g = Graph()
EX = Namespace("http://example.org/")

# Add triples
alice = EX.Alice
g.add((alice, RDF.type, FOAF.Person))
g.add((alice, FOAF.name, Literal("Alice Smith")))
g.add((alice, FOAF.age, Literal(30)))

# Query
for person in g.subjects(RDF.type, FOAF.Person):
    print(f"Person: {person}")

# Serialize
print(g.serialize(format="turtle"))
```

### Apache Jena (Java)

```java
Model model = ModelFactory.createDefaultModel();
String ex = "http://example.org/";

Resource alice = model.createResource(ex + "Alice");
alice.addProperty(RDF.type, FOAF.Person);
alice.addProperty(FOAF.name, "Alice Smith");
alice.addProperty(FOAF.age, model.createTypedLiteral(30));

// Query
String queryString = "SELECT ?name WHERE { ?person foaf:name ?name }";
Query query = QueryFactory.create(queryString);
QueryExecution qe = QueryExecutionFactory.create(query, model);
ResultSet results = qe.execSelect();
```

### Owlready2 (Python OWL)

```python
from owlready2 import *

onto = get_ontology("http://example.org/onto.owl")

class Person(Thing):
    namespace = onto

class knows(ObjectProperty):
    domain = [Person]
    range = [Person]

alice = Person("Alice")
bob = Person("Bob")
alice.knows.append(bob)

onto.save(file="knowledge_graph.owl")
```

---

## Best Practices

### Ontology Design

1. **Reuse existing ontologies** (Schema.org, FOAF, Dublin Core)
2. **Keep it simple** - start minimal, extend as needed
3. **Use meaningful IRIs** - human-readable, stable
4. **Document thoroughly** - rdfs:label, rdfs:comment
5. **Version your ontology** - owl:versionInfo

### Data Quality

1. **Validate with SHACL** - enforce constraints
2. **Entity resolution** - deduplicate entities
3. **Link to external sources** - owl:sameAs links
4. **Provenance tracking** - PROV vocabulary
5. **Regular audits** - completeness, consistency checks

### Performance

1. **Index frequently queried properties**
2. **Materialize inferred triples** - avoid runtime reasoning overhead
3. **Partition large graphs** - by domain or time
4. **Use property paths carefully** - can be expensive
5. **Cache common queries**

### Security

1. **Access control** - query-level permissions
2. **Data privacy** - PII handling, GDPR compliance
3. **Audit logging** - track all access
4. **Encryption** - at rest and in transit

---

## Resources

### Standards
- [RDF 1.1 Primer](https://www.w3.org/TR/rdf11-primer/)
- [SPARQL 1.1 Query Language](https://www.w3.org/TR/sparql11-query/)
- [OWL 2 Web Ontology Language](https://www.w3.org/TR/owl2-overview/)
- [SHACL Shapes Constraint Language](https://www.w3.org/TR/shacl/)

### Tutorials
- [RDFLib Documentation](https://rdflib.readthedocs.io/)
- [Apache Jena Tutorial](https://jena.apache.org/tutorials/)
- [SPARQL by Example](https://www.w3.org/2009/Talks/0615-qbe/)

### Public Knowledge Graphs
- [DBpedia](https://www.dbpedia.org/) - Structured Wikipedia
- [Wikidata](https://www.wikidata.org/) - Collaborative knowledge base
- [Google Knowledge Graph](https://developers.google.com/knowledge-graph)
- [Schema.org](https://schema.org/) - Web markup schemas

### Books
- "Knowledge Graphs" by Hogan et al.
- "Semantic Web for the Working Ontologist" by Allemang & Hendler
- "Linked Data: Evolving the Web into a Global Data Space" by Heath & Bizer
