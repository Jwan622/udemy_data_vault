# Data vault course from Udemy


# Traditional modeling concepts

## ERD
- An ERD is a visual representation of different entitites within a system and how they relate to each other. 
- they explain logical structure of databases.

- ERDs contain entities (rectangles), attributes (ovals), and relationships (diamond).
- entities are basic entities of ERDs and you can think of them as nouns (studnets, projets, faculty, etc). they are converted to tables in database
- relationships are how entities interact with each other.

__attributes__: property or characteristic of entity. these become columns in the table for the entity.

### Types of attributes:
- primary key - uniquely identifies an entity. unique and never changing and never null.
    - first name is not a good primary and key and neither is the combo of first and last name because two people can have the same names. but email is a beter primary key.

- foreign key - referencing key used to link two tables together. It can replicate, can be null.

- relational diagrams can be imported into your database. 

### Data models
- way to organize data
- done during planning phase. business rules are converted to data structures. conversion of business to graphical relationships.
- can be divided into 3 catagories, with different levels of abstraction.

![data_model_types.png](images/data_model_types.png)


### DWH architecture
- the staging area sits in between source data (could be raw files) and the DWH. Why? We don't want to disrupt the source data databases by running complicated ETL so we copy data from source to staging. we don't want to discrupt operational systems.
- Staging layer might have a lot of different formats of the same data. etl merges this data into data warehouse so the DWH has a single point of truth and unified data. It has a unified structure. we transform to the expected structure before loading into data warehouse from source data that might be different structures. Single structure/standard format should exist in DWH.


### Data Normalization
- column cell cannot hold more than 1 value.

__1nf__: 
![1nf](images/1nf.png)

Still redundancy problems as you can see

__2nf__:
- in 1nf
- redundant data must be moved to a separate table
![2nf](images/2nf.png)
  

__3nf__:
![3nf](images/3nf.png)

state column depends on country column and not the primary key. 
- state does not depend on primary key.
- transitive dependency: when a non-key column depends on another non-key column. we should separate these out to another table. 
- Here state and country change together. so we separate it out since state and country are not dependent on country column.


### Data Denormalizaton
- data from multiple tables are combined into 1
- can improve speed. Can avoid constant JOINS in relational database and improve performance. 
- it's an optimization technique AFTER applying normalization. it does not mean avoding normalization.
- it's not an alternative to normalization, denormalization is a technique to optimize queries.

![denorm_vs_norm.png](images/denorm_vs_norm.png)

### Why is normalization important for OLTP systems?
- updates are fast because you only have to updat 1 row. 

![normalization_importance.png](images/normalization_importance.png)

### Why is denorm important?
- updates have to update all rows in record. You can't miss 1 otherwise data inconsistency.
- but because data is combined into 1 place, reads are faster.
![denormalization_importance.png](images/denormalization_importance.png)
  
### Norm vs Denorm
![norm_vs_denorm.png](images/norm_vs_denorm.png)

### Top down vs bottom up DWH design
- in top down, the DWH uses ER modeling (3nf) while data marts use dimensional modeling (star schema or snowflake). Remember ER modeling uses foreign keys to relate tables, attributes as columns, and entites as tables.
- in bottom up, the DWH uses dimensional modeling and the data marts use dimensional modeling.
concepts we need to know: er modeling, dimensional modeling, 3nf, 

**top down advantages**:
- DWH is single source of truth for enterprise.
- subsequent project cost is lower.
- there is  data consistency because the data marts pull from the datawarehouse. So if data changes in the DWH, all data marts that use that data will be updated.

**top down disadvantages**:
- initial setup takes time. the DWH is a large project
- the model becomes complex over time as it involves more tables and joins.

**bottom up advantages**:
- works well for department metrics
- focuses on creating user friendly data structures using star schemas in data marks
- delivers rapidly, less infra up front
- works well for dept wide metrics
- just need to create new data marts

**bottom up disadvantages**:
- no one source of truth. data is not fully integrated before serving reporting needs
- data redundant due to denormalized data model, have to update in multiple areas



### Dimensional modeling.
- remember that OLTP systems are meant for fast write and not suitable for reporting. they normally employ ER model with 3nf to avoid data redundancy problems
- DWH handle compelx queries
- DWH top down or bottom up

Why dimensional modeling?
- you can write SQL but that's complex for avg user.
- our objective is a DWH that is easy for analyst to write queries. main goal is for fast retrieval.
- utilize fact and dimension tables.

What are Fact Tables?:
- sales revenue
- amount
- quantity

Dimension Tables:
- context surrounding business process event. Gives who/what/where
- customer, location, product names, etc.
- a description about the facts
- oftne denormalized

Attributes:
- various characteristics of the dimensions. state, country, zip for location dimensions for example.
- used to search or filter or classify the facts.
- exist in both dimension and fact tables.

![modeling_recap.png](images/modeling_recap.png)

![fact_dimension.png](images/fact_dimension.png)

**dimension tables**:
- denormalized
- descriptive characteristics of the facts
- joined to fact table via foreign key


![dimension_vs_normalForm.png](images/dimension_vs_normalForm.png)

**why dimensional modeling**:
- easy to understand regardless of end user tech knowledge
- faster database performance
- fewer tables in dimension tables because denormalized so better performance
- used in OLAP and not OLTP more simplified structure so business users can write queries against it.
- whereas Normal Form and 3nd are used in OLTP for faster writes.


### Star Schema
- in dimensional model, there are 2 types of dimensional models
- star schema can have 1 fact table and a number of associated tables around it. it resembles a star, so it's called a star schema.
- dimension tables are NOT joined to each other and only have primary key field
- dimension tables are not normalized tables. So customer tables can have customer info and location info. Or Sales Channel table can have channel information and store information. Not normalized data. So this information is stored in a single table.
- star schema has easy to use structure


### Snowflake Table
- further normalization. For customer info will separate out customer and location info.
- as a result, dimension tables can have foreign keys.
- main benefit: snowflake uses smaller disk space due to less denormalized structure
- but due to multiple tables, query performance is less.

![star_vs_snow.png](images/star_vs_snow.png)
![star_vs_snow2.png](images/star_vs_snow2.png)


