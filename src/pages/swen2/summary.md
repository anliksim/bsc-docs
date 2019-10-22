---
path: "/swen2/summary"
title: "SWEN2 Summary"
date: "2019-01-20"
draft: false
author: Simon Anliker
tags:
  - swen2
---

Software Engineering 2 course summary.

## Agile and XP

### Traditional methodologies

* Waterfall: Requirements, Design, Implementation, Verification, Maintenance
* Unified Process: Inception, Elaboration, Construction, Transition

### Agile Manifesto

* Individuals and interactions over processes and tools
* Working software over comprehensive documentation
* Customer collaboration over contract negotiation
* Responding to change over following a plan

Used in: Extreme programming (XP), SCRUM, DSDM

#### Principles

* Highest priority: Satisfy the customer
* Welcome changing requirements
* Deliver working software frequently
* Business and dev work together daily
* Trust in people
* Face-to-face conversations
* Working software as the measure of progress
* Simplicity, technical excellence
* Self-organizing teams
* Reflect regularly

### eXtreme Programming (XP)

Developing software:
* 4 variables (controlled by managers, customers): Time, Resource, Quality (controlled by devs only), Scope
* 6 values:
  * Communication: work together as a team, face-to-face
  * Simplicity: small steps, maximize value
  * Feedback: commit to delivery, demo work, adapt process
  * Courage: tell the truth, adapt to change, don't document excuses for failure
  * Respect: value team members, everyone contributes, dev-mgmt acceptance

#### Principles

(x) Fundamental

* Rapid feedback (x)
* Assume simplicity (x)
* Incremental change (x)
* Embracing change (x)
* Quality work (x)
* Teach learning
* Small initial investment
* Play to win
* Concrete experiments
* Open, honest communication
* Work with people's insticts, not against them
* Accepted responsibility
* Local adaption
* Travel light
* Honest measurement

#### Practices

* The Planning Game: Balance business and technical considerations
* Small releases: Small as possible with most valuable business requirements
* Metaphor: Common understanding of system, shared vocabulariy
* Simple design: Emergent, growing design, small, no duplication, tested
* Testing: Automated, help accept change
* Refactoring: Improve for upcoming change, tests as safety-net
* Pair programming: Two people - one screen, two roles, pairs change frequently
* Collective ownership: Everybody takes responsibility and opportunity to add value
* Continuous integration: Automated build proces, integrate and test at least once a day
* 40 hours week: Sustainable development, more productive - fresh and satisfied during work
* On-site customer: Physically with the team, available for questions
* Coding standards: Unified practices to allow collective ownership and constant refactoring

V2:
* Primary practices: Sit together, whole team, informative workspace, energized work, pair programming, stories, weekly cycle, quarterly cycle, slack (time), ten-minute build, continuous integration, test-first programming, incremental design
* Colloary practices: Real customer involvement, incremental deployment, team continuity, shrinking teams, root-cause analysis, shared code, code and tests, single code base, daily deployment, negotiated scope contract, pay-per-use


No silver bullet: There is no magic process - agile describes core values, team need to improve through reflection.


## User Stories

## Estimation and Planning

## Version Control

## Build Autonmation and CI

## Scrum

Project noise level: (Requirements vs Technology)
* Simple (Close to agreement vs certainty)
* Complicated
* Complex
* Anarchy (Far from agreement vs certainty)

Process control theory: (noice catergory indicates process)
* Defined: simple, unobstrusive noice, repeatable (almost never applicable to software!)
* Empirical: complex, noisy processes, not repeatable (managed through frequent inspection and adaptive control)

### Roles

* Product owner: Define features, decide on release dates and content, ROI, prioritize, adjust, accept results
* Srum master: enforce value and practices, remove impediments, enable cooperation, ensure productivity, shield the team
* Development team: 5-9 people, cross-functional, full-time, self-organizing (no titles), change only between sprints

### Ceremonies

* Sprint: 2-4 week iteration
* Sprint planning:
  * Sprint prioritization: analyze/eval product backlog, select sprint goal
  * Spring planning: design, create sprint backlog, estimate
