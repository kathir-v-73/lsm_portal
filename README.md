# Prime Vector Learning & Skill Management (LSM) Portal

This repository contains a production-quality, responsive, and enterprise-grade **Learning & Skill Management (LSM) Portal** designed and built for **Prime Vector**. It functions as an internship project demonstrating full-stack web application development capabilities using **Python Flask** and a modular, clean frontend architecture.

---

## 🏛️ Project Architecture & Tech Stack

This portal uses a clean, separation-of-concerns architecture designed to emulate enterprise SaaS platforms.

| Layer | Technology | Details |
|---|---|---|
| **Frontend** | HTML5, CSS3, Vanilla JS (ES6) | Responsive grid layouts, modern typography (Inter + Outfit), SVG icons, glassmorphism UI, custom micro-animations. |
| **Backend** | Python Flask | Structured with **Flask Blueprints** for modular route definition (`auth`, `student`, `mentor`, `admin`). |
| **Database** | JSON File Storage | Abstracted via a custom `DataManager` class to make the platform **MongoDB-ready** (ready to swap file I/O for `pymongo` statements). |
| **Libraries** | Chart.js | Renders dynamic analytics charts (monthly enrollments, course popularity, weekly activity, progress distribution). |

---

## 📁 Directory Structure

Here is an overview of the clean folder structure:

```text
lsm_portal/
├── app.py                      # Flask Application Factory & Server entry point
├── requirements.txt            # Python dependencies (Flask)
├── config/
│   └── config.py               # Development & Production settings
├── data/                       # JSON database collections (MongoDB collection equivalent)
│   ├── admins.json             # Admin credentials and profiles
│   ├── mentors.json            # Mentor profiles, specialties, and assigned courses
│   ├── students.json           # Student enrollment, batches, and progress data
│   ├── courses.json            # 15 courses offered by Prime Vector
│   ├── assignments.json        # Project/homework files and submissions
│   ├── announcements.json      # System-wide announcement history
│   ├── notifications.json      # Targeted alerts and events
│   └── dashboard.json          # Historical analytics data
├── routes/                     # Blueprint modules for routes & security decorators
│   ├── auth.py                 # Common authentication & login/logout logic
│   ├── student.py              # Student portal routes
│   ├── mentor.py               # Mentor portal routes
│   └── admin.py                # Admin portal CRUD and statistics routes
├── utils/
│   └── data_manager.py         # JSON Database CRUD abstraction layer (MongoDB-ready)
├── templates/                  # Jinja2 HTML templates
│   ├── base.html               # Base layout with global scripts, styles, and page loader
│   ├── landing.html            # Public-facing home page
│   ├── auth/                   # Student, Mentor, and Admin login pages
│   ├── student/                # Student dashboard view
│   ├── mentor/                 # Mentor dashboard view
│   └── admin/                  # Admin dashboard view
└── static/                     # Frontend assets
    ├── css/                    # Modular stylesheets (main, landing, dashboard, etc.)
    ├── js/                     # Client-side scripts (main, admin, auth, charts, etc.)
    └── images/                 # Logo and brand media assets
```

---

## 🚶 Step-by-Step Guide to Explaining the Project to Your Mentor

When presenting this project to your mentor, you can guide them through the user journey step-by-step:

### Step 1: Public Landing Page (`/`)
- Start by showing the **Landing Page**. Highlight the custom Prime Vector logo, navigation links, and animated stats counters showing metrics (e.g. 643+ students, 15 courses, 98% success rate).
- Explain that the landing page acts as a professional portal entrance with 3 direct cards to access login views for **Students**, **Mentors**, and **Admins**.

### Step 2: Role-based Authentication (`/login/<role>`)
- Demonstrate the login interfaces. Explain that the backend handles validation securely via Flask sessions. Show how:
  - **Super Admins** log in to manage the entire platform.
  - **Mentors** log in to view their courses, create assignments, and grade submissions.
  - **Students** log in to view progress, enroll in courses, and complete assignments.

### Step 3: Admin Management Dashboard (`/admin/dashboard`)
Show your mentor how the admin panel functions as a powerful control center:
1. **Overview Charts:** Displays monthly enrollments (Line Chart) and course popularity (Doughnut Chart) rendered dynamically using Chart.js.
2. **Student & Mentor Tables:** Displays list views. Click **"Add Student"** or **"Add Mentor"** to demonstrate the modal. Fill in the name, email, batch, and check which courses they are enrolled in. Save to demonstrate instant update.
3. **Courses Management:** Explains that the catalog displays exactly **15 expert-led upskilling courses** matching Prime Vector's real curriculum. Show how you can edit price, duration, category, and update assigned mentors.
4. **Admin Management:** Standard admins can be created or updated here to give system access to other employees.

### Step 4: Mentor Portal (`/mentor/dashboard`)
- Log in as a mentor (e.g., Gokul Anand).
- Show how the mentor dashboard displays only their assigned students and courses.
- Demonstrate **creating a new assignment** or **posting an announcement** that instantly targets the student portal.

### Step 5: Student Portal (`/student/dashboard`)
- Log in as a student (e.g., Arjun Sharma).
- Highlight the student profile stats: overall progress bar, grades (e.g., A+), and course-specific progress cards.
- Show the **Calendar Widget** displaying upcoming due dates and the announcements panel showing the targeted messages posted by admins and mentors.

---

## 🚀 How to Run the Project

Follow these steps to set up and run the application locally:

### 1. Prerequisites
Ensure you have Python 3.8+ installed on your computer.

### 2. Install Dependencies
Navigate to the project root directory in your terminal and install Flask:
```bash
pip install -r requirements.txt
```
*(If you do not have a requirements file, simply run: `pip install flask`)*

### 3. Run the Server
Start the Flask application by running:
```bash
python app.py
```

### 4. Open in Browser
Open your web browser and navigate to:
- **URL:** [http://127.0.0.1:5000](http://127.0.0.1:5000)

---

## ⚖️ Project Review: Benefits & Limitations

To impress your mentor, discuss the architectural trade-offs:

### 🌟 Benefits (Advantages)
1. **MongoDB-Ready Abstraction:** The custom `DataManager` class encapsulates all database queries (`find_one`, `find_many`, `create`, `update`, `delete`). If the project transitions to a database like MongoDB in the future, you only need to change the statements inside `data_manager.py` without touching a single line of your routing logic (`routes/*.py`).
2. **True Single Page Experience (SPA):** Section switching on the dashboards is handled via client-side CSS and JS display transitions without trigger page reloads, ensuring a smooth, premium experience.
3. **Dynamic Charting:** Dashboard graphs dynamically fetch database counts instead of using static mock values, keeping analytics in sync with actual collection items.
4. **Branded Micro-Animations:** Incorporates CSS animations, progress bar fills (using `IntersectionObserver`), and animated stats counters to establish a modern product aesthetic.

### ⚠️ Disadvantages (Limitations)
1. **Concurrency Limitations (JSON-based):** Reading and writing directly to JSON files is not thread-safe. If multiple administrators write data at the exact same millisecond, race conditions might occur. (Explain that this is by design for the local internship version and is resolved by swapping JSON files with a real database like MongoDB or PostgreSQL).
2. **Password Security:** Passwords are currently stored in plain text inside the JSON files for simple review and testing. In a full production environment, these must be hashed using a library like `bcrypt` or `werkzeug.security` before storing them.
3. **Session State Storage:** Session tokens are stored in the client-side browser cookie using Flask's default client-side sessions (cryptographically signed). For high-security environments, server-side session stores (like Redis) should be introduced.
