# 📦 Inventory & Supply Chain Management Dashboard

A full-stack **Inventory Management and Supply Chain Dashboard** built using **Python, Streamlit, and MySQL**.

The application provides a simple, business-friendly interface for monitoring inventory, managing products, tracking stock movements, placing supplier reorders, and receiving incoming stock **without requiring users to write SQL queries**.

> 🎯 **Core Idea:** Convert common inventory and supply-chain database operations into an interactive dashboard while keeping the core data management and business workflows inside MySQL.

---

## 📌 Problem Statement

Inventory teams often work with data stored inside relational databases, but not every user interacting with that data knows SQL.

For example, a warehouse manager may need to:

- Check current stock levels
- Identify products below their reorder level
- View supplier information
- Review a product's inventory history
- Place a supplier reorder
- Mark incoming stock as received
- Monitor sales and restock values

Traditionally, these operations may require manually running SQL queries.

This project solves that problem by providing a **Streamlit-based UI on top of a MySQL database**, allowing users to perform these operations through forms, dropdowns, tables, and buttons.

---

# 🚀 Key Features

## 📊 Dashboard Analytics

The dashboard provides operational information retrieved directly from the MySQL database.

### Key Performance Indicators

- 👥 Total number of suppliers
- 📦 Total number of products
- 🗂️ Total product categories
- 💰 Total sales value over the last 3 months
- 📈 Total restock value over the last 3 months
- ⚠️ Products below their reorder level without an existing pending reorder

### Detailed Information

The dashboard also displays:

- Supplier contact information
- Product and supplier relationships
- Current product stock levels
- Product reorder levels
- Products currently requiring reorder

The KPI queries use SQL aggregation, joins, filtering, subqueries, and date-based calculations.

---

# ⚙️ Operational Tasks

The dashboard provides four major operational workflows.

| Operation | Description |
|---|---|
| ➕ **Add New Product** | Adds a product along with its initial shipment and stock entry |
| 📜 **Product History** | Displays the inventory history of a selected product |
| 🛒 **Place Reorder** | Creates a supplier reorder for a selected product |
| 📦 **Receive Reorder** | Updates reorder status, increases stock, and records the incoming inventory |

The Streamlit UI exposes these workflows through forms, dropdowns, number inputs, and action buttons.

---

# 🏗️ System Architecture

The application follows a simple layered architecture:

```text
                    ┌──────────────────────┐
                    │        User          │
                    │ Manager / Operations │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    Streamlit UI      │
                    │                      │
                    │ • Dashboard          │
                    │ • Tables             │
                    │ • Forms              │
                    │ • Dropdowns          │
                    │ • Action Buttons     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Python DB Layer    │
                    │   db_functions.py    │
                    │                      │
                    │ • DB Connection      │
                    │ • SQL Execution      │
                    │ • Procedure Calls    │
                    │ • Commit Handling    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │        MySQL         │
                    │                      │
                    │ • Products           │
                    │ • Suppliers          │
                    │ • Stock Entries      │
                    │ • Shipments          │
                    │ • Reorders           │
                    │ • Views              │
                    │ • Stored Procedures  │
                    └──────────────────────┘
```

### Layer Responsibilities

| Layer | Responsibility |
|---|---|
| **Streamlit** | User interface, navigation, forms, tables and input validation |
| **Python** | Database connectivity and application-level database functions |
| **MySQL** | Data storage, reporting queries, views, stored procedures and transactions |

This separation keeps the UI code independent from most of the database execution logic.

---

# 🗄️ Database Design

The application uses a relational database designed around inventory and supply-chain operations.

```text
                         ┌──────────────┐
                         │  Suppliers   │
                         └──────┬───────┘
                                │
                                │ supplier_id
                                ▼
                         ┌──────────────┐
                         │   Products   │
                         └──────┬───────┘
                                │
               ┌────────────────┼────────────────┐
               │                │                │
               ▼                ▼                ▼
       ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
       │ Stock Entries│ │  Shipments   │ │   Reorders   │
       └──────────────┘ └──────────────┘ └──────────────┘
```

## Core Entities