* Sprint review: demo work, informal (no slides, 2-hour prep time rule), whole team, invite the world
* Sprint retrospective: 15-30mins, after every sprint, whole team, what is and what is not working (what to start/stop/vontinue doing)
* Daily scrum meeting: Daily, 15min, stand-up, not for problem solving, whole world invited - only scrum team talks, commitments to peers but not statuses for the scrum master
  * What did you do yesterday?
  * What will you do today?
  * Is anything in your way?

### Artifacts

* Product backlog: requirements, items with value to customer of product, managed by product owner
* Spring backlog: Sprint works, team members pick - add, delete or change
* Burndown chart: indicator for how well sprint is progressing

Task board:
* Not checked out: Nobody working on today
* Checked out: Work in progress today
* Done: Not worked on anymore 

### Definitions

Definition of Done (DoD):
* Agreed by the scrum team
* Evolves sprint by sprint
* e.g.:
  * Unit tests pass, sufficient negative tests, coverage
  * Code is reviewed
  * Coding standards are met
  * Continuous integration implemented
  * Code is refactored
  * UAT tests pass
  * Non-functional tests pass
  * Necessary documentaiton is completed

on sprint level: all stories met DoD, product increment was accepted by product owner

Definition of Ready:
* User story defined
* Acceptance criteria defined
* Dependencies identified
* Story sized by delivery team
* Scrum team accepts user experience artefacts
* Performance criteria identified
* Sign-off/accept person identified
* Team knows what it means to demo the story

on sprint level: backlog prioritized, no hidden work, user stories are "ready", capacity calculated


Self-organization:
* Select work to complete
* Determine how to meet requirements
* Get help with impediments
* Select own scrum master

### Kotter's 8-Step Change Model

1. Establish a sense of urgency
2. Create a guiding coalition
3. Develop a vision and strategy
4. Communicate and share the vision
5. Create and agile implementaiton backlog
6. Empower broad-based action
7. Generate short-term wins
8. Anchor the new approaches into the culture

### Anti-Patterns

* Poor use of retrospectives: Rant sessions without commitments
* Reverting to bad behavior: Excuses for ignoring principles
* The hero developer: One who is needed to get some things done and almost always ignores principles
* Absent product owner (APOP): No product owner around creates wait time and waste and decisions made by non-PO's


## Architecture




## JPA

### O/R-Mismatch

Conceptional differences of underlying technologies.

* Types (null, Date/Time)
* Relations (Direction, multi-relations)
* Inheritance
* Identity (Objects - implicitly, Primary Key - explicitly)
* Transactions

### O/R-Mapping

Solution for O/R-Mismatch.
Java Standards: EJB2, JDO, JPA - Java Persistence API (Hibernate, OpenJPA, Toplink)

* Decloupling DB from application: no SQL needed
* Automated persistence: abstraction of low-level details
* Transparent persistence / persistence ignorance: no dependency on persistence infrastructure, domain model does not know that it will be persisted 
* Abstraction: handle complexity
* Separation of concerns: decouple business logic from infrastructure concerns

#### Problems

* Location transparency: data has to be always available
* SQL performance/optimization: dynamic, generated SQL is well (enough) optimized
* Queries: trade-off as not every concepts is available in OO (e.g. no cartesian products, subset of attributes)

### API

* Annotaitons: Entity, Id, OneToOne, OneToManyi,...
* Interfaces: EntityManager, EntityManagerFactory, EntityTransaction, Query
* Enumerations: InheritanceType, CascaseType, FetchType,...
* Classes: Persistence

Concepts:
* Entity: Annotates java object (@Entity), has primary key (@Id), used transcationally
* Persistence unit: Configuration for entity mapping with DB (persistence.xml)
* Entiy manager: API to manage entity instance lifecycle i.e. persist, remove, refresh, merge, find, create query, get transaction, flush, clear, close
* Persistence context: set of entites managed by the Entity manager
* Transactions

In code terms: 
* Persistence creates EntityManagerFactory
* PersistenceiUnit configures EntityManagerFactory
* EntityManagerFactory creates EntityManager
* EntityManager manages PersistenceContext

