# 📦 Inventory Management Dashboard

A full-stack **Inventory Management and Supply Chain Dashboard** built using **Python, Streamlit, and MySQL**. The application enables businesses to monitor inventory, manage products, track stock movements, and handle supplier reorders through an interactive web interface.

---

## 🚀 Features

### 📊 Dashboard Analytics

* View total suppliers, products, and product categories.
* Monitor total sales value for the last three months.
* Track total restock value.
* Identify products below their reorder level.
* View supplier contact details.
* Monitor product stock levels.
* Display products requiring immediate reorder.

### ⚙️ Operational Tasks

* ➕ Add new products to inventory.
* 📜 View complete inventory history of any product.
* 🛒 Place supplier reorders.
* 📦 Mark reorders as received and automatically update inventory.

---

## 🛠️ Tech Stack

| Technology      | Purpose               |
| --------------- | --------------------- |
| Python          | Backend Logic         |
| Streamlit       | Web Dashboard         |
| MySQL           | Database              |
| MySQL Connector | Database Connectivity |
| Pandas          | Data Processing       |

---

## 🗂️ Project Structure

```text
Inventory-Management-Dashboard/
│
├── app.py                 # Streamlit application
├── db_functions.py        # Database helper functions
├── requirements.txt
├── README.md
└── database/
    ├── schema.sql
    ├── procedures.sql
    └── sample_data.sql
```

---

## 🗄️ Database Design

The project uses a relational database consisting of the following entities:

* **Products**
* **Suppliers**
* **Stock Entries**
* **Reorders**
* **Inventory History (View)**

Relationships are managed using primary and foreign keys to ensure data integrity.

---

## 💡 SQL Concepts Demonstrated

This project showcases practical SQL concepts including:

* CRUD Operations
* INNER JOIN
* Aggregate Functions (`COUNT`, `SUM`, `ROUND`)
* `GROUP BY`
* `ORDER BY`
* Date Functions (`CURDATE`, `DATE_SUB`)
* Views
* Stored Procedures
* Transactions
* Foreign Keys
* Data Validation

---

## 📌 Application Workflow

```text
User
   │
   ▼
Streamlit Dashboard
   │
   ▼
Python Database Layer
   │
   ▼
MySQL Database
   │
   ├── Products
   ├── Suppliers
   ├── Stock Entries
   ├── Reorders
   ├── Stored Procedures
   └── Views
```

---

## 📸 Dashboard Modules

### Basic Information

Displays:

* Inventory KPIs
* Supplier Information
* Product Stock Status
* Reorder Alerts

### Operational Tasks

Supports:

* Add Product
* Product Inventory History
* Place Reorder
* Receive Reorder

---

## ▶️ Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/inventory-management-dashboard.git
cd inventory-management-dashboard
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure MySQL

Create a MySQL database and import:

* `schema.sql`
* `procedures.sql`
* `sample_data.sql`

Update the database credentials in `db_functions.py`.

```python
host = "localhost"
user = "root"
password = "your_password"
database = "python_sql"
```

### 4. Run the application

```bash
streamlit run app.py
```

---

## 📈 Future Improvements

* User Authentication
* Role-Based Access Control
* Sales Forecasting using Machine Learning
* Automatic Low-Stock Notifications
* Supplier Performance Dashboard
* Barcode Scanner Integration
* Interactive Charts and Visualizations
* Export Reports (PDF/Excel)
* Cloud Deployment (AWS/Azure)

---

## 🎯 Learning Outcomes

Through this project, I gained hands-on experience with:

* Building interactive dashboards using Streamlit.
* Integrating Python applications with MySQL.
* Designing relational databases.
* Implementing stored procedures and views.
* Writing optimized SQL queries.
* Managing transactions and inventory workflows.
* Structuring applications with a clean separation between UI and database logic.

---

## 🤝 Contributing

Contributions, suggestions, and feature requests are welcome. Feel free to fork the repository and submit a pull request.

---

## 📄 License

This project is intended for educational and portfolio purposes.
