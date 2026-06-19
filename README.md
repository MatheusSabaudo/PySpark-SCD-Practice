# PySpark Incremental Merge

A PySpark exercise that merges a target dataset with an incremental (change) feed to produce a clean, deduplicated "latest state" table — the core pattern behind incremental loads and upserts in a data lake / warehouse.

## What it does

Given two order datasets — a base snapshot and an incremental batch of new/updated records — the notebook:

- Enforces a shared schema with explicit types (`StringType`, `DoubleType`, `DateType`, `TimestampType`).
- Standardizes string columns (trimming whitespace, normalizing empty values).
- Stacks both datasets with `unionByName`, tagging rows with an `is_incremental` flag.
- Uses a `Window` partitioned by `order_id` with `row_number()` to keep the most recent version of each order, so incremental records win over stale ones.
- Returns the merged, deduplicated result and demonstrates filtering it (e.g. orders with `status = "shipped"`).

## Key concepts demonstrated

- Schema definition and enforcement with `StructType` / `StructField`
- `unionByName` for combining datasets safely by column name
- Window functions (`Window.partitionBy`, `row_number`) for deduplication / upsert logic
- Column standardization with `pyspark.sql.functions`

## Tech stack

- Python 3.9+
- Apache Spark (PySpark) 3.5+

## Getting started

```bash
git clone https://github.com/MatheusSabaudo/pyspark-incremental-merge.git
cd pyspark-incremental-merge
pip install -r requirements.txt
jupyter notebook incremental_merge.ipynb
```

The notebook builds its sample data inline, so no external dataset is required.

## License

[MIT](LICENSE) © Matheus Sabaudo
