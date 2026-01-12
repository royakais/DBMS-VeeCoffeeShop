# Vee Coffee Shop DBMS Project

This repository contains the **Database Management System (DBMS) project for Vee Coffee Shop**, designed to manage a modern coffeehouse's operations.  
The system centralizes **sales, inventory, employees, suppliers, orders, and deliveries** into a single structured database, improving workflow, data accuracy, and decision-making.

---

## 🚀 Project Overview

The Vee Coffee Shop database system supports:

- **Customer Management:** Loyalty programs, unique customer IDs, phone numbers, and addresses. Each customer can place multiple orders.  
- **Product Cataloging:** Products have unique IDs, names, categories, and prices. Products are part of orders, stocked in branches, and supplied by suppliers.  
- **Branch Operations:** 16 branches, each with a unique ID, location, opening date, size, inventory, employees, deliveries, and a manager.  
- **Employee Management:** Employee details (SSN, name, position, salary, hire date), assigned to one branch.  
- **Order Processing:** Orders have unique IDs, date, total amount, status, and payment method. Each order belongs to a customer and contains multiple products.  
- **Inventory Management:** Tracks product quantities and restock dates per branch.  
- **Supplier Coordination:** Suppliers with unique IDs, names, contact info. Supplies products via deliveries.  
- **Delivery Tracking:** Deliveries linked to suppliers and branches, with status and costs.

---

## 📝 ER Diagram & Relational Schema
<img width="702" height="596" alt="image" src="https://github.com/user-attachments/assets/e492a84d-e10b-40bd-99f0-1480dc06d0d7" />


**Key Entities:**

- **Customer:** Customer_id, Name, Phone, Address  
- **Order:** Order_Id, OrderDate, TotalAmount, Status, PaymentMethod, Customer_id*  
- **Product:** Product_id, Name, Category, Price, Order_Id*, Branch_id*  
- **Supplier:** Supplier_id, Name, Email, Phone  
- **Supplies:** Product_id*, Supplier_Id*, SupplyAmount  
- **Branch:** Branch_id, Address, Size, Opening_date, Manager_id*  
- **Branch_location:** Branch_id*, Location  
- **Employee:** SSN, FirstName, LastName, Position, Salary, Hire_date, Branch_id*  
- **Inventory:** Quantity, Branch_id*, Restock_date  
- **Delivery:** Delivery_id, Arrival_date, Delivery_cost, Status, Supplier_id*  
- **Receive:** Delivery_id*, Branch_id*, Received_quantity

**Functional Dependencies:**

- `Customer(ID) → {name, address, phone}`  
- `OrderID → {OrderDate, Amount, PaymentMethod, Status, Customer_ID}`  
- `Product(ID) → {name, price, category, Order_Id, Branch_ID}`  
- `(SupplierID, ProductID) → {SupplyAmount}`  
- `Employee(SSN) → {FirstName, LastName, Position, Salary, Hire_date, BranchID}`  
- `(BranchID, Location) → {BranchID, Location}`  
- `(BranchID, Quantity) → {Restock_date}`  

> The database is normalized to **3NF** to ensure consistency and avoid redundancy.

---

## 💾 Sample SQL Table Creation

```sql
CREATE SCHEMA IF NOT EXISTS Vdb;
USE Vdb;

CREATE TABLE IF NOT EXISTS customer (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    address TEXT
);

CREATE TABLE IF NOT EXISTS orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2) CHECK (total_amount >= 0),
    payment_method ENUM('Cash','Online Payment') NOT NULL,
    status ENUM('paid','on hold','Cancelled') DEFAULT 'paid',
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);

CREATE TABLE IF NOT EXISTS supplier (
    supplier_id INT AUTO_INCREMENT PRIMARY KEY,
    Sname VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    phone VARCHAR(20)
);

CREATE TABLE IF NOT EXISTS employee (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    SSN VARCHAR(30) UNIQUE,
    branch_id INT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    position VARCHAR(50) NOT NULL,
    salary DECIMAL(10,2) CHECK (salary > 0),
    hire_date DATE
);