| Entity | Purpose |
|---|---|
| `products` | Stores product name, category, price, stock quantity, reorder level and supplier reference |
| `suppliers` | Stores supplier and contact information |
| `stock_entries` | Records inventory movements such as sales and restocks |
| `shipments` | Records incoming shipments and quantities received |
| `reorders` | Tracks supplier reorder requests, quantities, dates and status |
| `product_inventory_history` | SQL view combining shipment and stock-entry history |

Primary and foreign-key relationships connect the entities and maintain the relational structure of the inventory system.

---

# 🧠 SQL & Database Concepts

This project goes beyond basic CRUD operations and demonstrates practical database concepts.

## Basic SQL

- `SELECT`
- `INSERT`
- `UPDATE`
- `WHERE`
- `ORDER BY`
- `DISTINCT`

## Joins

The project uses `INNER JOIN` to combine information across related tables.

Example:

```sql
SELECT
    p.product_name,
    s.supplier_name,
    p.stock_quantity,
    p.reorder_level
FROM products p
JOIN suppliers s
    ON p.supplier_id = s.supplier_id
ORDER BY p.product_name ASC;
```

This allows the dashboard to display product information together with supplier details.

---

# 📊 SQL Aggregation & Reporting

The dashboard calculates business-level metrics using aggregate functions such as:

- `COUNT()`
- `SUM()`
- `ROUND()`

For example, total sales value is calculated using:

```text
Sales Quantity × Product Price
```

The query joins `stock_entries` with `products` and filters records based on inventory movement type and date.

---

# 📅 Date-Based Analysis

The dashboard calculates recent sales and restock values using MySQL date functions.

The project uses:

- `DATE_SUB()`
- `MAX()`
- Date comparisons
- Rolling three-month filtering

Conceptually:

```text
Latest Inventory Date
        ↓
Subtract 3 Months
        ↓
Filter Recent Stock Entries
        ↓
Calculate Sales / Restock Value
```

This allows the dashboard to generate time-based inventory KPIs directly from the database.

---

# 👁️ SQL View: Product Inventory History

The project uses a reusable SQL view:

```text
product_inventory_history
```

The view combines records from:

- `shipments`
- `stock_entries`

using `UNION ALL`.

Conceptually:

```text
             Product History
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
      Shipments       Stock Entries
          │                 │
          └────────┬────────┘
                   │
                   ▼
              UNION ALL
                   │
                   ▼
      product_inventory_history
```

This gives the application a unified source for displaying a product's inventory activity.

---

# ⚙️ Stored Procedures

A major part of the project is moving multi-step business operations into MySQL stored procedures.

The project currently uses two important procedures.

---

## 1. `AddNewProductManualID`

Adding a new product requires changes across multiple tables.

The stored procedure performs:

```text
Generate Product ID
        ↓
Insert Product
        ↓
Create Shipment Record
        ↓
Create Initial Stock Entry
```

This means the application does not need to separately execute every database operation from the UI.

---

## 2. `MarkReorderAsReceived`

Receiving inventory is a multi-step operation.

The procedure performs:

```text
Get Product + Quantity
        ↓
Get Supplier
        ↓
Update Reorder Status
        ↓
Increase Product Stock
        ↓
Create Shipment Record
        ↓
Create Restock Entry
        ↓
Commit Transaction
```

This keeps related inventory changes together.

---

# 🔐 Transactions

The `MarkReorderAsReceived` procedure uses a database transaction.

The operation involves multiple changes:

1. Updating the reorder status
2. Increasing product stock
3. Creating a shipment record
4. Creating a stock-entry record

These operations are logically connected.

Using a transaction ensures that they are handled as one database operation rather than treating each update as an unrelated action.

```text
START TRANSACTION
       │
       ├── Update Reorder
       │
       ├── Update Product Stock
       │
       ├── Insert Shipment
       │
       ├── Insert Stock Entry
       │
       ▼
     COMMIT
```

For a production implementation, explicit rollback and stronger transaction-level error handling should also be added.

---

# 🔄 Application Workflows

## ➕ Add New Product

```text
User
 │
 ▼
Enter Product Details
 │
 ▼
Streamlit Form Validation
 │
 ▼
Python Database Function
 │
 ▼
AddNewProductManualID()
 │
 ▼
MySQL
 ├── Insert Product
 ├── Insert Shipment
 └── Insert Stock Entry
 │
 ▼
Commit
 │
 ▼
Success Message
```

