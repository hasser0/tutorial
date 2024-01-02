- [Data models](#data-models)
- [Keys](#keys)

# Data models
Database models are representations (mainly graphical) of how data is stored. The main database models nowadays are **Relational Model** and **ER model**
+ Relational models are the tables and their relations using arrows with numbers to represent cardinality
+ ER model's components are
  - Entities: any object or abstract idea
  - Attributes: values related to the entities
  - Relationships: associations among data, described by a verb and cardinality
+ Big Data models: Most relational models doesn't match big data challenges
  - Unstructured data
  - Adding millions of rows to tables leds to the need for more storage and processing
  - OLAP systems works better

# Keys
There are several types of keys used for modeling databases
+ Super: Any set of attributes used to uniquely identify a row
+ Composite: A key composed of more than one attribute
+ Candidate: Set of attributes that uniquely identify rows in a table
+ Surrogate: Keys created by the DBMS and not releated to the data. Also called synthetic key
+ Minimal: Among all candidate keys, those with minimal cardinality
+ Primary: It's a choosen candidate key, ideally minimal. Used to ensure entity integrity
+ Foreign: It's a key used in a child's table to create a relation with a parent's table. Used to ensure referencial integrity
+ Alternative: Alternative to the primary key