Entity lifecycle status:
* New (not yet managed or persistend)
* Managed (on persist, persisted in DB on transcation commit)
* Detached (not on peristence context anymore, need to be mergedi back into managed entity)
* Removed (deleted from DB on transaction commit)

Access types:
* Field access (@Access(FILED))
* Property access (@Access(PROPERTY)): through getters
 
Persisted datatpyes:
* Primitives
* Strings
* Wrapper classes
* Serializable types (e.g. Date, Timestamp, Calendar)
* Byte and char arrays
* Enumerations
* Entity-Classes
* Collections of Entity-Classes
* No arrays
* No Collections of non-Entity-Classes

Fetching types:
* Lazy (FetchType.LAZY), default on one-to-many, many-to-many
* Lazy large objects (+@Lob)
* Eager (FetchType.EAGER), default on one-to-one, many-to-one

Enumareation variants:
* Ordinal (EnumType.ORDINAL)
* String (EnumType.STRING)

Temporal type:
* java.sql (no types needed)
* java.util: TemporalType.DATE/TIME/TIMESTAMP

Primary key:
* All allowed datatypes (@Id)
* Floating point types allowed (beware of rounding errors!)
* Generation strategies: Identity, Table, Sequence, Auto

Mappings:
* One-to-one (1:1, @OneToOne)
* Many-to-one (n:1, @ManyToOne, field is type)
* One-to-many (1:n, @OneToMany, field is Set, with @JoinColumn or 'mappedBy')
* Many-to-many (n:m, @ManyToMany, field is Set, with 'mappedBy')

Cascade type:
* Propagate persistence on assoziated entities: ALL, PERSIST, MERGE, REMOVE, REFRESH, DETACH
* CasadeType.ALL: Persisting parent also persists children

Skip serialization / exclude from transfer with @Transient (or transient)

### Querying

* JPQL (Java Persistence Query Language)
* Criteria API

## REST

Layers of an information system:
* Presentation layer: communication interface to external entities
* Application logic layer: operations requested through presentaiton layer
* Resource management layer: deals with data sources

Distributed system types:
* Request/response or call and return
  * Call oriented
  * Synchronous in nature
  * Input parameters -> output values
  * Focus on invoked opetations with in/out values
* Message passing or document passing
  * Data oriented
  * Asynchronous
  * Messages constructed and sent
  * Focus on message construction and dispatching
  * No focus on what happends after dispatching

Architectural style:
* Client/Server
* Component-based
* Domain driven design
* Layered
* Message-bus
* N-Tier / 3-Tier
* for call-based systems
  * Object-oriented: stateful communication with object instances
  * Resource-oriented: stateless requests / operations for resource management
  * Service-oriented: stateless communication via self-described service endpoints

Webservice: A web service is a software system designed to support interoperable machine-to-machine interaction over a network (with an interface describe in a machine-processable format).
* SOAP (former Simple Object Access Protocol) / WSDL (Web Service Description Language)
  * Interface through WSDL
  * Message exchange using SOAP
  * Using UDDI (Universal Description Discovery and Integration)
* REST (Representational State Transfer)
  * Resource identitief by URI's
  * Manipulations through HTTP operations
  * Set of architectural principles
  * Six constraints
     * Uniform interface: through HTTP methods
     * Stateless: does not hold client state
     * Cacheable: responses are cacheable
     * Cient-Server: disconnected system
     * Layered: client can't assume direct connection
     * Code on demand (optional): execute logic on client
  * Standard HTTP error codes

SOA (Service Oriented Architectures)
* Abstract pattern for webservices
* Loosely-coupled architecture
* Shared, reusable services
* Publish, Find, Bind

JAX-RS: Java API for RESTful Web Services


## SOLID

* KISS: Keep It Simple and Stupid
* SOLID:
    * Single Responsibility Principle (SRP): each module/class has only one responsibility
    * Open Close Principle (OCP): Open for extension, closed for change
    * Liskov Substitution Principle (LSP): replacing a class with another one derived from it should not change behaviour (if it does do not use inheritance)
    * Interface Segregation Principle (ISP): only depend on interfaces you actually use
    * Dependency Inversion Principle (DIP): interfaces over concrete classes
* DRY: Don't Repeat Yourself