---

## 📜 Product Inventory History

```text
User selects Product
        │
        ▼
Python retrieves product_id
        │
        ▼
product_inventory_history
        │
        ├── Shipment Records
        │
        └── Stock Entry Records
        │
        ▼
Pandas DataFrame
        │
        ▼
Streamlit Table
```

---

## 🛒 Place Reorder

```text
User selects Product
        │
        ▼
Enter Reorder Quantity
        │
        ▼
Input Validation
        │
        ▼
INSERT into reorders
        │
        ▼
Status = Ordered
        │
        ▼
Commit
```

---

## 📦 Receive Reorder

```text
User selects Reorder
        │
        ▼
Python calls stored procedure
        │
        ▼
MarkReorderAsReceived()
        │
        ▼
START TRANSACTION
        │
        ├── Update reorder status
        ├── Increase product stock
        ├── Insert shipment
        └── Insert restock entry
        │
        ▼
COMMIT
        │
        ▼
Inventory Updated
```

---

# 🧩 Project Structure

```text
inventory-management-dashboard/
│
├── app.py
├── db_functions.py
├── main.py
├── pyproject.toml
├── uv.lock
├── Python UI for SQL Databases.sql
├── README.md
│
├── data/
├── .gitignore
└── .python-version
```

## File Responsibilities

| File | Responsibility |
|---|---|
| `app.py` | Streamlit application, navigation, forms, validation and dashboard rendering |
| `db_functions.py` | MySQL connection, SQL execution and stored-procedure calls |
| `Python UI for SQL Databases.sql` | SQL reporting queries, view definitions, stored procedures and database workflows |
| `pyproject.toml` | Project metadata and dependencies |
| `uv.lock` | Locked dependency versions |
| `main.py` | Minimal standalone Python entry point |
| `data/` | Project data resources |
| `.gitignore` | Files excluded from version control |
| `.python-version` | Python version configuration |

---

# 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| 🐍 **Python** | Application and database-access logic |
| 🎈 **Streamlit** | Interactive dashboard and web UI |
| 🗄️ **MySQL** | Relational database and business-logic layer |
| 🔌 **MySQL Connector/Python** | Python-to-MySQL connectivity |
| 🐼 **Pandas** | Query-result processing and table rendering |
| 📝 **SQL** | Queries, joins, views, stored procedures and transactions |

---

# 📦 Dependencies

The project uses:

```text
Python >= 3.13
Streamlit
Pandas
MySQL Connector/Python
```

Dependencies are defined in `pyproject.toml`.

---

# ▶️ Getting Started

## Prerequisites

Make sure you have:

- Python 3.13+
- MySQL Server
- Git

---

## 1. Clone the Repository

```bash
git clone https://github.com/imdwnyn/inventory-management-dashboard.git
cd inventory-management-dashboard
```

---

## 2. Install Dependencies

Using the project configuration:

```bash
pip install -e .
```

Or install the dependencies directly:

```bash
pip install mysql-connector-python pandas streamlit
```

---

## 3. Set Up MySQL

Create a MySQL database named:

```text
python_sql
```

Then execute the SQL contained in:

```text
Python UI for SQL Databases.sql
```

The SQL file contains:

- Reporting queries
- `product_inventory_history` view
- `AddNewProductManualID` stored procedure
- `MarkReorderAsReceived` stored procedure

---

## 4. Configure Database Credentials

Update the MySQL connection inside:

```text
db_functions.py
```

Example:

```python
mysql.connector.connect(
    host="localhost",
    user="root",
    password="YOUR_PASSWORD",
    database="python_sql"
)
```

### ⚠️ Security Warning

**Do not commit real database credentials to GitHub.**

For production deployment, credentials should be stored using:

- Environment variables
- Streamlit secrets
- A secure secret manager

---

## 5. Run the Application

```bash
streamlit run app.py
```

Streamlit will provide a local URL where the dashboard can be opened in your browser.

---

# 🧪 Example Business Scenario

Consider a warehouse manager who notices that a product has fallen below its reorder level.

Instead of manually writing SQL queries, they can:

```text
Open Dashboard
      ↓
Check Products Needing Reorder
      ↓
Open Operational Tasks
      ↓
Select Place Reorder
      ↓
Select Product
      ↓
Enter Quantity
      ↓
Place Reorder
      ↓
Supplier Order Recorded
      ↓
Stock Arrives
      ↓
Select Receive Reorder
      ↓
Mark Reorder as Received
      ↓
Inventory Automatically Updated
      ↓
Shipment + Restock History Recorded
```

This demonstrates the main purpose of the project:

> **Turning database-level inventory operations into simple user-facing workflows.**

---

# 🧱 Design Decisions

## Database-Level Business Logic

Multi-step inventory operations are handled through stored procedures rather than putting every database operation directly into the Streamlit UI.

This provides a cleaner separation between:

```text
Presentation Layer
        ↓
Python Database Layer
        ↓
Database Business Logic
```

---

## Reusable Inventory History

Instead of making the application separately query shipments and stock entries, the `product_inventory_history` view combines them into a reusable reporting layer.

This simplifies the Python-side retrieval logic.

---

## Python Database Abstraction

Database-related operations are grouped inside:

```text
db_functions.py
```

Examples include:

```python
connect_to_db()
get_basic_info()
get_additonal_tables()
get_categories()
get_suppliers()
get_all_products()
get_product_history()
place_reorder()
mark_reorder_as_received()
```

This prevents the Streamlit UI from becoming tightly coupled to every individual SQL query.

---

# 🤖 Future Improvement: Agentic AI & Natural Language SQL

A major future improvement is to introduce a **Natural Language → SQL interface powered by Agentic AI with Human-in-the-Loop (HITL)**.

Currently, users interact with the database through predefined dashboard operations.

A future version could allow users to simply ask questions in natural language.

For example:

```text
"Which products are below their reorder level
and do not already have a pending reorder?"
```

The system could then:

```text
Natural Language Query
        ↓
AI / SQL Agent
        ↓
Understand User Intent
        ↓
Retrieve Database Schema
        ↓
Generate SQL
        ↓
Validate SQL
        ↓
Human Review / Approval
        ↓
Execute Query
        ↓
Validate Results
        ↓
Return Answer
```

---

## 🧠 Text-to-SQL

The AI agent could use the existing database schema as context to generate SQL queries automatically.

For example:

### User

```text
Show me all products that need to be reordered.
```

### AI-generated SQL

```sql
SELECT
    product_name,
    stock_quantity,
    reorder_level
FROM products
WHERE stock_quantity <= reorder_level;
```

The user would not need to know SQL syntax.

---

## 🔄 Agentic Text-to-SQL Architecture

A future architecture could look like:

```text
                         User
                           │
                           ▼
                 Natural Language Query
                           │
                           ▼
                  ┌─────────────────┐
                  │   AI Planner    │
                  └────────┬────────┘
                           │
                           ▼
                   Schema Retrieval
                           │
                           ▼
                  ┌─────────────────┐
                  │ Text-to-SQL     │
                  │     Agent       │
                  └────────┬────────┘
                           │
                           ▼
                    SQL Generation
                           │
                           ▼
              ┌────────────────────────┐
              │ Human-in-the-Loop      │
              │                        │
              │ Review SQL             │
              │ Approve / Modify / Deny│
              └───────────┬────────────┘
                          │
                      Approved
                          │
                          ▼
                   SQL Validation
                          │
                          ▼
                    MySQL Database
                          │
                          ▼
                    Query Execution
                          │
                          ▼
                   Result Validation
                          │
                          ▼
                   AI Explanation
                          │
                          ▼
                    Streamlit UI
```

---

# 🛡️ Human-in-the-Loop Safety

Allowing an LLM to directly execute generated SQL can be risky, especially when the query modifies data.

Potential problems include:

- Incorrect SQL generation
- Incorrect interpretation of user intent
- Unintended data modification
- Destructive SQL operations
- Unauthorized data access
- Incorrect business decisions based on generated queries

A **Human-in-the-Loop approval layer** can provide a safety checkpoint.

For example:

```text
User Request
     ↓
AI generates SQL
     ↓
Display generated SQL
     ↓
Human reviews query
     ↓
┌───────────────┐
│ Approve       │
│ Modify        │
│ Reject        │
└───────┬───────┘
        ↓
   Execute Safely
```

