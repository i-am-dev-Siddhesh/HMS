# Hospital Management App Documentation

## Introduction

The Hospital Management App is a comprehensive solution designed to streamline the administrative and operational processes of healthcare facilities. This app caters to three main user roles - Admin, Patient, and Doctor. Each role is equipped with specific functionalities to enhance their experience and contribute to the efficient management of the hospital.

## User Roles

### 1. Admin

The Admin is granted full access to the system, enabling them to manage various aspects of the hospital. The key features for the Admin include:

- **Dashboard**: A centralized view displaying essential information like doctor appointments, patient appointments, and charges per doctor appointment.
- **Appointment Management**: The ability to view, update, and reschedule appointments.
- **Authentication System**: Secure login to the admin interface.

Admin Schema:

```graphql
model Admin {
  id           Int       @id @default(autoincrement())
  createdAt    DateTime  @default(now())
  updatedAt    DateTime  @updatedAt
  username     String    @unique
  email        String    @unique
  password     String
  profileUrl   String?
  role         String?   // Possible values to be clarified
}
```

### 2. Patient

Patients can efficiently manage their appointments and interact with healthcare professionals through the app. Key features for the Patient include:

- **Online Booking**: The ability to book appointments online.
- **Video Consultation**: A crucial feature for remote healthcare, allowing video consultations.
- **Prescription Upload and Review**: Patients can upload prescriptions, and doctors can review them.
- **Doctor Filtering**: Patients can filter doctors based on location, disease, or experience.
- **Authentication System**: Secure login to the patient interface.

Patient Schema:

```graphql
model Patient {
  id             Int       @id @default(autoincrement())
  age            Int
  name           String
  phoneNumber    String
  email          String    @unique
  password       String
  profileUrl     String?
  appointments   Appointment[]
  createdAt      DateTime  @default(now())
  updatedAt      DateTime  @updatedAt
}
```

### 3. Doctor

Doctors can efficiently manage their schedules and interact with patients through the app. Key features for the Doctor include:

- **Appointment Slots**: Ability to make available slots in a day.
- **Booking Management**: Accept or reject appointment requests, view patient details, and disease type.
- **Availability Tracking**: Maintain a record of available and unavailable dates.
- **Authentication System**: Secure login to the doctor interface.

Doctor Schema:

```graphql
model Doctor {
  id                Int       @id @default(autoincrement())
  name              String
  email             String    @unique
  password          String
  profileUrl        String?
  specialization    String
  appointments      Appointment[]
  unavailableDates  Availability[]
  availableSlots    Slot[]
  createdAt         DateTime  @default(now())
  updatedAt         DateTime  @updatedAt
}
```

## Additional Features

### Enum AppointmentStatus

An enumeration to represent the status of an appointment.

```graphql
enum AppointmentStatus {
  PENDING
  ACCEPTED
  REJECTED
}
```

### Appointment Schema

```graphql
model Appointment {
  id              Int                @id @default(autoincrement())
  createdAt       DateTime           @default(now())
  updatedAt       DateTime           @updatedAt
  patient         Patient            @relation(fields: [patientId], references: [id])
  patientId       Int
  doctor          Doctor             @relation(fields: [doctorId], references: [id])
  doctorId        Int
  appointmentTime DateTime
  notes           String?
  status          AppointmentStatus @default(PENDING)
}
```

### Availability and Slot Schemas

```graphql
model Availability {
  id        Int      @id @default(autoincrement())
  doctor    Doctor   @relation(fields: [doctorId], references: [id])
  doctorId  Int
  startDate DateTime
  endDate   DateTime
}

model Slot {
  id        Int      @id @default(autoincrement())
  doctor    Doctor   @relation(fields: [doctorId], references: [id])
  doctorId  Int
  startTime DateTime
  endTime   DateTime
  isAvailable Boolean  @default(true)
}
```

This comprehensive Hospital Management App aims to improve the overall efficiency of hospital operations, enhance patient-doctor interactions, and provide a seamless experience for all users involved.
