# PL/SQL Oracle Database Capstone Project

# 📋 What’s This Project About? 🤔
This project is a Supplier Payment and Delivery Monitoring System built with an Oracle database and PL/SQL. It automates the tracking of supplier orders, delivery dates, and payment schedules. The system automatically flags delayed shipments and overdue payments using database triggers and procedures. It also generates performance reports to help businesses identify reliable suppliers. The goal is to eliminate manual errors, ensure timely payments, and improve overall supply chain efficiency.

**Name:** NTAKIRUTIMANA Kevin

**ID:** 29259

**Project title:** Supplier Payment and Delivery Monitoring System

**Group:** Wednesday

# PHASE I: Problem Statement & Presentation

# 1. Problem definition

•	Late Delivery 😣: A key supplier's shipment is late, but no one knows until it's already a problem. This stops production and hurts customer trust, all because of manual tracking.

•	Payment Confusion 💸: Finance gets an invoice, but is it for a delivered order? Is it even overdue? Without a clear system, payments get delayed, and suppliers get frustrated.

•	No Supplier Report Card 🔍: Is Supplier A always on time? Is Supplier B often late? There's no easy way to track performance, so it's hard to know who your reliable partners are.


# 2. Context: how the system will be used 🔄

This system is designed for use by a company's Procurement and Finance departments. It acts as a central, automated platform to manage all interactions with suppliers. It helps track orders from the moment they are placed until the final payment is made, ensuring everything runs smoothly and on time.

# 3. Target Users: 

•	**Procurement Officers:** Places orders with suppliers.

•	**Finance Officer:** Processes payments to suppliers after delivery.

•	**Supply Chain Manager:** Monitors overall supplier performance.

# 4. Our goals:

•	✅ Automate tracking of delivery and payment schedules.

•	✅ Generate supplier performance reports.

•	✅ Improve transparency and accountability in supplier management.

•	✅ It instantly finds and shows late deliveries and missed payments.

# 5. BI Potential

•	Analyze supplier on-time delivery rates.

•	Track payment cycle times and forecast cash flow.

•	Identify top-performing and underperforming suppliers for strategic decision-making.

# PHASE II: Business Process Modeling📊

**Main component:**

•	Order Placement: Procurement Officer creates a purchase order.

•	Delivery & Recording: Supplier delivers goods; system records delivery date and expected payment schedule.

•	Payment Processing: Finance Officer validates and processes payments.

•	Monitoring & Alerts: System automatically flags delayed deliveries or overdue payments using PL/SQL triggers.

•	Reporting: System generates monthly supplier performance reports via cursors.

**MIS Functions** 💻:

•	Automates data validation and exception handling

•	Provides real-time alerts for delays and overdue payments

•	Tracks supplier performance via predefined KPIs (e.g., on-time delivery rate, pending payments)

**Organizational Impact** 🏢:

•	Reduces manual errors

•	Improves supplier accountability

•	Enhances financial planning and transparency

**Analytics Opportunities:**

•	Trend analysis of supplier delivery performance

•	Forecasting payment schedules and cash flow

•	Identifying frequently delayed suppliers for contract reviews

**BPMN diagram**

<img width="682" height="968" alt="Untitled Diagram drawio" src="https://github.com/user-attachments/assets/d52e8c16-8a5d-481f-9fd7-fe2a7ec5cbff" />

# PHASE III: Logical Model Design

# The tables 📋

**The system has four  tables:**

**1. Suppliers:** Stores basic information about each business partner.
         
  - supplier_id (PK), supplier_name, contact_number, email, address

**2. Products:** Details of items purchased, linked to the main supplier.
 
  - product_id (PK), product_name, unit_price, supplier_id( FK)

**3. Deliveries:** Tracks procurement events, dates, and current delivery status.
 
  - payment_id (PK), supplier_id (FK), product_id (FK), quantity, expected_date, delivery_date, status
         
**4. Payments:** Records financial transactions and their scheduled/actual completion dates.
 
  - payment_id (PK), supplier_id (FK), amount, due_date, payment_date, status

# Relationships 🔗

✅ Suppliers (1) to Deliveries (A single Supplier can be responsible for multiple Deliveries over time.) 

✅ Products (1) to Deliveries  (A single Product can be included in many different Deliveries.)

✅ Suppliers (1) to Payments (A single Supplier will receive many Payments over the life of the business relationship.)

✅ Product to Supplier: (One supplier can provide many products.)

# Entity-Relationship Diagram 🔷

