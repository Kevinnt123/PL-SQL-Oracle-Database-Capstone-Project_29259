# PL/SQL Oracle Database Capstone Project

# 📋 What’s This Project About? 
This project is a Supplier Payment and Delivery Monitoring System built with an Oracle database and PL/SQL. It automates the tracking of supplier orders, delivery dates, and payment schedules. The system automatically flags delayed shipments and overdue payments using database triggers and procedures. It also generates performance reports to help businesses identify reliable suppliers. The goal is to eliminate manual errors, ensure timely payments, and improve overall supply chain efficiency.

**Name:** NTAKIRUTIMANA Kevin

**ID:** 29259

**Project title:** Supplier Payment and Delivery Monitoring System

**Group:** Wednesday

**Lecturer:** Eric Maniraguha

**Academic Year:** 2025-2026

# PHASE I: Problem Statement & Presentation

# 1. Problem definition

•	Late Delivery: A key supplier's shipment is late, but no one knows until it's already a problem. This stops production and hurts customer trust, all because of manual tracking.

•	Payment Confusion: Finance gets an invoice, but is it for a delivered order? Is it even overdue? Without a clear system, payments get delayed, and suppliers get frustrated.

•	No Supplier Report Card: Is Supplier A always on time? Is Supplier B often late? There's no easy way to track performance, so it's hard to know who your reliable partners are.


# 2. Context: how the system will be used 

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

# PHASE II: Business Process Modeling

**Main component:**

•	Order Placement: Procurement Officer creates a purchase order.

•	Delivery & Recording: Supplier delivers goods; system records delivery date and expected payment schedule.

•	Payment Processing: Finance Officer validates and processes payments.

•	Monitoring & Alerts: System automatically flags delayed deliveries or overdue payments using PL/SQL triggers.

•	Reporting: System generates monthly supplier performance reports via cursors.

**MIS Functions**:

•	Automates data validation and exception handling

•	Provides real-time alerts for delays and overdue payments

•	Tracks supplier performance via predefined KPIs (e.g., on-time delivery rate, pending payments)

**Organizational Impact**:

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

# 📘 Database Normalization

The design is intended to be in at least 3NF to minimize data duplication and ensure data integrity

**1NF:** All attributes are atomic (e.g., supplier_id and supplier_name are distinct).

**2NF:** Non-key attributes depend on the full Primary Key (e.g., in deliveries, quantity depends on the full composite key of the order, if applicable).

**3NF:** Non-key attributes depend only on the primary key, eliminating transitive dependencies (e.g., all supplier contact info resides only in the suppliers table, linked by supplier_id FK in other tables).

# PHASE IV: Database Creation

creating and configuring oracle pluggable database

# details

**•	Name:** Wed_29259_Kevin_SupplierMS_db

**•	Password:** Kevin

**•	Access:** Super Admin (full control)

**•	Tool:** Oracle Enterprise Manager (OEM) to check performance

<img width="624" height="474" alt="image" src="https://github.com/user-attachments/assets/00b8e2b5-81b6-4ae4-8f86-14ff8bc291d8" />

Real-World Fact: Database monitoring tools like Oracle Enterprise Manager help procurement teams detect payment delays and supplier issues 40% faster, reducing operational costs in manufacturing firms.

<img width="1911" height="870" alt="Screenshot 2025-11-22 103847" src="https://github.com/user-attachments/assets/71535934-c2d1-4577-a63c-db1607ffc673" />

# 🛠️ PHASE V: Table Implementation & Data Insertion

# Creating Tables

Here's is how I built my database

```SQL
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
```
<img width="1919" height="1007" alt="Screenshot 2025-11-22 110403" src="https://github.com/user-attachments/assets/007b87a2-ff4d-4347-9287-2393625aa7ba" />

# Adding Sample Data 

I added example info to test the system:

```SQL
         INSERT INTO suppliers VALUES (101, 'Alpha Supplies Ltd', '0788111222', 'alpha@supply.com', 'Kigali, KG 1');
         INSERT INTO suppliers VALUES (102, 'Beta Electronics', '0788333444', 'beta@elec.com', 'Musanze, MS 2');
         
         INSERT INTO products VALUES (1, 'Office Paper', 50.00, 101);
         INSERT INTO products VALUES (2, 'Laptop Charger', 250.00, 102);
         
         INSERT INTO deliveries VALUES (1001, 101, 1, 500, DATE '2025-12-01', DATE '2025-11-29', 'Delivered');
         INSERT INTO deliveries VALUES (1002, 102, 2, 50, DATE '2025-12-05', NULL, 'Pending'); 
         
         INSERT INTO payments VALUES (1, 101, 25000.00, DATE '2025-12-10', DATE '2025-12-08', 'Paid');
         INSERT INTO payments VALUES (2, 102, 12500.00, DATE '2025-11-20', NULL, 'Due');
```
<img width="1914" height="990" alt="Screenshot 2025-11-22 112230" src="https://github.com/user-attachments/assets/279ba4cf-2653-423c-99ea-dd1d1ebb98ff" />

