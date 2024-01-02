- [General](#general)
  - [Create a dbt project](#create-a-dbt-project)
  - [Basic commands](#basic-commands)
- [YAML files](#yaml-files)
  - [Sources](#sources)
  - [Schema](#schema)
  - [Packages](#packages)
  - [Documentation](#documentation)
- [Models](#models)
  - [Naming conventions](#naming-conventions)
  - [Materialization types](#materialization-types)
  - [Model example](#model-example)
- [Sources](#sources-1)
  - [Configuring sources](#configuring-sources)
- [Seeds](#seeds)
- [Snapshots](#snapshots)
- [Tests](#tests)
  - [Running tests](#running-tests)
  - [Generic tests](#generic-tests)
  - [Singular tests](#singular-tests)
  - [Macros test](#macros-test)
  - [Custom Generic Tests](#custom-generic-tests)
- [Macros](#macros)
- [Packages](#packages-1)
  - [Configuring packages](#configuring-packages)
  - [Using packages](#using-packages)
- [Documentation](#documentation-1)
  - [External file documentation](#external-file-documentation)
  - [Overview section](#overview-section)
- [Analyses](#analyses)
  - [Debugging dbt](#debugging-dbt)

# General
## Create a dbt project
```sh
dbt init dbtlearn
```

## Basic commands
```sh
dbt run
dbt compile
dbt run --full-refresh
dbt compile #run and test for every block of models
```

# YAML files
## [Sources](##Configuring-sources)
## [Schema](##Generic-tests)
## [Packages](##Configuring-packages)
## [Documentation](#Documentation)

# Models

## Naming conventions
+ Sources (`src`)
+ Staging (`stg`)
+ Intermediate (`int`)
+ Fact (`fct`)
+ Dimension (`dim`)


## Materialization types
1. table
2. view
3. incremental
4. ephemeral
5. materialized view

## Model example
```sql
{{
  config(
    materialized = 'incremental',
    on_schema_change='fail'
    )
}}
WITH src_reviews AS (
    SELECT * FROM {{ ref('src_reviews') }}
)
SELECT * FROM src_reviews
{% if is_incremental() %}
    WHERE date > (select max(date) FROM src_reviews)
{% endif %}
```

# Sources
Sources represent the raw data that is loaded into the data warehouse. We can reference tables in our models with an explicit table name. However, setting up Sources in dbt and referring to them with the source function enables a few important tools.
+ Multiple tables from a single source can be configured in one place.
+ Sources are easily identified as green nodes in the Lineage Graph.
+ You can use dbt source freshness to check the freshness of raw tables

This are some basic commands for sources in dbt
```sh
dbt source
dbt source freshness
```
## Configuring sources
Content of `models/sources.yml`
```yaml
version: 2

sources:
  - name: source_name
    database: database_name
    schema: schema_name
    tables:

      - name: source_reference_name
        identifier: table_name
        loaded_at_field: date_field_when_loaded
        freshness:
          warn_after: {count: 1, period: hour}
          error_after: {count: 24, period: hour}
```

# Seeds
Seeds are tabular files that are added as  new tables. Place any local source to the `seeds` folder.

This are some basic commands for uploading seeds
```sh
dbt seeds
```
# Snapshots

Snapshots are the way dbt handles automatically type-2 SCD (Slowly changing dimensions) by adding extra fields in case any change is made.

```sql
{% snapshot scd_raw_listings %}

{{
   config(
       target_schema='dev',
       unique_key='id',
       strategy='timestamp',
       updated_at='updated_at',
       invalidate_hard_deletes=True
   )
}}

select * FROM {{ source('airbnb', 'listings') }}
{% endsnapshot %}
```

# Tests
## Running tests
```sh
dbt test
dbt test --select dim_listings_cleansed
```

## Generic tests
Contents of `models/schema.yml`:
```yaml
version: 2

models:
  - name: dim_listings_cleansed
    columns:

     - name: listing_id
       tests:
         - unique
         - not_null

     - name: host_id
       tests:
         - not_null
         - relationships:
             to: ref('dim_hosts_cleansed')
             field: host_id

     - name: room_type
       tests:
         - accepted_values:
             values: ['Entire home/apt',
                      'Private room',
                      'Shared room',
                      'Hotel room']
```

## Singular tests
Create a `sql` file under `tests` folder. Tests are passed when queries return 0 rows
```sql
SELECT *
FROM {{ ref('dim_listings_cleansed') }}
WHERE minimum_nights < 1
LIMIT 10
```

## Macros test
You can use macros to define tests you can. Once the macro is defined, create a `sql` file under `tests` folder and call the macro
```sql
{{ no_nulls_in_columns(ref('dim_listings_cleansed')) }}
```

## Custom Generic Tests
Create a `sql` file under `macros` folder. The `model` and `column_name` parameters are mandatory, but additional parameters can be added
```sql
{% test positive_value(model, column_name) %}
SELECT *
FROM {{ model }}
WHERE {{ column_name}} < 1
{% endtest %}
```

# Macros
Create a `sql` file under `macros` folder and add parameters
```sql
{% macro no_nulls_in_columns(model) %}
    SELECT * FROM {{ model }} WHERE
    {% for col in adapter.get_columns_in_relation(model) -%}
        {{ col.column }} IS NULL OR
    {% endfor %}
    FALSE
{% endmacro %}
```

# Packages

## Configuring packages
Content of`packages.yml`
```yaml
packages:
  - package: dbt-labs/dbt_utils
    version: 0.8.0
```
## Using packages
Call any function from your included packages using jinja curly brackets `{{}}`
```sql
WITH src_reviews AS (
    SELECT * FROM {{ ref('src_reviews') }}
)
SELECT
    {{
        dbt_utils.surrogate_key([
            'listing_id',
            'review_date',
            'reviewer_name',
            'review_text'
        ])
    }}
    AS review_id,
FROM src_reviews
```

# Documentation
You can create and view documentation using the following commands
```sh
dbt docs generate
dbt docs serve
```

Content of `models/schema.yml`
```yaml
version: 2

models:
  - name: dim_listings_cleansed
    description: Cleansed table which contains Airbnb listings.
    columns:

      - name: listing_id
        description: Primary key for the listing

      - name: host_id
        description: The hosts's id. References the host table.

      - name: room_type
        description: Type of the apartment / room

      - name: minimum_nights
        description: '{{ doc("dim_listing_cleansed__minimum_nights") }}'
```
You can add documentation to tables, and columns in the `models/schema.yml` file using inline or referencing to an external markdown file with `{{ doc('table__column') }}`
the contents of the markdown referenced file `models/docs.md`:
## External file documentation
```txt
{% docs dim_listing_cleansed__minimum_nights %}
Minimum number of nights required to rent this property.

Keep in mind that old listings might have `minimum_nights` set
to 0 in the source tables. Our cleansing algorithm updates this to `1`.

{% enddocs %}
```

## Overview section

The general documentation can be replaced by using the following jinja macros in file `models/overview.md`:
```md
{% docs __overview__ %}
# Airbnb pipeline

Hey, welcome to our Airbnb pipeline documentation!

Here is the schema of our input data:
![input schema](https://dbtlearn.s3.us-east-2.amazonaws.com/input_schema.png)

{% enddocs %}
```

# Analyses
Analysis is a section where you can run `sql` queries without creating a table

## Debugging dbt

```
dbt --debug test --select dim_listings_w_hosts
```

Keep in mind that in the lecture we didn't use the _--debug_ flag after all as taking a look at the compiled sql file is the better way of debugging tests.

