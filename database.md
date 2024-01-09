- [Data models](#data-models)
- [Keys](#keys)
- [Indexes](#indexes)
- [Data Warehouses](#data-warehouses)

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

# Indexes
Indexes are data structures used to improve query performance by mapping values in certain columns to pointers and thus avoid unnecessary I/O operations. There exists two general types of indexes:
+ Clustered: Specify data order on disk and there can only exists a single index of this type per table. When a primary key is defined on a table a default index is created.
+ Nonclustered: Doesn't change data order, access data using pointers saved on different structures such as:
  - Hash tables: Good for simple equal operations
  - B-tree: Relative small repetition values in a column
  - Bitmap: Small number of values repeated many times, used to represent the existence of a condition

# Data Warehouses
A data warehouse is an special type of database design as decision support and analytical purpuses created due OLTP inneficiencies while creating reports (too many joins are needed to specific insights on data).
Data warehouses are implemented using relational databases but the design is different from those used in transactional databases. In this case tables should be denormalized to avoid unnecesary joins. The design for DW is called **dimensional design** which incorporates new terms:
+ Dimension: Is a context field that explains in detail the data being used
+ Fact: Is a measure or numeric value that can be summed, averaged and manipulated
+ Dimension table: Type of table that explains in detail more about dimensions
+ Fact table: Type of table compose of dimension keys and facts
+ Star schema: Composed of dimensional tables that contains static data and fact tables that contains historical data (business process tables)
+ Snowflake schema: Can be seen as a star schema with normalized dimensions (less efficient)

OLAP systems are closely releated to DW, but they are not the same; the first being a system specialized in retrieve analysis from data warehouses and the second working as the sources for the data in those analysis. There are 3 types of OLAP systems:
+ ROLAP: Relational OLAP, where all the tables are implemented on relational databases
+ MOLAP: Multidimensional OLAP, where all the tables are implemented using n-dimensional arrays
+ HOLAP: Hybrid OLAP, a mixture of the previous
