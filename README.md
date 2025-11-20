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

**MIS Functions:**

•	Automates data validation and exception handling

•	Provides real-time alerts for delays and overdue payments

•	Tracks supplier performance via predefined KPIs (e.g., on-time delivery rate, pending payments)

