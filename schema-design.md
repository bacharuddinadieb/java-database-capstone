# Smart Clinic Management System — Schema Design

This document defines the database schema design for the **Smart Clinic Management System**, based on real-world clinic operations such as patient registration, appointment scheduling, doctor availability management, and prescription handling. The design intentionally separates **structured relational data** (MySQL) from **flexible, evolving data** (MongoDB).

Before defining tables and collections, consider the core actions of a smart clinic:

* Registering patients and doctors
* Scheduling and tracking appointments
* Managing doctor availability and working hours
* Creating and managing prescriptions
* Storing notes, feedback, or logs

These actions give rise to core entities (Patient, Doctor, Appointment) and clear relationships (e.g., one Patient can have many Appointments).

---

## MySQL Database Design

MySQL is used for **structured, validated, and interrelated data** that forms the operational backbone of the clinic system. Strong constraints and relationships help ensure data integrity.

### Table: patients

Stores patient identity and contact information. Patient records are retained even if appointments are deleted, preserving medical history.

* id: BIGINT, Primary Key, Auto Increment
* full_name: VARCHAR(100), NOT NULL
* date_of_birth: DATE, NOT NULL
* email: VARCHAR(100), NOT NULL, UNIQUE
* phone: VARCHAR(20), NOT NULL
* created_at: TIMESTAMP, NOT NULL, DEFAULT CURRENT_TIMESTAMP

---

### Table: doctors

Stores doctor profiles and professional details. Availability is managed separately to avoid hardcoding schedules.

* id: BIGINT, Primary Key, Auto Increment
* full_name: VARCHAR(100), NOT NULL
* specialization: VARCHAR(100), NOT NULL
* email: VARCHAR(100), NOT NULL, UNIQUE
* phone: VARCHAR(20), NOT NULL
* is_active: BOOLEAN, NOT NULL, DEFAULT TRUE
* created_at: TIMESTAMP, NOT NULL, DEFAULT CURRENT_TIMESTAMP

---

### Table: admin

Stores system administrators who manage users, roles, and system configuration.

* id: BIGINT, Primary Key, Auto Increment
* username: VARCHAR(50), NOT NULL, UNIQUE
* password_hash: VARCHAR(255), NOT NULL
* email: VARCHAR(100), NOT NULL, UNIQUE
* created_at: TIMESTAMP, NOT NULL, DEFAULT CURRENT_TIMESTAMP

---

### Table: appointments

Represents scheduled visits between doctors and patients. Appointments are retained even if marked canceled or completed for audit and history purposes.

* id: BIGINT, Primary Key, Auto Increment
* doctor_id: BIGINT, NOT NULL, Foreign Key → doctors(id)
* patient_id: BIGINT, NOT NULL, Foreign Key → patients(id)
* appointment_time: DATETIME, NOT NULL
* status: INT, NOT NULL

  * 0 = Scheduled
  * 1 = Completed
  * 2 = Cancelled
* created_at: TIMESTAMP, NOT NULL, DEFAULT CURRENT_TIMESTAMP

**Design considerations:**

* Doctors should not have overlapping appointments; this is enforced at the service layer.
* Deleting a patient or doctor should be restricted if appointments exist, to preserve history.

---

### (Optional) Table: doctor_availability

Defines available working hours for each doctor, allowing flexible scheduling.

* id: BIGINT, Primary Key, Auto Increment
* doctor_id: BIGINT, NOT NULL, Foreign Key → doctors(id)
* day_of_week: INT, NOT NULL (1 = Monday … 7 = Sunday)
* start_time: TIME, NOT NULL
* end_time: TIME, NOT NULL

**Justification:** Availability is separated from appointments to avoid duplication and to support recurring schedules.

---

## MongoDB Collection Design

MongoDB is used for **flexible, schema-evolving data** that does not fit cleanly into rigid relational tables, such as prescriptions, notes, or logs.

### Collection: prescriptions

Prescriptions are tied to a specific appointment but may vary greatly in structure (number of medications, notes, metadata). MongoDB allows this flexibility without schema migrations.

```json
{
  "_id": "ObjectId('64abc123456')",
  "appointmentId": 51,
  "patient": {
    "id": 45,
    "name": "Fulan bin Fulan"
  },
  "doctor": {
    "id": 12,
    "name": "Dr. Fulan",
    "specialization": "Cardiology"
  },
  "medications": [
    {
      "name": "Paracetamol",
      "dosage": "500mg",
      "frequency": "Every 6 hours",
      "duration": "5 days"
    }
  ],
  "doctorNotes": "Take after meals. Monitor blood pressure.",
  "tags": ["painkiller", "short-term"],
  "issuedAt": "2026-01-10T09:30:00Z"
}
```

**Design considerations:**

* Only essential identifiers (appointmentId, patient.id, doctor.id) are stored to avoid full duplication.
* Nested objects and arrays allow prescriptions to evolve (e.g., adding lab results or attachments).
* New fields can be added without breaking existing data.

---

## Summary

* MySQL handles core clinic operations requiring consistency and relationships.
* MongoDB supports flexible medical and operational records.
* This hybrid schema supports scalability, auditability, and future feature growth (chat, feedback, uploads).
