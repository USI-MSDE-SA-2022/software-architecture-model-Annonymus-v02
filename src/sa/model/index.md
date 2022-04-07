title:
Software Architecture Model of Tag4You by Marco Tereh
---

# Getting started

You will use [Markdown](https://www.markdownguide.org/cheat-sheetplan) and [PlantUML](https://plantuml.com/) to describe a software architecture model about your own project.

This document will grow during the semester as you sketch and refine your software architecture model.

When you are done with each task, please push so we can give you feedback about your work.

We begin by selecting a suitable project domain.



# Ex - Domain Selection

{.instructions

Submit the name and brief description (about 100 words) of your domain using the following vision statement template:

```
For [target customers]
Who [need/opportunity/problem]
The [name your project]
Is  [type of project]
That [major features, core benefits, compelling reason to buy]
Unlike [current reality or competitors]
Our Project [summarize main advantages over status quo, unique selling point]
```

Please indicate if your choice is:

* a project you have worked on in the past (by yourself or with a team)
* a project you are going to work on this semester in another lecture (which one?)
* a new project you plan to build in the future
* some existing open source project you are interested to contribute to

The chosen domain should be unique for each student.

Please be ready to give a 2 minute presentation about it (you can use a slide but it's not necessary)

Hint: to choose a meaningful project look at the rest of the modeling tasks which you are going to perform in the context of your domain.

}

Project Name: Tag4You

Project Type: Potential future project

Vision Statement:

Companies whose service is to offer a large number of individual items, such as amazon and netflix, require a way for users to find the item(s) they want or need among the many on offer. One way for customers to do so is using tags: products are tagged with specific properties that a user can be interested in. For example: netflix tags some movies as "romantic". Users interested in romantic movies can search for that and get a list of movies they're interested in. Unfortunately, these systems are often sorely lacking and provide far too little granularity. For example, Netflix only provides a few dozen tags and there is no way to search for a movie with "female protagonist" or "gay love" or "urban fantasy". Furthermore, most companies only offer positive search and do not allow customers to *exclude* items with certain attributes. Tag4you offers a simple and rapid interface to accomplish all of that. Let us imagine a world in which instead of writing "fantasy movie" into netflix's search bar and being presented with a list of sci-fi TV series, I can write "high fantasy, strong female protagonist, no romance, good music, score > 8, length < 120" and get a movie I actually *want* to watch instead of the one I have to.

Additional Information:
The advantage of using Tag4You instead of developing the system in-house is clear: not only do you save on development costs and time, the end product is also more stable, feature-rich and provides a uniform query language which users are used to from other companies using our services.


# Ex - Architectural Decision Records

{.instructions

Software architecture is about making design decisions that will impact the quality of the software you plan to build.

Let's practice how to describe an architectural decision. We will keep using ADRs to document architectural decisions in the rest of the model.

Use the following template to capture one or more architectural design decisions in the context of your project domain

Pass: 1 ADR

Good: 2 ADR

Exceed: >2 ADR

}

![Architectural Decision Record Template](./examples/decision-template.madr)

1. What did you decide?

## Programming language: C++

2. What was the context for your decision?

A key characteristic for a tagging system is performance. Users expect low-latency answers to their queries. Moreover, the operation is in general rather expensive and many similar systems in the wild impose constraints on their users due to suboptimal performance, such as only allowing a limited number of tags per query. For that reason, the system must be built with performance in mind from the beginning. That starts with the choice of programming language. We choose C++ due to its high runtime performance. This choice will affect the entire development process.

3. What is the problem you are trying to solve?

How can we increase run-time performance?

4.  Which alternative options did you consider?

Java, JavaScript, C++, C, Ruby

5. Which one did you choose?

C++

6. What is the main reason for that?

Java and JavaScript are both popular languages for building web applications, but they are both significantly less performant than C++. C has similar performance as C++, but less powerful language features and libraries, giving us less flexibility later in the process. Ruby is also in use for web development, but our programmers (i.e. me) do not have much experience with it, leading to a greater chance of bugs and increased development time due to the requirement of learning the language. Performance impact of Ruby is unknown but very unlikely to be greater than C++.

C++ as a language is said to be harder to learn and develop in than other languages, however, our programmers (me) have experience with it already and should not find much trouble using it.

1. What did you decide?

## API: RESTful

2. What was the context for your decision?

Our goal for commercialisation is to give access to this service to other entities, which requires ease of interface. The easier it is to interface with our system, the greater the likelihood that a potential customer will choose to employ our services. A REST API will make it easy for customers to connect to our systems. Most of the process will be unaffected by this decision - the core system still remains the same, only the layer which interfaces with the network is constrained by the type of API to provide.

3. What is the problem you are trying to solve?

How can we make it as simple as possible for clients to interface with us?

4.  Which alternative options did you consider?

REST API, custom protocol, both

5. Which one did you choose?

REST API

6. What is the main reason for that?

REST APIs are commonplace and as such many programmers know well how to use them and many platforms have components, libraries or frameworks which allow interfacing with such an API easily. Particularly so in the space of web applications, which are always at least in part written in JavaScript, which is capable of interfacing with REST APIs natively. A custom API would allow us more flexibility, but come with a higher learning curve and implementation cost for clients. Some clients might find it easier to send and receive simpler messages instead of having to package everything in HTTP requests, but they are outnumbered.
Implementing both and allowing connections to either would maximise the affordability for our clients, but increase the size and cost of the system, thus we choose against it. However, it is possible to add the second API later on, without significantly increased costs, so we choose to keep the option open and not perform any action that would permanently rule out the possibility of implementing a custom API, so that we may opt to do so if it appears that we still have some resources left after all essential components have been implemented, or as a way to grow after launch if the product is successful enough.

1. What did you decide?

## Commercialisation: software as a service

2. What was the context for your decision?

In creating this system we must consider how to make it profitable, or at least not unprofitable.
Thus it is necessary to consider how we sell it. We choose a software as a service model as that allows us to profit continuously from every client. This requires us to expend additional resources to deploy and maintain the system, but gives us greater control of the platform we are deployed on, decreasing the requirements on portability.

3. What is the problem you are trying to solve?

How do we make money from this?

4.  Which alternative options did you consider?

one-time purchase of software package, software as a service

5. Which one did you choose?

software as a service

6. What is the main reason for that?

In the software as a service model, every client is a continuous revenue stream, allowing us to profit continuously as long as they keep using our service. It also increases attractivity by decreasing the burden on the clients for deploying the system, though at the cost of an increased expense. We have determined that the pros should outweigh the cons for our clients, as the kind of system which can benefit from a tagging system is one which contains large a volume of content and therefore must already be at a minimum scale, implying high availability of resources for the client and therefore greater affordibility of the cost of our service. The SaaS model also increases affordability for a client who is not sure whether they really need our service in the long term, or has limited initial investment, as it comes with a reduced up-front cost.
This model will require us to maintain our own servers to run the system on, but with an appropriate pricing strategy we can ensure the cost of that is outweighed by the increased profits. Furthermore, full control of the platform on which we deploy reduces the burden of ensuring portability of our system, reducing the amount of work that is necessary before we become profitable.

Deploying our software on a separate server from our clients' machines comes with the cost of increasing latency between queries and responses, but modern network speeds reduce it to an acceptable amount and using dedicated hardware counteracts this effect by avoiding increased processing times due to suboptimal hardware configurations or systems overloaded by other software running on the same machine.

# Ex - Quality Attribute Scenario

{.instructions

1. Pick a scenario for a specific quality attribute. Describe it with natural language.

2. Refine the scenario using the following structure:

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle Environment {

[Source] -> [System] : Stimulus

[System] -> [Measure] : Response

}

@enduml
```

*Stimulus*: condition affecting the system

*Source*: entity generating the stimulus

*Environment*: context under which stimulus occurred (e.g., build, test, deployment, startup, normal operation, overload, failure, attack, change)

*Response*: observable result of the stimulus

*Measure*: benchmark or target value defining a successful response

Pass: 3 scenarios

Good: >3 scenarios

Exceed: >6 scenarios using challenging qualities

}

## Maximum Latency

Quality: _Performance (Latency)_

Scenario: Answers to client requests should be transmitted no more than 100ms after reception of the query

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "During normal operation" {

rectangle "User" as Source
rectangle "max 0.1s" as Measure

Source -> [System] : "Query"

[System] -> [Measure] : "Reply"

}

@enduml
```

Note that precise numbers in this and other scenarios are based on a very basic understanding on my part of the possibilities involved

## Mutation Visibility

Quality: _Correctness_

Scenario: Changes to tags may take up to 60 min. for full propagation

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "During normal operation" {

rectangle "User" as Source
rectangle "All changes older than 1h visible" as Measure

Source -> [System] : "Query"

[System] -> [Measure] : "Reply"

}

@enduml
```

Responsiveness to write queries is not a high priority and may be sacrificed in exchange for performance of read queries or scalability.

{.feedback

This type of Correctness is also known as Read-Write Consistency.

}

## Influence on client data

Quality: _Ethics_

Scenario: Weight of tags and order of result must be based entirely on objective properties of the data (such as creation date) or user-defined criteria. The system may not influence, algorithmically or by admin intervention, the results of a query.

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "During normal operation" {

rectangle "Query" as Source
rectangle "Transparent and objective criteria" as Measure

Source -> [System] : "Result"

[System] -> [Measure] : "Order"

}

@enduml
```

"The algorithm" must not influence results beyond the obvious or explicitly requested manner.

## Intuitiveness of query language

Quality: _Usability_

Scenario: User should succeed in submitting the query they have in mind in less than 3 attempts.

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "During normal operation" {

rectangle "User" as Source
rectangle "< 1 rewrite on avg." as Measure

Source -> [System] : "Query"

[System] -> [Measure] : "Desired effect"

}

@enduml
```

The query language must be learnable quickly

## Failure Mode

Quality: _Safety_

Scenario: System should not return incorrect or incomplete results

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "During a system failure" {

rectangle "User" as Source
rectangle "Error message" as Measure

Source -> [System] : "Query"

[System] -> [Measure] : "Reply"

}

@enduml
```

Failures must be transparent - if the user believes an action succeeded, it must truly be so.

## Attack recovery

Quality: _Recoverability_

Scenario: Following a successful attack it should be possible to restore data to a known good state and resume operation within 1 day

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "After malicious attack occurred" {

rectangle "Admin" as Source
rectangle "max 24h" as Measure

Source -> [System] : "Restore"

[System] -> [Measure] : "Working Order"

}

@enduml
```

Intrusion must be detected and defeated within a day. Furthermore, logs and backups must be kept to allow rapid restoration of the system to a state it was in before the attacker had access to it.

## DoS attack resilience

Quality: _Availability_

Scenario: System should be able to withstand a targeted denial of service attacks from at least 1000 unique IPs per minute without interrupting operation for regular users

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "During normal operation" {

rectangle "Attackers" as Source
rectangle "< 1000/m" as Measure

Source -> [System] : "Flood with requests"

[System] -> [Measure] : "Keep serving regular users"

}

@enduml
```

Increased response times and aggressive throttling are unavoidable, but users must not be prevented from making queries completely for attacks below a certain scale.
(Actually I have no idea what scale of attack is realistic, 1000IP/m is a very rough guesstimate)

## User logs

Quality: _Defensibility_

Scenario: System should keep logs of all users and respective write queries.

```puml
@startuml

skinparam componentStyle rectangle
skinparam monochrome true
skinparam shadowing false

rectangle "During normal operation" {

rectangle "User" as Source
rectangle "Logs exist" as Measure

Source -> [System] : "Perform data modification"

[System] -> [Measure]

}

@enduml
```

Activity logs help in detection of and recovery from attacks and credential leaks both for our service as a whole and for our clients' individual databases

{.feedback

+ This may also be called Auditability.
+ The Event Sourcing pattern helps to deliver it.

}

# Ex - Quality Attribute Tradeoff

{.instructions

Pick a free combination of two qualities on the [map](https://usi365.sharepoint.com/:x:/s/MSDE-2022-SoftwareArchitecture/ESVksoXVgMNHtKBKrIwatMYBqorOFaKjxnoqssEy0gNPCg?e=81W7SI) and write your name to claim it.

Then write a short text giving an example for the tradeoff in this assignment.

Pass: 1 unique trade-off

Good: 2 trade-offs

Exceed: >2 trade-offs

}

## Privacy vs. Defensibility

Keeping high-detail logs of everything that goes on on your system and everything that your users do is very useful for detecting and defeating intrusions as well as rolling back any changes that an attacker may have made, but it is a raises huge privacy concerns (also confidentiality but I had to choose one)

## Defensibility vs. Simplicity

A simple system is easier to attack - complexity makes it more difficult to find and exploit vulnerabilities. That said, this is a tradeoff that is almost always heavily skewed towards simplicity - as we know, security through obscurity is not feasible. Moreover, simplicity has a lot of benefits on its own. Therefore, when we reduce simplicity to increase defensibility, it is always done in a later step, with a simpler system as a base, using code obfuscation and similar techniques.

## Feasability vs. Usability
(actually affordability vs. accessibility but these two qualities were missing for some reason)

An important aspect of usability is accessibility - systems with low accessibility are extremely unusable for people with impairments. Hence, to increase usability, one must make their systems accessible. However, accessibility is expensive and will significantly increase the time required to develop your product, as well as the cost of doing so.

Note that this tradeoff has already been claimed by somebody else - when I wrote it I wasn't sure if all the tradeoffs had to be unique. For grading purposes, ignore this one.

## Usability and Performance
Usability and performance go hand in hand - a system with bad performance is also unusable
as users become annoyed at waiting times. This effect is especially strong when performance
goes below user reaction speed - i.e. when they are thinking ahead of the program.
When users write or press buttons faster than they are processed, the forced waiting interrupts
their train of thought, worsening not only experience but also function


# Ex - Feature Modeling

{.instructions

In the context of your chosen project domain, describe your domain using a feature model.

The feature model should be correctly visualized using the following template:

![Example Feature Model Diagram](./examples/feature.fml)

If possible, make use of all modeling constructs.

Pass: Include at least 4 non-trivial features

Good: Include at least 6 non-trivial features, which are all implemented by your project

Exceed: Include more than 8 non-trivial features, indicate which are found in your project and which belong to one competitor

}

![Feature Model Diagram](./model.fml)
The root, Queries, Data modification, Access control, Content Tagging and Result presentation features are not optional and implemented by all competitors.
One competitor, Netflix, has an extremely barebones implementation, with no other features added.
Searching around for a bit I managed to find a competitor with a more interesting system to compare to: vndb.org, a database of visual novels. Their tagging system has the following additional features:
 - Computer-readable format
 - Weighted tags
 - Tag aliases
 - Sub-tags/categories
 - Tag attributes e.g. spoiler
 - Tag filter
 - Sorting
 - Client-supplied attributes (except of course they are themselves the client)
 - Paging
 - Variable page size

# Ex - Context Diagram

{.instructions

Prepare a context diagram to define the design boundary for your project.

Here is a PlantUML/C4 example to get started.

![Example Context Diagram](./examples/context.puml)

Make sure to include all possible user personas and external dependencies you may need.

Pass: 1 User and 1 Dependency

Good: >1 User and >1 Dependency

Exceed: >1 User and >1 Dependency, with both incoming and outgoing dependencies

}

```puml
@startuml
!include <C4/C4_Container>

Person(user_r, "Read-only user", "")
Person(user_w, "Privileged user", "")
Person(user_a, "Sysadmin", "")

System_Boundary(boundary, "Coeus") {

}

System_Ext(db, "DBMS")
System_Ext(web, "Client website")

Rel(user_r, web, "Perform Queries")
Rel(user_w, boundary, "Add/remove/edit tags and items")
Rel(user_a, boundary, "Manage permissions and databases")
Rel(boundary, db, "Store and retrieve", "raw data")
Rel(web, boundary, "translate or forward query")
@enduml
```


# Ex - Component Model: Top-Down

{.instructions

Within the context of your project domain, represent a model of your modular software architecture decomposed into components.

The number of components in your logical view should be between 6 and 9:

- At least one component should be further decomposed into sub components
- At least one component should already exist. You should plan how to reuse it, by locating it in some software repository and including in your model the exact link to its specification and its price.
- At least one component should be stateful.

The logical view should represent provide/require dependencies that are consistent with the interactions represented in the process view.

The process view should illustrate how the proposed decomposition is used to satisfy the main use case given by your domain model.

You can add additional process views showing how other use cases can be satisfied by the same set of components.

This assignment will focus on modularity-related decisions, we will worry about deployment and the container view later.

Here is a PlantUML example logical view and process view.

```puml
@startuml
skinparam componentStyle rectangle

!include <tupadr3/font-awesome/database>

title Example Logical View
interface " " as MPI
interface " " as SRI
interface " " as CDI
interface " " as PSI
[Customer Database <$database{scale=0.33}>] as CDB
[Music Player] as MP
[User Interface] as UI
[Payment Service] as PS
[Songs Repository] as SR
MP - MPI
CDI - CDB
SRI -- SR
PSI -- PS
MPI )- UI
UI --( SRI
UI -( CDI
MP --( SRI
CDB --( PSI


skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

```puml
@startuml
title Example Process View

participant "User Interface" as UI
participant "Music Player" as MP
participant "Songs Repository" as SR
participant "Customer Database" as CDB
participant "Payment Service" as PS

UI -> SR: Browse Songs
UI -> CDB: Buy Song
CDB -> PS: Charge Customer
UI -> MP: Play Song
MP -> SR: Get Music

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

Hint: How to connect sub-components to other external components? Use this pattern.

```puml
@startuml
component C {
   component S
   component S2
   S -(0- S2
}
interface I
S - I

component C2
I )- C2

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

Pass: 6 components (1 decomposed), 1 use case/process view

Good: 6 components (1 decomposed), 2 use case/process view

Exceed: >6 components (>1 decomposed) and >2 use case/process view

}

## Logical View


```puml
@startuml
skinparam componentStyle rectangle

!include <tupadr3/font-awesome/database>
!include <tupadr3/font-awesome/undo>

title Coeus Logical View
interface " " as DB_I
interface " " as MUT_I
interface " " as ADM_I
interface " " as ADM_API_I
interface " " as CAPI_I
' mysql: https://hub.docker.com/_/mysql (free)
[Database <$database{scale=0.33}>] as DB
note bottom of DB: MySQL docker image can be obtained for free here: https://hub.docker.com/_/mysql
component API {
    [Client API <$undo{scale=0.33}>] as CAPI
    [Admin API <$undo{scale=0.33}>] as ADM_API
}
component "Query Engine" {
    interface " " as PROC_I
    interface " " as PARSE_I
    [Query Parser] as PARSE
    [Query Processor] as PROC
}
interface " " as AUTH_I
[Auth Service] as AUTH
[Mutation Processor] as MUT
[Client Management System] as ADM

DB_I -- DB
PROC_I -- PROC
PARSE - PARSE_I
MUT_I -- MUT
ADM - ADM_I
ADM_API_I -- ADM_API
CAPI_I -- CAPI

PARSE_I )- PROC
CAPI --( PROC_I
PROC --( DB_I
AUTH_I - AUTH
CAPI -( AUTH_I
CAPI --( MUT_I
MUT --( DB_I
ADM_I )- ADM_API
ADM --( DB_I

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```


## Process Views

```puml
@startuml
title Coeus "User Query" Process View
autoactivate on

participant "Client API" as CAPI
participant "Auth Service" as AUTH
participant "Query Processor" as PROC
participant "Query Parser" as PARSE
participant "Mutation Processor" as MUT
participant "Admin API" as ADM_API
participant "Client Management System" as ADM
participant "Database" as DB

CAPI -> PROC: Perform Query
PROC -> PARSE: Parse Query
return Query Details
PROC -> DB: Retrieve Data
return Result
return Result

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

```puml
@startuml
title Coeus "Mutation Query" Process View
autoactivate off

participant "Client API" as CAPI
participant "Auth Service" as AUTH
participant "Query Processor" as PROC
participant "Query Parser" as PARSE
participant "Mutation Processor" as MUT
participant "Admin API" as ADM_API
participant "Client Management System" as ADM
participant "Database" as DB

CAPI -> AUTH ++: Check credentials
alt Unauthorised
CAPI <-- AUTH: Failure
else Authorised
'activate PROC
return Success
CAPI -> MUT ++: Perform Command
MUT -> DB ++: Execute changes
return Success
return Success
end

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

```puml
@startuml
title Coeus "Contract Changes" Process View
autoactivate on

participant "Client API" as CAPI
participant "Auth Service" as AUTH
participant "Query Processor" as PROC
participant "Query Parser" as PARSE
participant "Mutation Processor" as MUT
participant "Admin API" as ADM_API
participant "Client Management System" as ADM
participant "Database" as DB

ADM_API -> ADM: Add/edit/remove client
ADM -> DB: Structural changes
return Success
return  Success

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```


# Ex - Component Model: Bottom-Up

{.instructions

Within the context of your project domain, represent a model of your modular software architecture decomposed into components.

To design this model you should attempt to buy and reuse as many components as possible.

In addition to the logical and process views, you should give a precise list to all sources and prices of the components you have selected to be reused.

Write an ADR to document your component selection process (indicating which alternatives were considered).

Pass: Existing design with at least 1 reused components (1 Logical View, 1 Process View)

Good: Existing design with at least 3 reused components (1 Logical View, 1 Process View, 1 ADR)

Exceed: Redesign based on >3 reused components (1 Logical View, >1 Process View, >1 ADR)

}

Note: if I ever actually do this there's no way in hell I'll use all those things,
but for the sake of the assignment, these are components that could in theory be
reused.

```puml
@startuml
skinparam componentStyle rectangle

!include <tupadr3/font-awesome/database>
!include <tupadr3/font-awesome/undo>

title Coeus Logical View
interface " " as DB_I
interface " " as MUT_I
[Client Management System] as ADM
interface " " as ADM_I
interface " " as ADM_API_I
interface " " as CAPI_I
[Database <$database{scale=0.33}>] as DB
note bottom of DB: [[https://dev.mysql.com/doc/refman/8.0/en/ MySQL]] docker image can be obtained for free [[https://hub.docker.com/_/mysql here]]
component API {
    [Client API <$undo{scale=0.33}>] as CAPI
    [Admin API <$undo{scale=0.33}>] as ADM_API
}
component "Query Engine" {
    interface " " as PROC_I
    interface " " as PARSE_I
    [Query Parser] as PARSE
    note top of PARSE: [[https://lexy.foonathan.net lexy]] - free
    [Query Processor] as PROC
}
interface " " as AUTH_I
interface " " as AUTH_I2
note top of AUTH_I2: [[https://oauth.net/2/ OAuth]] - free
[Auth Service] as AUTH
AUTH_I2 -- AUTH
[Mutation Processor] as MUT

interface " " as DEPL_I
[Deployment Manager] as DEPL
note top of DEPL: [[https://docs.aws.amazon.com/ec2/index.html AWS]] - 0.2-37 $/h
DEPL_I -- DEPL
ADM --( DEPL_I

DB_I -- DB
PROC_I -- PROC
PARSE - PARSE_I
MUT_I -- MUT
ADM - ADM_I
ADM_API_I -- ADM_API
CAPI_I -- CAPI

PARSE_I )- PROC
CAPI --( PROC_I
PROC --( DB_I
AUTH_I - AUTH
CAPI -( AUTH_I
CAPI --( MUT_I
MUT --( DB_I
ADM_I )- ADM_API
ADM --( DB_I

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

```puml
@startuml
title Coeus "User Query" Process View
autoactivate on

participant "Client API" as CAPI
participant "Auth Service" as AUTH
participant "Query Processor" as PROC
participant "Query Parser" as PARSE
participant "Mutation Processor" as MUT
participant "Admin API" as ADM_API
participant "Client Management System" as ADM
participant "Deployment Manager" as DEP
participant "Database" as DB

CAPI -> PROC: Perform Query
PROC -> PARSE: lexy::parse
return Query
PROC -> DB: SELECT
return Result
return Result

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

```puml
@startuml
title Coeus "Mutation Query" Process View
autoactivate off

participant "Client API" as CAPI
participant "Auth Service" as AUTH
participant "Query Processor" as PROC
participant "Query Parser" as PARSE
participant "Mutation Processor" as MUT
participant "Admin API" as ADM_API
participant "Client Management System" as ADM
participant "Deployment Manager" as DEP
participant "Database" as DB

[-> AUTH ++: Authorise
return Access Token
CAPI -> AUTH ++: Validate
alt Unauthorised
CAPI <-- AUTH: Failure
else Authorised
'activate PROC
return Success
CAPI -> MUT ++: Perform Command
MUT -> DB ++: UPDATE
return Success
return Success
end

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

```puml
@startuml
title Coeus "Contract Changes" Process View
autoactivate on

participant "Client API" as CAPI
participant "Auth Service" as AUTH
participant "Query Processor" as PROC
participant "Query Parser" as PARSE
participant "Mutation Processor" as MUT
participant "Admin API" as ADM_API
participant "Client Management System" as ADM
participant "Deployment Manager" as DEP
participant "Database" as DB

ADM_API -> ADM: Add Client\nEdit Client\nRemove Client
opt
ADM -> DEP: RunInstances\nTerminateInstances
return Success
end
ADM -> DB: ALTER SCHEMA\nCREATE DATABASE\nDROP DATABASE
return Success
return  Success

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

1. What did you decide?

## Authentication Protocol: OAuth 2

2. What was the context for your decision?

We have a need to authenticate users. The API for this must be secure, but
easy to use and simple to implement. OAuth is an existing protocol designed for
precisely these criteria. It is also the most popular one out there, meaning
the programmers of our clients are likely used to it and will find it easy
to implement the client side or re-use an existing library for that purpose.
The rest of the project is largely independent of this choice, requiring only
minor changes in the Client API component and an appropriate implementation
in the Auth Service component

3. What is the problem you are trying to solve?

How can we allow users to authenticate in an easy but secure manner?

4.  Which alternative options did you consider?

OAuth, Kerberos, own solution

5. Which one did you choose?

OAuth 2

6. What is the main reason for that?

OAuth is a more popular and widely supported protocol than Kerberos.
Its age and popularity also makes it more robust than a self-designed
protocol is likely to be.

1. What did you decide?

## Parser: Lexy

2. What was the context for your decision?

Our users send us queries to retrieve data - as a string.
In order to filter the data for an appropriate response, the string needs to be
parsed into a format usable by the application. Lexy can be used for that.
The impact of this choice is mainly in performance.

3. What is the problem you are trying to solve?

How can we turn a query string into an object we can use to process data?

4.  Which alternative options did you consider?

Lexy, custom parser

5. Which one did you choose?

Lexy

6. What is the main reason for that?

Lexy is an open-source, freely-available parser which allows specification of
grammars in a convenient C++ DSL.
Using a library instead of building it ourselves saves a lot of work and
provides a guarantee of performance.

Unfortunately, although performance is guaranteed to not be *bad*, it is also
otherwise out of our control, meaning if we find it to be insufficient later on,
we cannot do anything about it.

1. What did you decide?

## Deployment provider: AWS

2. What was the context for your decision?

Given that we decided to run our software ourselves instead of selling it to
clients as a package, we have the need for hardware to run it. A multitenant
architecture means we'd also benefit from a scalable solution and that we can
scale horizontally with little effort.

3. What is the problem you are trying to solve?

Where will we run our software?

4.  Which alternative options did you consider?

AWS, fixed host, self-host

5. Which one did you choose?

AWS

6. What is the main reason for that?

Renting fixed machines from a provider would allow us to run our software at low
cost, but makes it difficult to scale when we need more capacity to accomodate
new or growing clients.

Self-hosting can be very expensive and would likely provide weaker guarantees
on availability and failure recovery than outsourcing.

AWS is rather expensive as hosting goes due to its flexibility, but allows us to
increase and decrease resources programmatically and quickly in response to
client needs.


```puml
@startuml
title Middleware with control flow
'autoactivate on

participant "Frontend" as F2
participant "Frontend" as F
participant "Middleware" as M
participant "Backend" as B
participant "Backend" as B2
actor "admin" as ADMIN

create B
B <- ADMIN
M o<- B
F -> M ++
M --> B ++
M <-- B --
F <- M --

create B2
B2 <- ADMIN
M o<- B2

F -> M ++
M --> B ++

F2 -> M ++
M --> B2 ++
M <-- B2 --
F2 <- M --
M <-- B --
F <- M --

M -> B
destroy B


F -> M ++
M --> B2 ++
M <-- B2 --
F <- M --

M -> B2
destroy B2

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```


# Ex - Interface/API Specification

{.instructions

In this iteration, we will detail your previous model to specify the provided interface of all components based on their interactions found in your existing process views.

1. choose whether to use the top down or bottom up model. If you specify the interfaces of the bottom up model, your interface descriptions should match what the components you reuse already offer.

2. decide which interface elements are operations, properties, or events.

Get started with one of these PlantUML templates, or you can come up with your own notation to describe the interfaces, as long as it includes all the necessary details.

The first template describes separately the provided/required interfaces of each component.

![Separate Required/Provided Interfaces](./examples/interface1.puml)

The second template annotates the logical view with the interface descriptions: less redundant, but needs the logical dependencies to be modeled to show which are the required interfaces.

![Shared Interfaces](./examples/interface2.puml)

Pass: define interfaces of all outer-level components

Good: Define interfaces of all outer-level components. Does your architecture publish a Web API? If not, extend it so that it does.

Exceed: Also, document the Web API using the OpenAPI language. You can use the [OpenAPI-to-Tree](http://api-ace.inf.usi.ch/openapi-to-tree/) tool to visualize the structure of your OpenAPI description.

}

```puml
@startuml
skinparam componentStyle rectangle

!include <tupadr3/font-awesome/database>
!include <tupadr3/font-awesome/undo>

title Coeus Logical View
interface " " as DB_I
    note left of DB_I
    operations:
    ..
    execute(SQL)
    end note
interface " " as MUT_I
    note right of MUT_I
    operations:
    ..
    addItem(udd)
    removeItem(id)
    getItem(id)
    setTag(id, tag, value)
    removeTag(id, tag)
    end note
interface " " as ADM_I
    note top of ADM_I
    operations:
    ..
    addCollection(properties)
    removeCollection(id)
    setProperties(id, properties)
    end note
interface " " as ADM_API_I
    note top of ADM_API_I
    operations:
    ..
    /collections POST
    /collections/{id} DELETE
    /collections/{id} GET
    /collections/(id) PUT
    /users/{name} PUT
    end note
interface " " as CAPI_I
    note top of CAPI_I
    operations:
    ..
    /collections/{id}?q={query} GET
    /collections/{id} POST
    /collections/{id}/{iid} GET
    /collections/{id}/{iid} DELETE
    /collections/{id}/{iid}/{tag} PUT
    /collections/{id}/{iid}/{tag} DELETE
    end note
[Database <$database{scale=0.33}>] as DB
component API {
    [Client API <$undo{scale=0.33}>] as CAPI
    [Admin API <$undo{scale=0.33}>] as ADM_API
}
interface " " as PROC_I
    note right of PROC_I
    operations:
    ..
    execute(query)
    end note
component "Query Engine" {
    [Query Processor] as PROC
    interface " " as PARSE_I
    [Query Parser] as PARSE
}
[Auth Service] as AUTH
[Mutation Processor] as MUT
[Client Management System] as ADM
interface " " as AUTH_I
    note top of AUTH_I
    operations:
    ..
    isValid(token)
    addUser(name)
    end note

DB_I -- DB
PROC_I -- PROC
PARSE - PARSE_I
MUT_I -- MUT
ADM - ADM_I
ADM_API_I -- ADM_API
CAPI_I -- CAPI

PARSE_I )- PROC
CAPI --( PROC_I
PROC --( DB_I
AUTH_I - AUTH
ADM_API -( AUTH_I
CAPI -( AUTH_I
CAPI --( MUT_I
MUT --( DB_I
ADM_I )- ADM_API
ADM --( DB_I

skinparam monochrome true
skinparam shadowing false
skinparam defaultFontName Courier
@enduml
```

openapi specification:
```yaml
openapi: "3.0.2"
info:
  title: Coeus API
  version: "0.1"
paths:
  /adm/collections:
    post:
      description: add a new collection
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
  /adm/collections/{id}:
    put:
      description: edit a collection
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
    get:
      description: get a collection
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
        "404":
          description: NOT FOUND
    delete:
      description: delete a  collection
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
        "404":
          description: NOT FOUND
  /adm/users/{name}:
    put:
      description: add a new user or change their credentials
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
  /collections/{id}:
    get:
      description: perform a query
      parameters:
        - in: query
          name: q
          schema:
            type: string
          description: the query
      responses:
        "200":
          description: OK
        "404":
          description: NOT FOUND
    post:
      description: add a new item
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
        "404":
          description: NOT FOUND
  /collections/{id}/{iid}
    get:
      description: get an item
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
        "404":
          description: NOT FOUND
    delete:
      description: delete an item
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
        "404":
          description: NOT FOUND
  /collections/{id}/{iid}/{tag}:
    put:
      description: add or edit a tag on an item
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
        "404":
          description: NOT FOUND
    delete:
      description: remove a tag from an item
      responses:
        "200":
          description: OK
        "401":
          description: NOT AUTHORISED
        "404":
          description: NOT FOUND
```

Two endpoints missing due to bug in the 'openapi to tree' app:
![](../images/apiTree.png)

openapi to tree feedback:
 - It refuses to render the above document unless you comment the `/collections/{id}/{iid}` path
 - The main advantage of a tree structure is that it allows easy representation of
   deeply nested structures, but most APIs are very flat and won't be rendered very well by this.
 - The tool has tooltips, but they do not include any useful information. There should at least
   be the description and non-path parameters
 - If the tool is meant to be used by people new to openapi, better error messages would be nice
 - The colors of the get/post/put/delete markers do not provide enough contrast to the
   text, they should be less saturated
 - Occasionally the editor opens an autocomplete tooltip with no content, which it will never close again
 - The viewer's zoom controls are too sensitive for macos scrolling (which is usually much more fine-grained than other platforms')

# Ex - Connector View

{.instructions

Extend your existing models introducing the connector view

For every pair of connected components (logical view), pick the most suitable connector. Existing components can play the role of connector, or new connectors may need to be introduced.

Make sure that the interactions shown in the process views reflect the primitives of the selected connector

Pass: model existing connectors based on previous model decisions

Good: model existing connectors based on previous model decisions, write an ADR about the choice of one connector

Exceed: introduce a new type of connector and update your existing process view
(sequence diagram) to show the connector primitives in action

}

# Ex - Adapters and Coupling

{.instructions

1. Highlight the connectors (or components) in your existing bottom-up design playing the role of adapter. (We suggest to use the bottom-up design since when dealing with externally sourced components, their interfaces can be a source of mismatches).
2. Which kind of mismatch** are they solving?
3. Introduce a wrapper in your architecture to hide one of the previously highlighted adapters
4. Where would standard interfaces play a role in your architecture? Which standards could be relevant in your domain?
5. Explain how one or more pairs of components are coupled according to different coupling facets
6. Provide more details on how each adapter solves the mismatches identified using pseudo-code or the actual code
7. How can you improve your architectural model to minimize coupling between components? (Include a revised logical/connector view with your solution)

Pass: 1-5 (with one adapter)

Good: 1-6 (with at least two adapters)

Exceed: 1-7 (with at least two adapters)

** If you do not find any mismatch in your existing design we suggest to introduce one artificially.

## Hints

* (1) Should we find cases where two components cannot communicate (and are doing it wrongly) and highlight they would need an adapter?, or cases where we have already a "component playing the role of adapter in the view" and highlight only the adapter?

  *Both are fine. We assumed that if you draw a dependency (or a connector) the interfaces match, but if you detect that the components that should communicate cannot communicate then of course introduce an adapter to solve the mismatch*

* (2) Please show the details about the two interfaces which do not match (e.g., names of parameters, object structures) so that it becomes clear why an adapter is needed and what the adapter should do to bridge the mismatch

* (5-6) These questions are about the implications on coupling based on the decisions you documented in the connector view.
Whenever you have a connector you couple together the components and different connectors will have different forms of coupling

  For example, if you use calls everywhere, do you really need them everywhere? is there some pair of components where you could use a message queue instead?

  Regarding the coupling facets mentioned in question 5. You do not have to answer all questions related to "discovery", "session", "binding", "interaction", "timing", "interface" and "platform" (p.441, Coupling Facets). Just the ones that you think are relevant for your design and by answering them you can get ideas on how to do question 6.

}

# Ex - Physical and Deployment Views

{.instructions

a. Extend your architectural model with the following viewpoints:

1. Physical or Container View

2. Deployment View

Your model should be non-trivial: include more than one physical device/virtual container (or both). Be ready to discuss which connectors are found at the device/container boundaries.

b. Write an ADR about which deployment strategy you plan to adopt. The alternatives to be considered are: big bang, blue/green, shadow, pilot, gradual phase-in, canary, A/B testing.

c. (Optional) Prepare a demo of a basic continuous integration and delivery pipeline for your architectural documentation so that you can obtain a single, integrated PDF with all the viewpoints you have modeled so far.

For example:

- configure a GitHub webhook to be called whenever you push changes to your documentation
- setup a GitHub action (or similar) to build and publish your documentation on a website

Pass: 1 physical view, 1 deployment view, 1 ADR (b.)

Good: >1 physical view, >1 deployment view, 1 ADR (b.)

Exceed: 1 physical view, 1 deployment view, 1 ADR (b.) + 1 demo (c.)

}

# Ex - Availability and Services

{.instructions

The goal of this week is to plan how to deliver your software as a service with high availability.

1. If necessary, change your deployment design so that your software is hosted on a server (which could be running as a Cloud VM). Your SaaS architecture should show how your SaaS can be remotely accessed from a client such as a Web browser, or a mobile app
2. Sketch your software as a service pricing model (optional)
3. How would you define the availability requirements in your project domain? For example, what would be your expectation for the duration of planned/unplanned downtimes or the longest response time tolerated by your clients?
4. Which strategy do you adopt to monitor your service's availability? Extend your architecture with a watchdog or a heartbeat monitor and motivate your choice with an ADR.
5. What happens when a stateless component goes down? model a sequence diagram to show what needs to happen to recover one of your critical stateless components
6. How do you plan to recover stateful components? write an ADR about your choice of replication strategy and whether you prefer consistency vs. availability. Also, consider whether event sourcing would help in your context.
7. How do you plan to avoid cascading failures? Be ready to discuss how the connectors (modeled in your connector view) impact the reliability of your architecture.
8. How did you mitigate the impact of your external dependencies being not available? (if applicable)

Pass: 1, 3, 4, one of:  5, 6, 7, 8

Good: 1, 2, 3, 4, two of:  5, 6, 7, 8

Exceed: 1, 2, 3, 4, 5, 6, 7, 8

}

# Ex - Scalability

{.instructions

Now that your architecture delivers your software as a service, let's redesign it so that it can scale!

1. Pick one scalability dimension: number of clients, size of input, size of state, number of dependencies

2. How well does your architecture scale along the chosen dimension? Where do you expect the bottleneck to be?

3. Modify your architecture to remove the scalability bottleneck you have identified (show both logical, process and deployment view) - consider whether the API/interface of the bottleneck component should be improved.

4. Write an ADR regarding the scalability pattern you have introduced.

5. Write an ADR regarding the issue of component discovery, choosing one of the alternatives: dependency injection vs. directory. Can you identify an existing component playing the role of directory/dependency injection container? Could you give an example of where you would need to add such component to facilitate dynamic component discovery?

Pass: 1, 2, 3, 5

Good: 1, 2, 3, 4, 5

Exceed: 1, 2, 3, 4, 5 then redo 1, 2, 3 for different scalability dimensions

}

# Ex - Flexibility

{.instructions

Only dead software stops changing. You just received a message from your customer, they have an idea. Is your architecture ready for it?

1. Pick a new use case scenario. Precisely, what exactly do you need to change of your existing architecture so that it can be supported? Model the updated logical/process/deployment views.

2. Pick another use case scenario so that it can be supported without any major architectural change (i.e., while you cannot add new components, it is possible to extend the interface of existing ones or introduce new dependencies). Illustrate with a process view, how your previous design can satisfy the new requirement.

3. Change impact. One of your externally sourced component/Web service API has announced it will introduce a breaking change. What is the impact of such change? How can you control and limit the impact of such change? Update your logical view

4. Open up your architecture so that it can be extended with plugins by its end-users. Where would be a good extension point? Update your logical view and give at least one example of what a plugin would actually do.

5. Assuming you have a centralized deployment with all stateful components storing their state in the same database, propose a strategy to split the monolith into at least two different microservices. Model the new logical/deployment view as well as the interfaces of each microservice you introduce.

Pass: 1, one out of 2-5.

Good: 1, two out of 2-5.

Exceed: 1-5.

}