Real-World Fact: Adding realistic supplier and delivery test data helps manufacturing companies identify payment bottlenecks and delivery issues before system deployment, reducing procurement errors and improving supplier relationship management.

# ⚙️ Phase VI: Database Interaction & Transactions

The Supplier Monitoring System is dynamic! It uses PL/SQL to enable staff to safely add, change, and check real-time supplier data.

# DML (Data Manipulation Language)
```SQL
UPDATE suppliers SET email = 'beta.electronics.new@email.com' WHERE supplier_id = 102;
```
<img width="1908" height="869" alt="Screenshot 2025-12-02 113518" src="https://github.com/user-attachments/assets/407c0ed3-e4d5-44bc-8704-6b7a60d0a6cd" />

# DDL (Data Definition Language)
```SQL
CREATE TABLE supplier_reviews (
    review_id NUMBER PRIMARY KEY,
    supplier_id NUMBER REFERENCES suppliers(supplier_id)
);

DROP TABLE supplier_reviews;
```
<img width="1919" height="1006" alt="image" src="https://github.com/user-attachments/assets/cec0cecf-092f-49ce-b739-0ac48957dfe1" />

# Analytical Task: Tracking Supplier Payments

Problem: How much has a specific supplier been paid over time, and what is the running total of those payments?

Solution: Used a window function (SUM() OVER...) to group payments by supplier and compute a cumulative total, helping the finance team track total expenditure per vendor.
```SQL
SELECT 
    supplier_id,
    payment_id,
    amount,
    SUM(amount) OVER (PARTITION BY supplier_id ORDER BY payment_date) AS running_total
FROM payments
ORDER BY supplier_id, payment_date;
```

<img width="1919" height="1009" alt="image" src="https://github.com/user-attachments/assets/bba42003-cc82-48fb-b14e-bf1a734372b2" />

This query is vital for financial auditing. It instantly shows the cumulative value of business conducted with a supplier, which is critical for contract review and budget analysis.

# Procedure Implementation: Fetching Supplier Contact Information

The get_supplier_info procedure takes a supplier ID as input and safely retrieves the supplier’s name and contact number. It includes exception handling to manage common errors like an invalid ID.
```SQL
CREATE OR REPLACE PROCEDURE get_supplier_info(p_supplier_id IN NUMBER) IS
    v_name suppliers.supplier_name%TYPE;
    v_phone suppliers.contact_number%TYPE;
BEGIN
    SELECT supplier_name, contact_number
    INTO v_name, v_phone
    FROM suppliers
    WHERE supplier_id = p_supplier_id;

    DBMS_OUTPUT.PUT_LINE('Supplier Name: ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Contact Number: ' || v_phone);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No supplier found with ID ' || p_supplier_id);
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error retrieving supplier details: ' || SQLERRM);
END;
/
```
<img width="1919" height="1008" alt="image" src="https://github.com/user-attachments/assets/0a6c360b-6278-4bdf-80d0-6a9c564733e3" />

# Procedure Call
```sql
BEGIN
    DBMS_OUTPUT.ENABLE;
    get_supplier_info(102); 
END;
/
```

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/0e4af285-b781-4f4d-941b-3512a7240a22" />

# Implementation with Cursor: Listing Supplier Deliveries

