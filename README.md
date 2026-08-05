# MediLink 🩺

> **MediLink** is an end-to-end Telehealth Platform engineered to connect patients and healthcare professionals. It offers real-time video consultations, seamless appointment scheduling, secure messaging, medical record management, and interactive pharmacy location services.

---

## Key Features

###  For Healthcare Professionals (Doctors)
- **Practice Dashboard**: Real-time overview of daily appointments, patient queue, pending reviews, and recent activity.
- **Availability Manager**: Flexible calendar-based configuration for setting custom recurring or date-specific consultation slots.
- **Patient Directory**: Comprehensive directory containing full patient medical histories, contact info, allergies, and lab records.
- **HD Teleconsultation**: One-click encrypted video calls powered by Twilio Video WebRTC.
- **Practice Analytics**: Interactive visual insights into appointment trends, completion rates, and patient volume powered by Recharts.

### For Patients
- **Smart Appointment Booking**: Browse licensed medical specialists, view real-time availability, and book slots instantly.
- **Digital Health Records**: Securely upload, view, and download medical records, prescriptions, and lab reports.
- **Doctor Chat**: Encrypted direct messaging for follow-ups and pre-consultation inquiries.
- **Pharmacy Locator**: Interactive map integration using Google Maps Places API to locate open nearby pharmacies and calculate walking distances.
- **Personalized Profile**: Manage health details, emergency contacts, and notification preferences.

### Security & Authentication
- **Multi-Factor Authentication**: Email OTP verification alongside standard password authentication.
- **Google OAuth 2.0 Integration**: Single Sign-On via Google accounts.
- **Role-Based Access Control (RBAC)**: Strict route protection isolating doctor workspace features from patient features.

---

## Repository Architecture

The codebase is organized as a multi-package workspace:

```text
medilink/
├── backend/            # Express.js REST API Server
│   ├── config/         # Database, Passport, Cloudinary, & Twilio configurations
│   ├── controllers/    # API logic for Users, Appointments, Records, Messages, Video
│   ├── middlewares/    # Authentication & Role-based authorization middleware
│   ├── models/         # MongoDB (Mongoose) Data Models
│   ├── routes/         # REST API Route definitions
│   └── server.js       # Entry point for backend server
│
├── frontend/           # React 18 Web Client
│   ├── public/         # Static assets and HTML template
│   └── src/
│       ├── api/        # Axios API service interfaces
│       ├── components/ # Shared UI components (Sidebar, Navbar, Modals)
│       ├── context/    # React Auth Context & State Management
│       ├── pages/      # Route pages grouped by domain (Doctor, Patient, Auth, Home)
│       └── routes/     # Protected and Public route configuration
│
└── video-app/          # Dedicated Twilio Video WebRTC Client Application
    ├── scripts/        # Webpack and build scripts
    ├── server/         # Express token authorization backend for video sessions
    └── src/            # React + TypeScript video room components
```

---

## Tech Stack

| Domain | Technologies |
| :--- | :--- |
| **Frontend** | React 18, React Router v6, Tailwind CSS, Lucide Icons, Recharts, Formik, Yup |
| **Backend** | Node.js, Express.js, MongoDB, Mongoose ORM |
| **Authentication** | JSON Web Tokens (JWT), Passport.js (Google OAuth 2.0), OTP Verification |
| **Cloud & Media** | Cloudinary (Record Storage), Twilio Video SDK (WebRTC Telehealth Rooms) |
| **Maps & Location**| `@react-google-maps/api`, Google Places API |

---

## Getting Started

### Prerequisites
- **Node.js** >= 18.x
- **npm** >= 9.x
- **MongoDB** instance (Local or MongoDB Atlas)

---

### Environment Setup

#### 1. Backend Environment Setup (`backend/.env`)
Create a `.env` file in the `backend/` directory:

```env
PORT=5050
MONGO_URI=mongodb://localhost:27017/medilink
JWT_SECRET=your_jwt_secret_key

# Email Service
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_email_app_password

# Google OAuth
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
GOOGLE_CALLBACK_URL=http://localhost:5050/api/auth/google/callback
CLIENT_URL=http://localhost:3000

# Cloudinary
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# Twilio
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_API_KEY=your_twilio_api_key
TWILIO_API_SECRET=your_twilio_api_secret
```

#### 2. Frontend Environment Setup (`frontend/.env`)
Create a `.env` file in the `frontend/` directory:

```env
REACT_APP_BACKEND_BASE_URL=http://localhost:5050/api
```

---

### Installation & Local Development

#### 1. Start the Backend API Server
```bash
cd backend
npm install
npm start
```
*Server will start on `http://localhost:5050`*

#### 2. Start the Frontend Client
```bash
cd frontend
npm install
npm start
```
*App will launch on `http://localhost:3000`*

#### 3. Start the Video Client (Optional for Teleconsultation Testing)
```bash
cd video-app
npm install
npm start
```

---

## API Endpoint Summary

| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Register new Patient or Doctor account | Public |
| `POST` | `/api/auth/login` | Authenticate credentials & send OTP | Public |
| `POST` | `/api/auth/verify-otp` | Validate OTP & issue JWT token | Public |
| `GET`  | `/api/users/patients` | Fetch list of patients | Doctor |
| `GET`  | `/api/appointments/doctor/:id` | Fetch appointments for a doctor | Doctor |
| `POST` | `/api/appointments/book` | Book a consultation slot | Patient |
| `GET`  | `/api/availability/doctor/:id` | Get availability matrix | Private |
| `POST` | `/api/availability` | Create new availability slot | Doctor |
| `GET`  | `/api/records/:patientId` | Fetch medical records | Private |
| `POST` | `/api/records/:patientId` | Upload health record to Cloudinary | Doctor/Patient |
| `POST` | `/api/messages/send` | Send encrypted direct chat message | Private |
| `GET`  | `/api/video/token` | Generate Twilio Video Access Token | Private |

---

## Production Build & CI Verification

To run production builds under Continuous Integration constraints:

```bash
# Build Frontend Client
cd frontend
CI=true npm run build

# Build Video Teleconsultation App
cd ../video-app
CI=true npm run build
```

---

## License

This project is licensed under the [MIT License](LICENSE).
