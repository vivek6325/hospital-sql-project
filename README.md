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

-- Patient Billing Summary with Department Filter
SELECT 
    p.name AS patient_name,
    d.department_name,
    COUNT(a.appointment_id) AS visits,
    SUM(b.amount) AS total_billed
FROM Patients p
JOIN Appointments a ON p.patient_id = a.patient_id
JOIN Doctors doc ON a.doctor_id = doc.doctor_id
JOIN Departments d ON doc.department_id = d.department_id
LEFT JOIN Bills b ON p.patient_id = b.patient_id
WHERE d.department_name = 'Cardiology'  -- Filter by department
GROUP BY p.patient_id, d.department_name
HAVING total_billed > 1000;  -- Only show significant bills

-- Doctor Availability Check
DELIMITER //
CREATE PROCEDURE CheckDoctorAvailability(
    IN p_doctor_id INT,
    IN p_appointment_date DATETIME,
    OUT p_is_available BOOLEAN
)
BEGIN
    DECLARE conflict_count INT;
    
    SELECT COUNT(*) INTO conflict_count
    FROM Appointments
    WHERE doctor_id = p_doctor_id
    AND appointment_date = p_appointment_date
    AND status != 'Cancelled';
    
    SET p_is_available = (conflict_count = 0);
END //
DELIMITER ;

-- Usage:
CALL CheckDoctorAvailability(101, '2025-07-15 10:00:00', @is_available);
SELECT @is_available AS is_doctor_available;