A cursor is used to efficiently loop through and display all delivery records for a specified supplier, which is crucial for logistics staff to review a vendor's delivery history
```sql
DECLARE
    CURSOR c_supplier_deliveries IS
        SELECT delivery_id, expected_date, delivery_date, status
        FROM deliveries
        WHERE supplier_id = 102; -- Using supplier 102 as an example
        
    v_delivery_rec c_supplier_deliveries%ROWTYPE;
BEGIN
    DBMS_OUTPUT.PUT_LINE('--- Deliveries for Supplier 102 ---');
    
    OPEN c_supplier_deliveries;
    LOOP
        FETCH c_supplier_deliveries INTO v_delivery_rec;
        EXIT WHEN c_supplier_deliveries%NOTFOUND;
        
        DBMS_OUTPUT.PUT_LINE('Delivery ID: ' || v_delivery_rec.delivery_id || 
                            ', Expected: ' || v_delivery_rec.expected_date || 
                            ', Actual: ' || NVL(TO_CHAR(v_delivery_rec.delivery_date, 'YYYY-MM-DD'), 'N/A') ||
                            ', Status: ' || v_delivery_rec.status);
    END LOOP;
    CLOSE c_supplier_deliveries;
END;
/
```
<img width="1919" height="1006" alt="image" src="https://github.com/user-attachments/assets/bfa797fa-8cbe-45a6-8bd1-d4c120d10423" />

# Testing cursors
```sql
SELECT delivery_id, expected_date, delivery_date, status
FROM deliveries
WHERE supplier_id = 102;
```
<img width="1914" height="1005" alt="image" src="https://github.com/user-attachments/assets/4e17b708-50f0-4905-92d0-e5d7eae54900" />

# Function Implementation: Total Amount Paid to a Supplier

A function is created to calculate the aggregate amount paid to a supplier, ensuring managers have a quick, accurate financial summary.
```sql
CREATE OR REPLACE FUNCTION total_amount_paid(p_supplier_id IN NUMBER) 
RETURN NUMBER IS
    v_total NUMBER;
BEGIN
    SELECT SUM(amount)
    INTO v_total
    FROM payments 
    WHERE supplier_id = p_supplier_id
    AND status = 'Paid'; 
    
    RETURN NVL(v_total, 0);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 0;
    WHEN OTHERS THEN
        RETURN -1; 
END;
/
```
<img width="1915" height="1010" alt="image" src="https://github.com/user-attachments/assets/b5a8a72c-291b-4f29-9139-b3c3ee1ec0dc" />

# Testing the Function:
```sql
SELECT total_amount_paid(101) AS total_paid_to_supplier_101 FROM DUAL;
```
<img width="1919" height="1001" alt="image" src="https://github.com/user-attachments/assets/3e1c6838-9848-4ee2-ab49-27acb73c985f" />

# Package Implementation: Supplier Tools

The supplier_tools package logically groups related procedures and functions for managing supplier data, promoting code organization and reusability.
```sql
CREATE OR REPLACE PACKAGE supplier_tools AS
    PROCEDURE list_deliveries(p_supplier_id IN NUMBER);
    PROCEDURE display_payments(p_supplier_id IN NUMBER);
    FUNCTION total_paid(p_supplier_id IN NUMBER) RETURN NUMBER;
END supplier_tools;
/

CREATE OR REPLACE PACKAGE BODY supplier_tools AS
    
    PROCEDURE list_deliveries(p_supplier_id IN NUMBER) IS
    BEGIN
        FOR rec IN (
            SELECT delivery_id, expected_date, delivery_date, status
            FROM deliveries
            WHERE supplier_id = p_supplier_id
        ) LOOP
            DBMS_OUTPUT.PUT_LINE('Delivery ID: ' || rec.delivery_id || 
                                ', Expected: ' || rec.expected_date || 
                                ', Status: ' || rec.status);
        END LOOP;
    END list_deliveries;

    PROCEDURE display_payments(p_supplier_id IN NUMBER) IS
    BEGIN
        FOR rec IN (
            SELECT payment_id, amount, due_date, status
            FROM payments
            WHERE supplier_id = p_supplier_id
        ) LOOP
            DBMS_OUTPUT.PUT_LINE('Payment ID: ' || rec.payment_id || 
                                ', Amount: ' || rec.amount || 
                                ', Due Date: ' || rec.due_date ||
                                ', Status: ' || rec.status);
        END LOOP;
    END display_payments;

    FUNCTION total_paid(p_supplier_id IN NUMBER) RETURN NUMBER IS
        v_total NUMBER;
    BEGIN
        SELECT SUM(amount)
        INTO v_total
        FROM payments 
        WHERE supplier_id = p_supplier_id AND status = 'Paid';
        RETURN NVL(v_total, 0);
    EXCEPTION
        WHEN OTHERS THEN
            RETURN -1;
    END total_paid;
END supplier_tools;
/
```

<img width="1919" height="1005" alt="image" src="https://github.com/user-attachments/assets/cffec088-7841-438f-930b-60f67bc2c900" />

