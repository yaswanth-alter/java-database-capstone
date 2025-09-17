#  Database Schema Design - Smart Clinic Management System

This document outlines the relational and document-based data models used in the SmartCare Clinic Management System. The system uses **MySQL** for structured data and **MongoDB** for flexible, nested medical records.

---

## MySQL Relational Schema

### 1. `patients` Table
Stores patient information.

```sql
CREATE TABLE patients (
  id INT PRIMARY KEY AUTO_INCREMENT,
  full_name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  phone VARCHAR(20),
  birth_date DATE,
  gender ENUM('M', 'F', 'Other'),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

```
 email is marked as UNIQUE to prevent duplicate accounts.


### 2. `doctors` Table  
Stores doctors information.

```sql
CREATE TABLE doctors (
  id INT PRIMARY KEY AUTO_INCREMENT,
  full_name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  specialty VARCHAR(50) NOT NULL,
  available_hours TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

available_hours can be parsed as JSON or comma-separated values for flexibility.

### 3. `appointments` Table
Links patients and doctors with scheduled time slots.

```sql
CREATE TABLE appointments (
  id INT PRIMARY KEY AUTO_INCREMENT,
  patient_id INT NOT NULL,
  doctor_id INT NOT NULL,
  appointment_date DATE NOT NULL,
  time_slot VARCHAR(20) NOT NULL,
  status ENUM('Scheduled', 'Completed', 'Cancelled') DEFAULT 'Scheduled',
  FOREIGN KEY (patient_id) REFERENCES patients(id),
  FOREIGN KEY (doctor_id) REFERENCES doctors(id)
);
```

Foreign keys enforce referential integrity between patients and doctors.

### 4. `admins` Table
Stores system administrator accounts.

```sql
CREATE TABLE admins (
  id INT PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(50) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  role ENUM('SuperAdmin', 'Manager') DEFAULT 'Manager',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
Passwords are stored as hashes for security. Roles allow for permission-based access.

## MongoDB Document Schema
### 1. `prescriptions` Collection
Stores medical prescriptions linked to appointments.

```Json
{
  "appointmentId": 131,
  "patient": {
    "id": 26,
    "name": "John Smith"
  },
  "doctor": {
    "id": 12,
    "name": "Dr. Emily Adams"
  },
  "medications": [
    {
      "name": "Vitamin C tablets",
      "dosage": "Twice a day",
      "duration": "7 days"
    },
    {
      "name": "Ibuprofen",
      "dosage": "Once every 8 hours",
      "duration": "5 days"
    }
  ],
  "notes": "Patient should hydrate and rest.",
  "issued_at": "2025-05-22T09:00:00Z"
}
```
Nested objects for patient and doctor allow quick access without additional joins.
medications is an array to support multiple prescriptions per visit.
appointmentId links this document back to the relational system.

## Justification
 MySQL is ideal for structured, transactional data like users and appointments.

 MongoDB supports flexible, nested data like prescriptions, which vary in length and structure.

 This hybrid approach ensures scalability, performance, and clarity in data modeling.



---

### Next Steps

To commit this file to  GitHub repository:

```bash
git add schema-design.md
git commit -m "Add database schema design for MySQL and MongoDB"
git push origin main
```

