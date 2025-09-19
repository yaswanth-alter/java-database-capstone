#  Database Schema Design - Smart Clinic Management System

This document outlines the relational and document-based data models used in the SmartCare Clinic Management System. The system uses **MySQL** for structured data and **MongoDB** for flexible, nested medical records.

---

## MySQL Relational Schema

### 1. `patients` Table
Stores patient information.

```sql
CREATE TABLE patients (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  phone VARCHAR(10) NOT NULL,
  address VARCHAR(255) NOT NULL
);

```
 email is marked as UNIQUE to prevent duplicate accounts.


### 2. `doctors` Table  
Stores doctors information.

```sql
CREATE TABLE doctors (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100) NOT NULL,
  specialty VARCHAR(50) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  phone VARCHAR(10) NOT NULL
);
```

available_hours can be parsed as JSON or comma-separated values for flexibility.

### 3. `appointments` Table
Links patients and doctors with scheduled time slots.

```sql
CREATE TABLE appointments (
  id INT PRIMARY KEY AUTO_INCREMENT,
  doctor_id INT NOT NULL,
  patient_id INT NOT NULL,
  appointment_time DATETIME NOT NULL,
  status INT NOT NULL,
  FOREIGN KEY (doctor_id) REFERENCES doctors(id),
  FOREIGN KEY (patient_id) REFERENCES patients(id)
);
```

Foreign keys enforce referential integrity between patients and doctors.

### 4. `admins` Table
Stores system administrator accounts.

```sql
CREATE TABLE admins (
  id INT PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(50) NOT NULL,
  password VARCHAR(255) NOT NULL
);
```
Passwords are stored as hashes for security. Roles allow for permission-based access.

## MongoDB Document Schema
### 1. `prescriptions` Collection
Stores medical prescriptions linked to appointments.

```Json
{
 "prescriptionId": "12345",
  "patientId": "67890",
  "doctorId": "54321",
  "medications": [
   { "name": "Amoxicillin",
     "dosage": "500mg",
     "frequency": "Every 8 hours",
     "duration": "7 days"
   },
   { "name": "Ibuprofen",
     "dosage": "200mg",
     "frequency": "Every 6 hours",
     "duration": "3 days"
   }
  ],
 "tags": ["antibiotic", "pain relief"],
 "metadata": {
  "createdAt": "2023-10-01T12:00:00Z",
  "updatedAt": "2023-10-02T12:00:00Z",
  "status":    "active"
  },
 "notes": "Patient should take medication with food."
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