# Package Usage: Testing the Tools

This final block demonstrates how a logistics or finance user would call the package to get a complete snapshot of a supplier's status.
```sql
DECLARE
    v_paid_amount NUMBER;
    v_supplier_id CONSTANT NUMBER := 102; 
BEGIN
    DBMS_OUTPUT.ENABLE;
    
    DBMS_OUTPUT.PUT_LINE('--- SUPPLIER ' || v_supplier_id || ' DELIVERY HISTORY ---');
    supplier_tools.list_deliveries(v_supplier_id);
    
    DBMS_OUTPUT.PUT_LINE('--- SUPPLIER ' || v_supplier_id || ' PAYMENT HISTORY ---');
    supplier_tools.display_payments(v_supplier_id);
    
    v_paid_amount := supplier_tools.total_paid(v_supplier_id);
    DBMS_OUTPUT.PUT_LINE('TOTAL AMOUNT PAID TO SUPPLIER ' || v_supplier_id || ': ' || v_paid_amount);
END;
/
```

<img width="1918" height="1005" alt="image" src="https://github.com/user-attachments/assets/4685c1e5-ca4a-41de-adbe-bbfa459d8117" />

# 🔒 PHASE VII: Advanced Programming & Auditing

This phase implements the advanced PL/SQL features—Triggers—to ensure the system is automated, secure, and tracks every significant action. These triggers act like silent, automatic guards for the data, critical for monitoring your high-value supplier transactions.

# Why It’s Needed 🌟

* The supply chain and financial operations involve tight deadlines and strict rules. We need clever rules to:

* Automate Status Updates: Instantly detect and flag delayed Deliveries without manual checks.

* Prevent Financial Changes: Block changes to Payments on non-working days (weekdays/holidays) to ensure data stability during high-volume periods.

* Ensure Accountability: Track every modification to core data, like Supplier information. These rules are called Triggers, and they act like magic guards protecting the system!

# DDL for Required Tables

To support the security and restriction logic, we define the holidays and audit_logs tables.

Creation of Audit Log Table To track who did what and when, we first ensure the mandatory audit_logs table is present.
```sql
CREATE TABLE audit_logs (
    audit_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    user_id VARCHAR2(50),
    action_time TIMESTAMP DEFAULT SYSTIMESTAMP,
    operation VARCHAR2(50),
    status VARCHAR2(20)
);
```
Table Holidays This table stores dates when system modifications should be restricted
```sql
CREATE TABLE holidays (
    holiday_date DATE PRIMARY KEY,
    description VARCHAR2(100)
);
```
<img width="1912" height="1003" alt="image" src="https://github.com/user-attachments/assets/4eaaf356-ed9c-4f03-9f99-9c456346a119" />

# Smart Rule (Trigger): trigger restrict delivery weekdays

This kind of trigger spots changes such as adding, updating, or deleting deliveries on Mondays to Fridays and on holidays.
```sql
CREATE OR REPLACE TRIGGER trg_restrict_delivery_weekdays
BEFORE INSERT OR UPDATE ON deliveries
DECLARE
    v_day VARCHAR2(10);
    v_holiday_count NUMBER;
BEGIN
    v_day := TRIM(TO_CHAR(SYSDATE, 'DAY'));
    
    IF v_day IN ('MONDAY', 'TUESDAY', 'WEDNESDAY', 'THURSDAY', 'FRIDAY') THEN
        RAISE_APPLICATION_ERROR(-20001, 'Deliveries cannot be modified on weekdays.');
    END IF;
    
    SELECT COUNT(*) INTO v_holiday_count
    FROM holidays
    WHERE TRUNC(holiday_date) = TRUNC(SYSDATE);
    
    IF v_holiday_count > 0 THEN
        RAISE_APPLICATION_ERROR(-20002, 'Today is a holiday. Delivery operations are not allowed.');
    END IF;
END;
/
/
```
<img width="1918" height="1000" alt="trg_restrict_delivery_weekdays" src="https://github.com/user-attachments/assets/97935d51-7611-4a25-81af-370e08d149c3" />

This trigger directly implements the core business logic of Phase I automatically detecting and flagging delayed deliveries using simple date comparison.

# Tracking Every Change (Auditing Trigger on Suppliers) 🛡️

