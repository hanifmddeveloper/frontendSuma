# 🩺 MediPulse: Doctor & Patient Care Management Platform

A modern, full-stack medical practice web application built with **Next.js 14 (App Router)** on the frontend and **Node.js, Express, MongoDB & Mongoose** on the backend.

Designed for doctors and clinics to offer comprehensive, 360-degree medical services to their patients, including dynamic appointment booking, digital prescriptions (Rx) with PDF printing, health records tracking, patient reviews, and doctor management workspaces.

---

## 🌟 Key Features

### 👩‍⚕️ For the Doctor & Clinic
- **Lead Doctor Profile & Credentials**: Specializations, certifications, affiliations, consultation fees, and working hours.
- **Doctor Workspace Dashboard**:
  - Live metric counters: Total Patients, Today's Appointments, Pending Approval, Completed Consultations, Total Prescriptions, Revenue.
  - Appointment Management: Filter by status (`all`, `pending`, `confirmed`, `completed`, `cancelled`), approve or reschedule slots.
  - Digital Prescription (Rx) Issuer: Add clinical diagnoses, record patient vitals (BP, pulse, temp, weight), prescribe medications with structured frequency/dosage schedules, advice, and recommended laboratory tests.
  - Patient Inquiries & Messaging: Read patient queries and transmit direct clinical replies.
  - Catalog Management: Configurable medical services with pricing and durations.

### 🧑 For Patients
- **Interactive 3-Step Booking Wizard**:
  - Select service & format (**In-Person Clinic Visit** or **Telemedicine HD Video Call**).
  - Dynamic slot checker: automatically prevents double-booking and checks real-time slot availability.
  - Instant appointment confirmation & portal tracking.
- **Patient Health Portal (`/portal`)**:
  - Track upcoming & past appointments with status badges.
  - Digital Prescriptions (Rx) viewer with a printable/saveable medical certificate modal.
  - Medical Profile & History: Blood group, known allergies, chronic conditions, emergency contact.
  - Ask Doctor non-urgent inquiries and review past replies.
  - Submit verified feedback & testimonials with star ratings.

### ⚡ Out-of-the-Box Zero-Config Database
- Supports standard MongoDB (`mongodb://localhost:27017/doctor_portal` or MongoDB Atlas).
- **Embedded In-Memory Fallback**: If no local MongoDB service is currently running, the server automatically boots an in-memory MongoDB instance and auto-seeds the doctor account, 8 services, sample appointments, and prescriptions!

---

## 🔑 Demo Accounts (Ready to Test)

| Role | Email | Password | Access |
| :--- | :--- | :--- | :--- |
| **👩‍⚕️ Lead Doctor** | `doctor@drjenkinsclinic.com` | `Password123!` | `/doctor` (Doctor Workspace) |
| **🧑 Demo Patient** | `patient@example.com` | `Password123!` | `/portal` (Patient Portal) |

*(Quick 1-Click buttons for both accounts are available on the [Login Page](http://localhost:3000/login)!)*

---

## 🏗️ Architecture & Tech Stack

```
projectOne/
├── backend/                  # Node.js + Express REST API + Mongoose
│   ├── src/
│   │   ├── config/db.js      # MongoDB connection with in-memory fallback
│   │   ├── models/           # Mongoose Models (User, Service, Appointment, Prescription, Message, Review)
│   │   ├── controllers/      # Business logic & endpoints
│   │   ├── routes/           # REST API routes
│   │   ├── middleware/       # JWT Auth & centralized error handling
│   │   ├── seeds/seed.js     # Realistic medical database seeder
│   │   └── server.js         # Express app entrypoint
│   └── package.json
│
├── frontend/                 # Next.js 14 (App Router) + Tailwind CSS + Lucide Icons
│   ├── src/
│   │   ├── app/              # Next.js App Router pages
│   │   │   ├── page.tsx      # Landing page / Hero / Services / Reviews
│   │   │   ├── services/     # Full searchable medical services catalog
│   │   │   ├── book-appointment/ # 3-step dynamic booking wizard
│   │   │   ├── portal/       # Patient Health Portal (Rx, Appointments, Profile)
│   │   │   ├── doctor/       # Doctor Workspace & Dashboard
│   │   │   ├── login/        # Sign In (with 1-Click Demo buttons)
│   │   │   ├── register/     # Patient Registration
│   │   │   ├── about/        # Doctor Biography & Credentials
│   │   │   └── contact/      # Clinic Location, Hotline & Inquiries
│   │   ├── components/       # Navbar, Footer, ServiceCard, PrescriptionModal, etc.
│   │   ├── context/          # AuthContext (JWT & session state)
│   │   └── lib/api.ts        # Axios API client with token interceptor
│   └── package.json
│
├── package.json              # Root package to run both concurrently
└── README.md
```

---

## 🚀 Quick Start Guide

### 1. Install Dependencies
```bash
# In the root projectOne directory
npm run install:all
```
*(Or run `npm install` inside `backend/` and `frontend/` individually)*

### 2. Seed Initial Database (Optional)
To pre-populate the doctor profile, services, sample appointments, and reviews:
```bash
npm run seed
```

### 3. Start Both Frontend & Backend Concurrently
```bash
npm run dev
```

- **Frontend App**: [http://localhost:3000](http://localhost:3000)
- **Backend API**: [http://localhost:5000/api](http://localhost:5000/api)
- **API Health Check**: [http://localhost:5000/api/health](http://localhost:5000/api/health)

---

## 📡 REST API Summary

### Authentication (`/api/auth`)
- `POST /api/auth/register` - Create patient account
- `POST /api/auth/login` - Authenticate & receive JWT
- `GET /api/auth/me` - Get current session
- `PUT /api/auth/profile` - Update patient/doctor profile
- `GET /api/auth/doctor-profile` - Lead doctor public details

### Medical Services (`/api/services`)
- `GET /api/services` - List active services & treatments
- `GET /api/services/:idOrSlug` - Get service details
- `POST /api/services` - Doctor creates service

### Appointments (`/api/appointments`)
- `GET /api/appointments/available-slots?date=YYYY-MM-DD` - Check open slots
- `POST /api/appointments` - Book appointment
- `GET /api/appointments/my-appointments` - Current patient appointments
- `GET /api/appointments` - All appointments (Doctor view)
- `PUT /api/appointments/:id/status` - Update status (`confirmed`, `completed`, `cancelled`)
- `PUT /api/appointments/:id/cancel` - Cancel appointment

### Prescriptions (`/api/prescriptions`)
- `POST /api/prescriptions` - Doctor issues digital prescription
- `GET /api/prescriptions/my-prescriptions` - Patient's prescriptions
- `GET /api/prescriptions` - Doctor's issued prescriptions
- `GET /api/prescriptions/:id` - View single prescription

### Messages & Reviews (`/api/messages`, `/api/reviews`, `/api/stats`)
- `POST /api/messages` - Submit contact inquiry
- `GET /api/messages` - Doctor views inquiries
- `PUT /api/messages/:id/reply` - Doctor sends reply
- `GET /api/reviews` - Public patient reviews
- `POST /api/reviews` - Submit review
- `GET /api/stats/dashboard` - Doctor KPI metrics
