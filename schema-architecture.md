# System Architecture - Hospital CMS

This document outlines the technical architecture of the Hospital CMS application, built with Spring Boot. It integrates both MVC and REST controllers, relational and document-based databases, and a modular service-oriented structure.

---

## Section 1: Architecture Overview

The application is developed using **Spring Boot**, combining:

- **MVC Controllers**: Used to render dynamic views with **Thymeleaf**, primarily for the Admin and Doctor dashboards.
- **REST Controllers**: Expose endpoints for modules such as appointments, patient records, and prescriptions, returning data in **JSON** format.

### Databases
- **MySQL**: Stores structured relational data such as patients, doctors, appointments, and administrative users. Accessed via **Spring Data JPA** using `@Entity`-annotated classes.
- **MongoDB**: Stores flexible document-based data such as prescriptions. Accessed via **Spring Data MongoDB** using `@Document`-annotated models.

### Layered Structure
- **Controllers** → **Services** → **Repositories** → **Databases**
- The service layer centralizes business logic and coordinates operations between controllers and repositories.

---

## Section 2: Data and Control Flow

### 1. **User Interaction**
Users access the system through:

- **Thymeleaf-based web dashboards**: `AdminDashboard`, `DoctorDashboard`
- **REST API clients**: `Appointments`, `PatientDashboard`, `PatientRecord`

### 2. **Request Handling**
- Web requests are routed to **MVC controllers**, which return `.html` templates populated with dynamic data and rendered in the browser.
- API requests are handled by **REST controllers**, which return structured **JSON** responses.

### 3. **Business Logic**
- **Service classes** encapsulate business rules, validations, and workflow coordination.
- Services invoke **repositories** to interact with persistent data.

### 4. **Data Persistence**
- **MySQL Repository**: Manages relational entities such as patients, doctors, appointments, and admin records.
- **MongoDB Repository**: Manages document-based entities such as prescriptions.

Each repository connects directly to its respective database engine.

### 5. **Data Mapping**
- Retrieved data is mapped to **Java model classes**:
  - In MySQL: JPA entities (`@Entity`) representing rows in relational tables.
  - In MongoDB: Document objects (`@Document`) mapped to BSON/JSON structures in collections.

### 6. **Client Response**
- In MVC flows: Models are passed from controllers to Thymeleaf templates and rendered as dynamic HTML.
- In REST flows: Models (or transformed DTOs) are serialized into JSON and returned to the client via HTTP responses.