This trigger logs all changes made to the central suppliers table. Any time a supplier's contact information is added, updated, or deleted, an entry is written to the audit_logs table.
```sql
CREATE OR REPLACE TRIGGER trg_audit_supplier_changes
AFTER INSERT OR UPDATE OR DELETE ON suppliers
FOR EACH ROW
DECLARE
    v_operation VARCHAR2(50);
BEGIN
    IF INSERTING THEN
        v_operation := 'INSERT_SUPPLIER';
    ELSIF UPDATING THEN
        v_operation := 'UPDATE_SUPPLIER';
    ELSIF DELETING THEN
        v_operation := 'DELETE_SUPPLIER';
    END IF;
    
    INSERT INTO audit_logs(user_id, operation, status)
    VALUES (USER, v_operation, 'Logged');
END;
/
```
<img width="1910" height="1003" alt="image" src="https://github.com/user-attachments/assets/c74884e2-0110-4f8e-aba4-4d63aabf975c" />

This ensures accountability. If a supplier's record is altered, managers can check the audit_logs to see who made the change and when.

# Log Denied Actions (Logging Function)

This function is crucial for logging when an attempt to modify data (e.g., during a weekday block) is rejected by the system's security rules
```sql
CREATE OR REPLACE FUNCTION log_denied_action(p_action VARCHAR2)
RETURN VARCHAR2 IS
BEGIN
    INSERT INTO audit_logs(user_id, operation, status)
    VALUES (USER, p_action, 'Denied');
    RETURN 'Logged';
EXCEPTION
    WHEN OTHERS THEN
        RETURN 'Failed to log: ' || SQLERRM;
END;
/
```
<img width="1919" height="1003" alt="image" src="https://github.com/user-attachments/assets/f8df5d9a-93bd-4c4e-84d9-04e61dbe940f" />

# Testing the Triggers: Let’s See If They Work! 🧪

We test the triggers to prove they are correctly applying the business rules like superheroes.

# Testing the trigger restrict delivery weekdays

1. **Test 1:** Testing the trigger in weekdays 

This kind of rule will stop me to make changes in weekdays

* **Action:**
```sql
INSERT INTO deliveries (delivery_id, supplier_id, product_id, quantity, expected_date, delivery_date)
VALUES (3001, 102, 2, 10, TRUNC(SYSDATE - 1), NULL);
```
<img width="1915" height="982" alt="testing trg_restrict_delivery_weekdays(weekdays)" src="https://github.com/user-attachments/assets/ca82c33f-d82e-4771-b14a-8d63cfb98796" />

Result: 'Deliveries cannot be modified on weekdays.' the trigger worked perfectly it stopped me from making changes in weekdays

2. **Test 2 :** Testing the trigger in weekend

*** Action:**
```sql
INSERT INTO deliveries (delivery_id, supplier_id, product_id, quantity, expected_date, delivery_date)
VALUES (9999, 101, 1, 50, TRUNC(SYSDATE + 7), TRUNC(SYSDATE));
```
<img width="1919" height="1017" alt="testing trg_restrict_delivery_weekdays(holidays)" src="https://github.com/user-attachments/assets/2adb029e-ab7f-4390-b216-134e688b29e5" />

**Results:** The trigger will automatically work because we are in weekend or public holiday. 

# Testing the Auditing Trigger (on Suppliers)

1. Action: Update a supplier's contact number.
```sql
UPDATE suppliers
SET contact_number = '0789123456'
WHERE supplier_id = 101;
```
<img width="1914" height="997" alt="image" src="https://github.com/user-attachments/assets/d7c4ec97-0ec7-4418-9b3f-6b3fee125fed" />

2. Check the Audit Log:
```sql
SELECT user_id, operation, status, action_time FROM audit_logs;
```
<img width="1919" height="1005" alt="image" src="https://github.com/user-attachments/assets/24eab523-b445-4534-8dd6-0827802f7df2" />

**Result:** The log will show a new entry: USER_ID, OPERATION: UPDATE_SUPPLIER, STATUS: Logged, at the current timestamp, proving the action was recorded.

# Tracking System 🛡️

* What It Tracks: Who did what, when, and if it was blocked on key tables (suppliers, payments, deliveries).

*  Why It Matters: Keeps the supplier payment and monitoring system safe from operational mistakes and ensures full accountability for financial and logistical data.

# What We Learned from Testing:

✅ The Delivery Status rule works—it automatically flags delayed items, preventing manual errors.

✅ Every change to a Supplier record is tracked, so nothing gets lost.

✅ The system can log denied attempts (using the function), ensuring a complete security record.
