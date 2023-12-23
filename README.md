
**This library integrates relational databases, vector databases, and embedding models by exposing vector databases as a virtual table (sqlite) or foreign data wrapper (PostgreSQL). This allows you to interact with vector databases using SQL queries.**

## Supported Vector Databases

- Pinecone
- FAISS (local)

## Supported Embedding Models

- OpenAI-Ada
- Sentence-Transformers (local)

## Supported Relational Databases

- PostgreSQL
- SQLite

## Demonstration

A demonstration notebook can be found on [Google Colab](https://colab.research.google.com/drive/1gY8nOv0Jww_M5bL3UTGePMqWI4QPc_Oi?usp=sharing).
This demonstration uses sqlite3, sentence-transformers, and FAISS. It is only a one-line change to use another embedder or vector database.

## Installation for PostgreSQL

To use this library with PostgreSQL, you must install the Multicorn extension for PostgreSQL. Note that it is only compatible with PostgreSQL 12.

```bash
pgxn install multicorn
```

### Usage

After installing Multicorn, set up a foreign server and a foreign table in PostgreSQL as follows:
```sql
-- Create a new foreign server
CREATE SERVER my_srv FOREIGN DATA WRAPPER multicorn OPTIONS (
    wrapper 'vsql.relational_dbs.postgres.VectorForeignDataWrapper'
);

-- Create a foreign table
CREATE FOREIGN TABLE vt (
    id integer, query text, similarity numeric
) SERVER my_srv;
```

After this refer to the [example notebook](https://colab.research.google.com/drive/1gY8nOv0Jww_M5bL3UTGePMqWI4QPc_Oi?usp=sharing) to see how to use the foreign table and keep it synchronized with a base table.

### Configuration

Currently, to use PostgreSQL with a different embedder and VDB than the default Sentence-Transformers and FAISS, you will need to modify a line in the source code in `vsql/relational_dbs/postgres/__init__.py`.

