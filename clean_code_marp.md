---
marp: true
---

# Index
- [Meaningful names](#meaningful-names)
- [Functions](#functions)
- [Comments](#comments)
- [Formating code](#formating-code)

---

# Meaningful names

---

# Don't use single-letter names.
```py
x = ["apple", "banana", "pera"]
for i in x:
    print(i)
```
instead
```py
fruits = ["apple", "banana", "pera"]
for fruit in fruits:
    print(fruit)
```

---

# Don't use indexed variables such as a1,...,an
```py
file1 = "/home/hasser/names.txt"
file2 = "/home/hasser/fruits.txt"
df1 = pd.read_csv("20230023.csv")
df2 = pd.read_csv("2023catal.csv")
```
instead
```py
source_file = "/home/hasser/names.txt"
target_file = "/home/hasser/fruits.txt"
monthly_transactions = pd.read_csv("20230023.csv")
transactions_types = pd.read_csv("2023catal.csv")
```

consider using arrays or lists.

---

# Class names should be nouns, avoid common names and be explicit.
```py
class Data:
class Popup:
class FileValidator:
```
instead
```py
class TimeDimensionFile:
class FloatingAlertPopup:
class FileTypeValidator:
```

---

# Method names should be verbs.
```py
data()
response()
```
instead
```py
read_sql()
fetch_response()
```
*Always use a verb*

---

# Pick one noun per concept, one verb per action.
```py
class BrasilValidator:
class MexicoCheck:
class PanamaConfiguration:
get_file_data()
read_sql_data()
```
instead
```py
class BrasilValidator:
class MexicoValidator:
class PanamaValidator:
read_file()
read_sql()
```

---

# Solution domain(algorithms, patterns, math) or problem domain(business logic).
Use terms such as
```py
traverse_tree_bfs()
create_rsa_keys()
encode_text_utf8()

update_brasil_crm_configuration()
create_new_user()
send_first_client_alert()
```

---

# Add meaningful context enclosing in classes, files, functions or namespaces.
```py
class KOFBrasilModalidadesValidator:
class KOFBrasilFileConfiguration:
def get_brasil_alerts():
def get_brasil_modalidades():
```
instead, in `kof/brasil_crm`
```py
class ModalidadesValidator:
class FileConfiguration:
```

# Don't add context that is not useful and is repeated everywhere.

---

# Functions
Functions are the building blocks for programming, writing them properly is worth.

---

# Small(try to write functions shorter than 30 lines long).

---

# Don't repeat yourself.

---

# Do one thing.

---

# Write functions as describing a brief TO DO paragraph.

`./brasil_crm`
```py
def validate_crm(df):
    check_all_columns_in_file(df)
    check_not_duplicated_rutas(df)
    check_modalidades(df)
    check_hierarchies_match(df)
    check_all_rutas_in_file(df)
```

---

# Function divided into declarations, initializations and transformations are code smells.

```py
def clean_config_crm():
    country = "brasil"
    server = "https://172.58.52.12"
    port = "5000"
    target_schema = "raw_data"
    target_table = "crm_brasil"
    alertas_table = "brasil_alertas"
    do_something()
    do_something()
    do_something()
    do_something()
    do_something()
```

---

# Avoid switch statements, they always do N things. DO ONE THING!
```py
def clean():
    if file == "crm":
        do_crm()
    if file == "catalogo_funcional":
        do_cat_funcional()
    if file == "cat_modalidades":
        do_modalidades()
    ...
    ...
    ...
```

---

# The Command-Query Separation
+ Command: Method that modifies the state
+ Query: Method that returns data.

```js
const [searchText, setSearchText] = useState('');
```

---

# Have no side effects. Separate command and query methods
```py
if update_username("ernest", "ernesto"):
    do_other_thing()
```
instead
```py
if username_can_be_updated("ernest"):
    update_username("ernest", "ernesto")
```

---

# Don't mix levels of abstraction.
```py
def clean_crm_configuration(df):
    remove_duplicated_rutas(df)
    x = """
        select enorme
    """
```

---

# The less function arguments, the better.

---

# Prefer exceptions to returning error codes.

---

# Don't use output arguments, since this is confusing.

---

# Don't try to follow all the rules at once, refine it instead.

---

# Monadic functions
Almost always ask a question about the argument or transform it, but should never do both.

```py
if set_attribute("username", "robert"):
    do_something()
```
instead
```py
if not attribute_exists("username"):
    set_attribute("username", "robert")
```

---

# Dyadic arguments

Dyads have no natural ordering, therefore try to include the order in the function name.
```py
assert_equals(expected, actual)
```
instead
```py
assert_expected_equals_actual(expected, actual)
```

this avoid checking function definition.

---

# Comments

+ Avoid comments always.
+ Comments need to be refactor with code DOUBLE WORK.
+ **explain yourself in code**

---

# Good comments

+ Legal comments
+ Explaining regex.
+ Warning of consequences such as dangerous code, or long runs.
+ TODO comments

---

# Redundant comments
```py
# This function adds two numbers
def add(x, y):
    # Return the sum of x and y
    return x + y
```

---
# Position markers
```py
# Import libraries
import os
import sys

# Position marker for preprocessing
# ---------------------------------------------------------------

# Preprocess data
def preprocess_data(data):
    # Preprocessing steps
    pass

# Position marker for model training
# ---------------------------------------------------------------

# Train model
def train_model(data):
    # Model training steps
    pass

# Position marker for evaluation
# ---------------------------------------------------------------

# Evaluate model
def evaluate_model(model, test_data):
    # Evaluation steps
    pass
```
---

# Comments that can be replaced by code.
```py
# Uncomment when need total reprocessing
# initial_date = "2024-04-15"
# table_source = "historical_alerts"
do_stuff()
```
instead
```py
reprocess_all = True
initial_date = datetime.today()
table_source = "daily_alerts"
if reprocess_all:
    table_source = "historical_alerts"
    initial_date = get_min_db_value(table=table_source)
do_stuff()
```

---

# Author
```py
# Author: Israel
def function():
    pass
```
---

# Commented-out code.
# **DON'T COMMENT CODE, DELETE IT**

---

# Large comments.
Any body read those.

---

# Formating code

---

# Vertical blocks of code show relation, releated code should be vertically close, to avoid moving around code

```py
def main():
    data = load_data()

    preprocessed_data = preprocess_data(data)

    model = train_model(preprocessed_data)

    evaluation_result = evaluate_model(model, test_data)
```
instead
```py
def main():
    data = load_data()
    preprocessed_data = preprocess_data(data)
    model = train_model(preprocessed_data)
    evaluation_result = evaluate_model(model, test_data)
```

---

# Variables declarations close to their usage
```py
def do_stuff():
    best_crs_parameter = "epsg:5642"
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    do_stuff()
    convert_crs(best_parameter)
```

---

# Functions vertically close to other related functions

```py
def clean_crm_configuration():
def function1():
def function2():
def function3():
def function4():
def function5():
def create_crm_configuration():
```
instead
```py
def clean_crm_configuration():
def create_crm_configuration():
def function1():
def function2():
def function3():
def function4():
def function5():
```

---

# Important concepts should come first and details at the end
```py
def main():
    stuff1()
    stuff2()

def stuff1():
    pass

def stuff2()
    pass
```

---

# Separate functions parameters, variables and operators
```py
def function1(parameter1,parameter2)
    return parameter1+parameter2
```
instead
```py
def function1(parameter1, parameter2)
    return parameter1 + parameter2
```

---

# Don't align variables declarations, this might mean that a block of code do more than it should
```py
data   = "value"
config = "value"
table  = "brasil_alertas"
schema = "brasil"
port   = "5000"
server = "https://noc.reporteria.com"
PI     = 3.1416
```

---

# Invert the condition
```py
if exists_data:
    bla()
    ...
    bla()
else:
    print("not data")
```
instead
```py
if not exists_data:
    print("not data")
    return
bla()
...
bla()
```

---

# Search for code smells

Some python code smells

https://www.youtube.com/watch?v=zmWf_cHyo8s&list=PLC0nd42SBTaNILCJRCd4DvzNueVN_Sr5R&ab_channel=ArjanCodes

---

# Use team rules

+ formaters
+ linters
+ tests


python developer tools to automate and use same rules

https://github.com/ml-tooling/best-of-python-dev