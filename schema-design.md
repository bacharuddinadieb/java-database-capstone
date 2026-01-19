# Smart Clinic Management System — Schema Design

This document describes the database schema design for the **Smart Clinic Management System**, covering both the relational (MySQL) and document-based (MongoDB) data stores. The design follows the system’s three-tier architecture and supports scalability, data integrity, and flexibility.

---

## MySQL Database Design (Structured Data)

MySQL is used for core entities that require strong consistency, relationships, and constraints.

### 1. `admin` Table

Stores system administrators who manage users and configurations.

| Column Name   | Data Type    | Constraints                         | Description             |
| ------------- | ------------ | ----------------------------------- | ----------------------- |
| id            | BIGINT       | PRIMARY KEY, AUTO_INCREMENT         | Unique admin identifier |
| username      | VARCHAR(50)  | NOT NULL, UNIQUE                    | Admin login username    |
| password_hash | VARCHAR(255) | NOT NULL                            | Encrypted password      |
| email         | VARCHAR(100) | NOT NULL, UNIQUE                    | Admin email address     |
| created_at    | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Account creation time   |

---

### 2. `doctor` Table

Stores doctor profiles and professional information.

| Column Name    | Data Type    | Constraints                         | Description              |
| -------------- | ------------ | ----------------------------------- | ------------------------ |
| id             | BIGINT       | PRIMARY KEY, AUTO_INCREMENT         | Unique doctor identifier |
| name           | VARCHAR(100) | NOT NULL                            | Doctor full name         |
| specialization | VARCHAR(100) | NOT NULL                            | Medical specialization   |
| email          | VARCHAR(100) | NOT NULL, UNIQUE                    | Doctor email             |
| phone          | VARCHAR(20)  | NOT NULL                            | Contact number           |
| is_active      | BOOLEAN      | NOT NULL, DEFAULT TRUE              | Availability status      |
| created_at     | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Profile creation time    |

---

### 3. `patient` Table

Stores patient personal and contact information.

| Column Name   | Data Type    | Constraints                         | Description               |
| ------------- | ------------ | ----------------------------------- | ------------------------- |
| id            | BIGINT       | PRIMARY KEY, AUTO_INCREMENT         | Unique patient identifier |
| name          | VARCHAR(100) | NOT NULL                            | Patient full name         |
| date_of_birth | DATE         | NOT NULL                            | Patient birth date        |
| email         | VARCHAR(100) | NOT NULL, UNIQUE                    | Patient email             |
| phone         | VARCHAR(20)  | NOT NULL                            | Contact number            |
| created_at    | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Registration time         |

---

### 4. `appointment` Table

Stores appointment scheduling information and links doctors with patients.

| Column Name      | Data Type   | Constraints                         | Description                          |
| ---------------- | ----------- | ----------------------------------- | ------------------------------------ |
| id               | BIGINT      | PRIMARY KEY, AUTO_INCREMENT         | Unique appointment identifier        |
| doctor_id        | BIGINT      | NOT NULL, FOREIGN KEY               | References doctor(id)                |
| patient_id       | BIGINT      | NOT NULL, FOREIGN KEY               | References patient(id)               |
| appointment_time | DATETIME    | NOT NULL                            | Scheduled appointment time           |
| status           | VARCHAR(20) | NOT NULL                            | Status (BOOKED, CANCELED, COMPLETED) |
| created_at       | TIMESTAMP   | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Booking time                         |

**Design Notes:**

* Foreign keys enforce referential integrity between doctors, patients, and appointments.
* Status is kept as a string for readability and easier extension.

---

## MongoDB Collection Design (Unstructured Data)

MongoDB is used for data that benefits from schema flexibility and nested structures.

### Collection: `prescriptions`

This collection stores prescription records created by doctors for patients. Prescriptions may vary in structure and may evolve over time, making MongoDB a suitable choice.

### Example Document

```json
{
  "_id": "64f1b92c8e9a123456789abc",
  "appointmentId": 1023,
  "doctor": {
    "id": 31,
    "name": "Dr. Fulan",
    "specialization": "Cardiology"
  },
  "patient": {
    "id": 45,
    "name": "Fulan binti Fulan"
  },
  "medications": [
    {
      "name": "Aspirin",
      "dosage": "100 mg",
      "frequency": "Once daily",
      "duration": "7 days"
    },
    {
      "name": "Atorvastatin",
      "dosage": "20 mg",
      "frequency": "Once daily",
      "duration": "30 days"
    }
  ],
  "notes": "Monitor blood pressure and cholesterol levels",
  "issuedAt": "2026-01-10T09:30:00Z"
}
```

**Design Notes:**

* Nested objects (`doctor`, `patient`) reduce the need for joins when reading prescriptions.
* The `medications` array allows flexible prescription sizes.
* MongoDB supports easy schema evolution if new medical fields are required later.

---

## Summary

* MySQL ensures consistency and integrity for core transactional data.
* MongoDB provides flexibility for complex and evolving medical records.
* This hybrid design aligns with the Smart Clinic system’s scalability and maintainability goals.
