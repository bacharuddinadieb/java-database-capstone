# Smart Clinic Management System — User Stories

This document defines the core user stories for the Smart Clinic Management System. The stories are grouped by user role and written to guide feature development, API design, and access control.

---

## Admin User Stories

**As an Admin**, I want to manage system users and configurations so that the clinic operates securely and efficiently.

1. As an Admin, I want to create, update, and deactivate doctor accounts so that only authorized doctors can access the system.
2. As an Admin, I want to create, update, and deactivate patient accounts so that patient access is controlled and accurate.
3. As an Admin, I want to assign roles and permissions so that users can only access features relevant to their responsibilities.
4. As an Admin, I want to view all appointments across doctors and patients so that I can monitor clinic operations.
5. As an Admin, I want to manage system settings (clinic hours, appointment duration, availability rules) so that scheduling follows clinic policies.
6. As an Admin, I want to lock or unlock user accounts so that security incidents can be handled quickly.

---

## Patient User Stories

**As a Patient**, I want to manage my appointments and view my health-related information easily and securely.

1. As a Patient, I want to register and log in securely so that my personal and medical data is protected.
2. As a Patient, I want to view available doctors and their schedules so that I can choose a suitable appointment time.
3. As a Patient, I want to book an appointment with a doctor so that I can receive medical consultation.
4. As a Patient, I want to reschedule or cancel my appointments so that I can manage changes in my availability.
5. As a Patient, I want to view my upcoming and past appointments so that I can track my visit history.
6. As a Patient, I want to view my prescriptions and medical notes so that I can follow treatment instructions correctly.

---

## Doctor User Stories

**As a Doctor**, I want to manage my availability and patient appointments so that I can deliver care efficiently.

1. As a Doctor, I want to log in securely so that only I can access my schedule and patient information.
2. As a Doctor, I want to define and update my availability so that patients can only book valid time slots.
3. As a Doctor, I want to view my daily and weekly appointment schedule so that I can plan my work effectively.
4. As a Doctor, I want to view patient details associated with an appointment so that I am prepared for consultations.
5. As a Doctor, I want to create and update prescriptions for patients so that treatment records are properly documented.
6. As a Doctor, I want to mark appointments as completed or canceled so that appointment statuses remain accurate.

---

## Notes

* These user stories will be used to guide backend API design, frontend development, and role-based access control (RBAC).
* Additional stories may be added as the system evolves (e.g., notifications, reports, integrations).
