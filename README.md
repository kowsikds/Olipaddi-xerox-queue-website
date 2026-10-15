# Olipaddi Xerox Queue

> A virtual Xerox queue management system for college students.

## About

**Olipaddi** is the Xerox/photocopy centre in our college. Students currently have no way to check if the centre is open, whether machines are free, or how long the wait is.

This web app lets students:
- See real-time centre and machine status
- Upload a PDF document
- Join a virtual FCFS (First Come First Served) queue
- Track their job status from waiting → printing → completed

## Features

- **Centre Status** — See if Olipaddi is open or closed at a glance
- **Machine Status** — View which machines are free, printing, or offline
- **Upload Document** — Select a PDF, choose copies, color, single/double side
- **Virtual Queue** — FCFS queue with automatic job number assignment (OX001, OX002, etc.)
- **Job Tracking** — Track your position, people ahead, and estimated wait time
- **Admin Dashboard** — Open/close the centre, manage machines, process jobs
- **Mobile Responsive** — Works on desktop, tablet, and phone
- **Estimated Wait Time** — Simple calculation: ~5 seconds per page

## Technology

| Layer       | Technology            |
|-------------|----------------------|
| Framework   | React 19             |
| Build Tool  | Vite                 |
| Styling     | Tailwind CSS 4       |
| Icons       | Lucide React         |
| Routing     | React Router DOM     |
| Storage     | Browser localStorage |

## Installation

```bash
# Clone or download the project
cd olipaddi-xerox-queue

# Install dependencies
npm install

# Start development server
npm run dev
```

The app will be available at `http://localhost:5173`

## Production Build

```bash
npm run build
```

The built files will be in the `dist/` folder, ready to deploy.

## How the FCFS Queue Works

1. When a student uploads a document and clicks **Join Queue**, they receive a unique job number (e.g., `OX005`)
2. Jobs are processed in **First Come First Served** order — the earliest submitted job is processed first
3. When the admin marks a job as **Completed**, the next waiting job automatically becomes the next in line
4. No job can skip ahead of an earlier waiting job

### Example

```
Before:
  OX001 → PRINTING
  OX002 → WAITING
  OX003 → WAITING

Admin completes OX001:
  OX001 → COMPLETED
  OX002 → PRINTING  (auto-promoted)
  OX003 → WAITING
```

## How localStorage Is Used

This is Version 1 / MVP. All data is stored in the browser's `localStorage`:

| Key                      | Data                              |
|--------------------------|-----------------------------------|
| `olipaddi_centre_status` | Open/closed state                 |
| `olipaddi_machines`      | Machine list and statuses         |
| `olipaddi_queue`         | All queue jobs                    |
| `olipaddi_job_counter`   | Auto-incrementing job counter     |
| `olipaddi_my_job`        | Current user's job number         |

The storage logic is isolated in `src/services/storage.js` so it can be swapped with API calls later.

## Project Structure

```
src/
├── main.jsx                 # App entry point
├── App.jsx                  # Root component with routes
├── index.css                # Tailwind CSS import
├── context/
│   └── AppContext.jsx       # Global state management
├── services/
│   └── storage.js           # localStorage abstraction layer
├── utils/
│   └── helpers.js           # Utility functions
├── components/
│   ├── Header.jsx           # Navigation header
│   ├── Footer.jsx           # Page footer
│   ├── StatusCard.jsx       # Centre status display
│   ├── MachineCard.jsx      # Individual machine status
│   ├── QueueList.jsx        # Queue display list
│   ├── UploadForm.jsx       # Document upload form
│   ├── JobStatusCard.jsx    # Job tracking card
│   └── ProgressIndicator.jsx# Step progress display
└── pages/
    ├── HomePage.jsx         # Landing/dashboard page
    ├── QueueStatusPage.jsx  # Full queue view
    ├── MyJobPage.jsx        # Job tracking page
    ├── UploadPage.jsx       # Upload document page
    └── AdminPage.jsx        # Admin dashboard
```

## Future Cloud Architecture

This project is designed to be easily migrated to a cloud stack:

| Service         | Planned Technology                      |
|----------------|-----------------------------------------|
| Frontend Host  | Vercel / Netlify                        |
| Backend API    | Node.js on Render / Railway             |
| Database       | Supabase PostgreSQL / Neon PostgreSQL   |
| File Storage   | Supabase Storage / Cloudinary / S3      |
| Auth           | Supabase Auth / NextAuth               |
| Real-time      | Supabase Realtime / WebSockets         |

The `storage.js` service layer is designed to be replaced with real API calls when the backend is ready.

## License

MIT
