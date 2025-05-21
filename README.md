# hospital-sql-project
MySQL project for managing hospital operations and data

This SQL project simulates a hospital management system using **MySQL**. It includes database schema design, data insertion, views, queries, and procedures.

---

## 📚 Features

- Manage Departments, Doctors, Patients
- Book and Track Appointments
- Handle Room Allocations & Bills
- Store Prescriptions & Medicine stock
- Execute Reports using SQL Queries & Views
- Procedure to generate bills

---

## 📁 Database Schema

### Tables
- `Departments`: Hospital departments
- `Doctors`: Doctors with specialization and department
- `Patients`: Patient details
- `Appointments`: Doctor-patient appointments
- `Medicines`: Stock of medicines
- `Bills`: Billing details
- `Prescriptions`: Medicines prescribed during appointments
- `Room_Allocations`: Admission and discharge records

---

## 📊 Sample Queries

```sql

-- Active Appointments
SELECT p.name AS patient, d.name AS doctor, a.appointment_date
FROM Appointments a
JOIN Patients p ON a.patient_id = p.patient_id
JOIN Doctors d ON a.doctor_id = d.doctor_id
WHERE a.status = 'Scheduled';

-- Medicine Stock Alert
SELECT name, quantity FROM Medicines WHERE quantity < 100;

-- Current Hospitalized Patients
SELECT p.name, r.room_number
FROM Room_Allocations r
JOIN Patients p ON r.patient_id = p.patient_id
WHERE r.discharge_date IS NULL;