A useful policy would be:

```text
SELECT queries
     ↓
Lower-risk / optional approval

INSERT / UPDATE / DELETE
     ↓
Explicit human approval required
```

---

# 🔒 AI Query Safety Layer

The future AI system could introduce multiple safeguards before executing generated SQL:

- Schema-aware SQL generation
- SQL syntax validation
- Query classification
- Read-only mode
- SQL allowlisting
- Permission-based tool access
- Query timeout limits
- Result-size limits
- Human approval for write operations
- Transaction-based execution
- Audit logging
- Database role restrictions

This would prevent the AI agent from having unrestricted access to the database.

---

# 🧠 Agentic AI Capabilities

The system could eventually move beyond simple Text-to-SQL and support multiple specialized tools or agents.

```text
                         User
                           │
                           ▼
                    AI Orchestrator
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
        SQL Agent     Analytics Agent  Inventory Agent
             │             │             │
             ▼             ▼             ▼
          MySQL       Pandas / SQL    Reorder Logic
             │             │             │
             └─────────────┼─────────────┘
                           ▼
                    Result Validator
                           │
                           ▼
                    Human Approval
                           │
                           ▼
                     Final Response
```

Potential capabilities include:

- Natural-language database querying
- Conversational inventory exploration
- Automatic inventory analysis
- Low-stock reasoning
- Intelligent reorder recommendations
- Supplier comparisons
- Sales trend analysis
- Automated report generation
- Query explanation in plain English
- Controlled database tool calling
- AI-assisted operational workflows

---

# 💬 Example Future Interaction

```text
User:

"Which products need to be reordered?
Also tell me which supplier provides each product."
```

```text
                ↓

             AI Agent

                ↓

        Generate SQL Query

                ↓

         Human Approval

                ↓

          Execute Query

                ↓

        Validate Results

                ↓

             AI Response
```

Example response:

```text
I found 5 products below their reorder levels.

Product       Stock   Reorder Level   Supplier
------------------------------------------------
Product A       8          20          Supplier X
Product B       5          15          Supplier Y
Product C      12          30          Supplier X

These products may require reordering.
```

The same architecture could eventually allow controlled actions:

```text
User:
"Create a reorder for Product A with quantity 100."

        ↓

AI identifies required tool/action

        ↓

Generate proposed database operation

        ↓

Human Approval

        ↓

Execute controlled reorder function

        ↓

Confirm result
```

This would transform the current dashboard into a **natural-language inventory assistant** while still maintaining database-level safety and human oversight.

---

# 🚀 Future Improvements

## 🔐 Security & Access Control

- [ ] Move credentials to environment variables or Streamlit secrets
- [ ] User authentication
- [ ] Role-Based Access Control
- [ ] Admin / Manager / Warehouse Staff roles
- [ ] Audit logging
- [ ] Database-level permission management

---

## 📊 Advanced Analytics

- [ ] Inventory trend charts
- [ ] Sales trend analysis
- [ ] Restock trend analysis
- [ ] Supplier performance metrics
- [ ] Inventory turnover analysis
- [ ] Inventory aging analysis
- [ ] Product demand analysis

---

## 🤖 AI & Agentic Systems

- [ ] Natural Language → SQL interface
- [ ] LLM-powered Text-to-SQL
- [ ] Schema-aware SQL generation
- [ ] Human-in-the-Loop SQL approval
- [ ] Read-only AI query mode
- [ ] AI-powered inventory analysis
- [ ] Intelligent reorder recommendations
- [ ] Conversational database assistant
- [ ] Tool-calling agents for controlled database operations
- [ ] AI-generated reports and insights
- [ ] AI-based demand forecasting
- [ ] AI-assisted supplier analysis

---

## 🔔 Automation

- [ ] Automatic low-stock alerts
- [ ] Automated reorder recommendations
- [ ] Scheduled reports
- [ ] Supplier delivery tracking
- [ ] Email notifications
- [ ] Automated inventory monitoring

---

## 🏭 Production Readiness

- [ ] Cloud deployment
- [ ] Database connection pooling
- [ ] REST API layer
- [ ] Automated testing
- [ ] CI/CD pipeline
- [ ] Structured logging
- [ ] Query validation and execution safeguards
- [ ] CSV / Excel / PDF exports
- [ ] Barcode scanner integration
- [ ] Database backup and recovery strategy