Supplier Payment and Delivery Monitoring System - ER diagram

<img width="612" height="586" alt="image" src="https://github.com/user-attachments/assets/795eb306-4dc4-42d8-9194-92668161aa6f" />

# Normalization

The design is intended to be in at least 3NF to minimize data duplication and ensure data integrity

**1NF:** All attributes are atomic (e.g., supplier_id and supplier_name are distinct).

**2NF:** Non-key attributes depend on the full Primary Key (e.g., in deliveries, quantity depends on the full composite key of the order, if applicable).

**3NF:** Non-key attributes depend only on the primary key, eliminating transitive dependencies (e.g., all supplier contact info resides only in the suppliers table, linked by supplier_id FK in other tables).

# PHASE IV: Database Creation 🗄️

creating and configuring oracle pluggable database

# details

**•	Name:** Wed_29259_Kevin_SupplierMS_db

**•	Password:** Kevin

**•	Access:** Super Admin (full control)

**•	Tool:** Oracle Enterprise Manager (OEM) to check performance

<img width="624" height="474" alt="image" src="https://github.com/user-attachments/assets/00b8e2b5-81b6-4ae4-8f86-14ff8bc291d8" />

Real-World Fact: Database monitoring tools like Oracle Enterprise Manager help procurement teams detect payment delays and supplier issues 40% faster, reducing operational costs by 25% in manufacturing firms.

<img width="1911" height="870" alt="Screenshot 2025-11-22 103847" src="https://github.com/user-attachments/assets/71535934-c2d1-4577-a63c-db1607ffc673" />

# 🛠️ PHASE V: Table Implementation & Data Insertion

# Creating Tables

Here's is how I built my database

         CREATE TABLE suppliers (
             supplier_id NUMBER(5) PRIMARY KEY,
             supplier_name VARCHAR2(100) NOT NULL,
             contact_number VARCHAR2(15),
             email VARCHAR2(100),
             address VARCHAR2(150)
         );
         
         CREATE TABLE products (
             product_id NUMBER(5) PRIMARY KEY,
             product_name VARCHAR2(100) NOT NULL,
             unit_price DECIMAL(10, 2) NOT NULL CHECK (unit_price > 0),
             supplier_id NUMBER(5) REFERENCES suppliers(supplier_id)
         );
         
         CREATE TABLE deliveries (
             delivery_id NUMBER(5) PRIMARY KEY,
             supplier_id NUMBER(5) REFERENCES suppliers(supplier_id),
             product_id NUMBER(5) REFERENCES products(product_id),
             quantity NUMBER(8),
             expected_date DATE,
             delivery_date DATE,
             status VARCHAR2(20) 
                 CHECK (status IN ('Pending', 'Delayed', 'Delivered', 'Cancelled'))
         );
         
         CREATE TABLE payments (
             payment_id NUMBER(5) PRIMARY KEY,
             supplier_id NUMBER(5) REFERENCES suppliers(supplier_id),
             amount DECIMAL(10, 2) NOT NULL CHECK (amount >= 0),
             due_date DATE,
             payment_date DATE,
             status VARCHAR2(20)
                 CHECK (status IN ('Due', 'Overdue', 'Paid', 'Partial'))
         );

<img width="1919" height="1007" alt="Screenshot 2025-11-22 110403" src="https://github.com/user-attachments/assets/007b87a2-ff4d-4347-9287-2393625aa7ba" />

# Adding Sample Data 

I added example info to test the system:

         INSERT INTO suppliers VALUES (101, 'Alpha Supplies Ltd', '0788111222', 'alpha@supply.com', 'Kigali, KG 1');
         INSERT INTO suppliers VALUES (102, 'Beta Electronics', '0788333444', 'beta@elec.com', 'Musanze, MS 2');
         
         INSERT INTO products VALUES (1, 'Office Paper', 50.00, 101);
         INSERT INTO products VALUES (2, 'Laptop Charger', 250.00, 102);
         
         INSERT INTO deliveries VALUES (1001, 101, 1, 500, DATE '2025-12-01', DATE '2025-11-29', 'Delivered');
         INSERT INTO deliveries VALUES (1002, 102, 2, 50, DATE '2025-12-05', NULL, 'Pending'); 
         
         INSERT INTO payments VALUES (1, 101, 25000.00, DATE '2025-12-10', DATE '2025-12-08', 'Paid');
         INSERT INTO payments VALUES (2, 102, 12500.00, DATE '2025-11-20', NULL, 'Due');


