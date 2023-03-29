# Type-checking in knowledge graphs

Iztok Savnik, Famnit<  

Koper, August 2021

<br>

## 1. Problems

* Type-checking database
    - For each triple determine a type
    - Check the type against the stored schema

* Type-checkig queries
    - We have graph pattern 
        - A set of triples including consts and vars
    - We derive types of TPs
    - We derive types of GPs

* Many properties of things are described through classification
    - Relations between the classes, instances and classes, etc.
    - We must have lang. constructs to capture this
    - Annotating TPs with types
    - Type annotations for specification of queries (can constrain TPs)
    - Relating objects on the basis of schemata and logic
    - To use the formal operations from construction of KGs

## 2. Tasks

* What are the main tasks involved in type-checking?
    - Compute a type of a thing
    - Compute a type of a triple
    - Determine a type of a TP
    - Determine a type of a GP

* What are the results of type-checking?
    + The types of TPs from a 
    + Diagnostics of triple type (classification)

* The usage of triple types 
    + For localizing the parts of SPARQL queries
    + For distribution of triples on the basis of the selected types


## 3. Compute a type of a literal

* For a given triple t = (s,p,o) (s,p,o are concrete!) we compute a type
    + Collect all types T associated to a literal l by `rdf:type` predicate
        - We get a set of types, say S, for each literal 
        - The type of l is the intersection of types T!!
            - Literal has all types from T
    + The common type of a literal is intersection of *lub* types of T
        - Intersection, since a literal is an element in ***all*** *lub* types of T
        - *lub*(S) = { t| t=*lub1*(S) } where ***lub1*** computes one lub type of S
    + Example: 
        - rdf:type for *tom* gives *phd_student* and *assistant*
        - *tom* has *lub* types *student* and *employee*
        - *tom : student & employee*
    + Lub types define common properties of types from S!
         - All types from *lub*(S) are lub generalizations of all types from S
         - Therefore a type of a given component of a schema triple is an **intersection** 
           of types from *lub*(S)
         - This means that all types from *lub*(S) must have a generalization 
           that is a component of some stored schema triple (from T!) 
    + From the perspective of values (related triples with common S) 
        - Using intersection gives tuple-based view
        - A literal is associated with the properties of types *student* ***and*** *employee*


## 4. Compute a type of a triple

* For a given triple t=(s,p,o) we have a set of possible schema triples
    + A schema triple (t_s,t_p,t_o) represents a type T=t_s*t_p*t_o
    + One predicate can have multiple definitions
    + The type of t is the union (\\cup) of types representing one schema triple
         - T = t_s1*t_p1*t_o2 | t_s2*t_p2*t_o2 | ...
    + In the next step we constraint T with the types of t componnets
         - Thus, we (should) obtain one type t_s*t_p*t_o for one concrete schema triple (t_s,t_p,t_o)
         - Is it possible to obtain more than one? 

+ When we get only *lub* sets for the types of s,p,o
    + How to compute types of triples?

* We now have T and *lub*(S)
    + There are two tasks: 
    1. Deriving a unique type from T (one of types connected union) using *lub*(S)
         - We have to link types from *lub*(S) to types from T
    2. At the same time we are checking that types from *lub*(S) are correct
         - The procedure may include the dignosis of a problem (type mismatch) 
    

## 5. Determine a type of a TP and GP

### Compute a type of a TP

* Type of TP without a predicate? use rdf:predicate.
* Type of TP without the schema? use generalizations.
* type of TP with predicate? use a triple type defined as the schema.
* type of TP can be \cup of triple types (defined in the schema).

### Compute a type of a GP

* Meeting points in GP: use \\cap type constructor
* \\cap of \\cup of triple types can select one of triple types
* One triple type is selected by other oper of \\cap