---

# ⚠️ Current Limitations & Improvement Areas

This project is primarily a **learning and portfolio implementation**. Before using it in a production environment, several areas should be improved.

## 1. Database Credentials

Database credentials should not be stored directly inside Python source code.

### Better approach

```text
Environment Variables
        or
Streamlit Secrets
        ↓
Python Application
        ↓
MySQL
```

---

## 2. Manual ID Generation

The current SQL implementation generates IDs using logic similar to:

```sql
MAX(id) + 1
```

This can create race conditions when multiple users insert records simultaneously.

### Better approach

Use MySQL:

```sql
AUTO_INCREMENT
```

or another database-managed ID-generation strategy.

---

## 3. Reorder Status Handling

The application distinguishes between reorder states such as:

```text
Ordered
Received
```

The reorder retrieval logic should be tightened so that the **Receive Reorder** interface only displays reorders that are actually eligible to be received.

---

## 4. Error Handling

The current application uses basic exception handling.

A production version should introduce:

- Structured logging
- More specific exception handling
- Database rollback on failed transactions
- User-friendly error messages
- Server-side validation

---

## 5. Testing

Automated tests can be added for:

- Product creation
- Reorder creation
- Reorder receiving
- Stock updates
- Inventory history
- KPI calculations
- Database constraints
- Stored procedures

---

# 💡 Key Learning Outcomes

Through this project, I gained practical experience with:

### Python

- Python application development
- Database connectivity
- Modular database helper functions
- Exception handling
- Pandas-based data processing

### SQL

- Relational database design
- CRUD operations
- Primary and foreign keys
- `INNER JOIN`
- Aggregate functions
- Subqueries
- Date functions
- Views
- `UNION ALL`
- Stored procedures
- Transactions
- Database-side business logic

### Streamlit

- Interactive dashboard development
- Sidebar navigation
- Forms
- Select boxes
- Number inputs
- Buttons
- KPI metrics
- Data tables
- User input validation

### Software Design

- Separation of UI and database logic
- Database-driven application architecture
- Multi-step transactional workflows
- Business workflow modelling

---

# 🎯 What Makes This Project Interesting?

The project is not just a basic CRUD application.

It demonstrates how a **database-backed business application** can combine:

```text
                Python
                  │
                  ▼
             Streamlit UI
                  │
                  ▼
          Database Abstraction
                  │
                  ▼
                MySQL
             ┌────┼────┐
             │    │    │
             ▼    ▼    ▼
           Views  SPs  Transactions
             │    │    │
             └────┼────┘
                  ▼
          Inventory Workflows
```

The important part is that the system does not simply display database records. It models actual inventory operations such as:

- Stock monitoring
- Product management
- Supplier management
- Reordering
- Receiving inventory
- Recording stock movements
- Maintaining inventory history

The planned **Agentic AI + Text-to-SQL + HITL** layer can further extend this architecture by allowing users to interact with the same database through natural language while keeping database operations controlled and auditable.

---

# 📚 Concepts Covered

This repository can be used to practise:

- Relational database design
- SQL joins
- SQL aggregation
- Nested queries
- Date-based reporting
- Database views
- `UNION ALL`
- Stored procedures
- Transactions
- Python/MySQL integration
- Streamlit application development
- Pandas data processing
- Inventory management workflows
- Supply-chain data modelling
- Database abstraction
- Agentic AI architecture
- Text-to-SQL
- Human-in-the-Loop systems
- AI tool calling
- Controlled database automation

---

# 👤 Author

**Dwinayan**

GitHub: [@imdwnyn](https://github.com/imdwnyn)

---

# ⭐ Acknowledgement

This project was developed as a hands-on implementation of a **Python-driven UI for advanced SQL database operations**, with a focus on inventory management and supply-chain workflows.

The main goal was to understand how a relational database, backend logic, and user-facing dashboard can work together to solve a practical business problem.

The planned AI extension explores how the same foundation can evolve into a **natural-language database assistant using Text-to-SQL, Agentic AI, tool calling, and Human-in-the-Loop controls**.

---

# 📄 License

This project is intended for **educational and portfolio purposes**.
