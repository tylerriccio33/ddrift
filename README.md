`drift` analyzes the differences between data sources, i.e. how much has dataset a drifted from dataset b. The framework is engine agnostic. Each  engine is required to comply with simple abstract protocols in order to enable the standard reporting.

Engines Supported:
- DuckDB

Engines to be Supported:
- Postgres
- Polars
- Pandas

Install with `pip install drift` or (prefferebly) `uv pip install drift`

## Getting Started

Let's create 2 simple tables and compare them to one another. The fundamental question we're asking is "How much has table2 drifted from table1?"

```
import duckdb

with duckdb.connect() as con:
    con.execute("CREATE TABLE table1 (city VARCHAR, state VARCHAR)")
    con.execute(
        "INSERT INTO table1 VALUES ('New York', 'NY'), ('Los Angeles', 'CA'), ('Chicago', 'IL')"
    )

    con.execute("CREATE TABLE table2 (city VARCHAR, state VARCHAR)")
    con.execute(
        "INSERT INTO table2 VALUES ('New York', 'NY'), ('Phoenix', 'AZ'), ('Philadelphia', 'PA')"
    )

    sql = SQLComparator(df1="table1", df2="table2", con=con)
    sql.comp_freq(vars=("city", "state"))

    print(sql.results) # prints a list of result objects containing an in memory representation
```