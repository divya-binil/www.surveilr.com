# SQL Notebooks for maintable `surveilr` RSSDs SQL

## Introduction

The SQL Notebooks concept in the Resource Surveillance Commons (RSC) framework
allows you to generate, manage, and execute SQL code in a modular and organized
manner. This system is composed of various TypeScript modules that provide a
unified structure for handling SQL in a programmatic and type-safe way. The key
components of this system are defined in the `rssd.ts`, `code.ts`, and
`sqlpage.ts` files.

### Overview of Modules

1. **rssd.ts**: Defines the base classes and utilities for managing SQL
   notebooks, including functions for generating SQL statements and handling SQL
   emit contexts.

2. **code.ts**: Builds on the base classes from `rssd.ts`, adding functionality
   for handling code notebooks, which store and execute SQL and other code cells
   within a Resource Surveillance State Database (RSSD).

3. **sqlpage.ts**: Extends the notebook concept to SQLPage files, allowing for
   the management and upsertion of SQLPage content, which is used to serve web
   UI pages in the RSSD environment.

## Core Concepts

### 1. SurveilrSqlNotebook

The `SurveilrSqlNotebook` class in `rssd.ts` is the foundation of the SQL
Notebooks system. It provides core functionality for generating and managing SQL
code, including:

- **SQL Generation**: Methods to generate SQL statements in a type-safe manner.
- **Provenance Tracking**: Utilities to track the origin of SQL code, helping
  with debugging and traceability.
- **Idempotent SQL**: Support for generating SQL that can be safely executed
  multiple times, ensuring that it does not have unintended side effects.

### 2. TypicalCodeNotebook

The `TypicalCodeNotebook` class in `code.ts` extends `SurveilrSqlNotebook` to
manage code notebooks. These notebooks are designed to store and execute SQL and
other code cells, which are inserted into specific tables in the RSSD. Key
features include:

- **Cell Management**: Methods to define and manage individual code cells, which
  can be decorated to include metadata and configuration.
- **Kernel Upserts**: Support for upserting kernel records that manage the
  execution environment for the code cells.
- **Unified SQL Generation**: Integration of SQL and non-SQL code into a
  coherent system that can be executed or stored as part of the RSSD.

### 3. TypicalSqlPageNotebook

The `TypicalSqlPageNotebook` class in `sqlpage.ts` extends the notebook concept
to SQLPage files, which are used to generate and manage content for web UIs
within the RSSD. It includes:

- **Shell Configuration**: Utilities to manage the "shell" or wrapper around
  SQLPage content, including breadcrumbs and page titles.
- **Navigation Management**: Methods to generate and manage navigation routes
  within the SQLPage system.
- **File Upserts**: Functionality to generate SQL that upserts content into the
  `sqlpage_files` table, ensuring that SQLPage content is stored in the database
  rather than the file system.

## How the System Works Together

The SQL Notebooks system is designed to allow for modular and organized SQL
generation, which can be stored and managed within the RSSD. Here's how the
components work together:

1. **Defining Notebooks**: You define notebooks by subclassing
   `TypicalCodeNotebook` or `TypicalSqlPageNotebook`. These subclasses allow you
   to define methods that generate SQL or other code, which is then stored and
   managed within the RSSD.

2. **Generating SQL**: The methods within these subclasses can be decorated with
   metadata, such as `@cell` or `@shell`, which configures how the generated SQL
   should be managed. These methods can also be used to define SQL pages that
   are served as part of the web UI.

3. **Executing and Storing SQL**: The SQL generated by these notebooks can be
   executed directly, or it can be stored in the `sqlpage_files` or other
   relevant tables within the RSSD, ensuring that all SQL and content are
   managed consistently within the system.

4. **Unified System**: By leveraging the core concepts provided in `rssd.ts`,
   `code.ts`, and `sqlpage.ts`, you create a unified system where SQL
   generation, execution, and storage are all handled in a consistent,
   type-safe, and traceable manner.

## Example Usage

Here's a simple example to illustrate how you might use this system:

```typescript
import { TypicalSqlPageNotebook, shell, navigation } from "./sqlpage.ts";

class MyNotebook extends TypicalSqlPageNotebook {
  constructor() {
    super();
  }

  @shell({ path: "/index.sql" })
  index() {
    return this.SQL\`SELECT * FROM my_table;\`;
  }

  @navigation({
    caption: "Home",
    title: "Homepage",
    description: "The main page of the notebook"
  })
  index() {
    return this.SQL\`SELECT * FROM my_table;\`;
  }
}

// Create an instance of the notebook
const notebook = new MyNotebook();

// Generate SQL statements from the notebook methods
const sqlStatements = await TypicalSqlPageNotebook.SQL(notebook);

// Outputs SQL including the notebook-specific shell and navigation configurations
console.log(sqlStatements);
```

This example demonstrates how you can define a notebook, generate SQL, and
manage SQLPage content, all within the unified framework provided by the RSC
system.
