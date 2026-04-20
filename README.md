# 📋 AttendanceApp

> A web-based attendance management system for faculty — take attendance, track defaulters, send email notifications, and download reports, all from one interface.

![PHP](https://img.shields.io/badge/PHP-8.x-777BB4?logo=php&logoColor=white)
![jQuery](https://img.shields.io/badge/jQuery-3.x-0769AD?logo=jquery&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5-7952B3?logo=bootstrap&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-Grid%20%2F%20Flexbox-1572B6?logo=css3&logoColor=white)

---

## 📖 Overview

**AttendanceApp** is a responsive, session-based web application designed for faculty members to manage student attendance efficiently. Faculty can log in securely, select a session and course, mark attendance with a single click per student, view cumulative attendance statistics, identify defaulter students, notify them via email, and download both summary and detailed attendance reports as CSV files.

---

## ✨ Features

- 🔐 **Secure Login** — Session-based authentication with animated login UI and loading screen
- 📅 **Session & Course Selection** — Dynamically loads academic sessions and faculty-assigned courses from the database
- ✅ **One-click Attendance** — Mark students present/absent with instant AJAX saving; rows update color in real time (blue = present, pink = absent)
- 📊 **Attendance Statistics** — Shows classes attended and attendance percentage per student
- ⚠️ **Defaulter Detection** — Enter a cut-off percentage to instantly list students below the threshold
- 📧 **Automated Email Notifications** — Send attendance shortage emails to all defaulters in one click; tracks how many times each student has been notified
- 📥 **Report Downloads** — Download a **Summary Report** or **Detailed Date-wise Report** as CSV files
- 📱 **Fully Responsive** — Mobile-first CSS with breakpoints at 600px and 767px; grid layouts reflow for all screen sizes
- 🎨 **Polished Animations** — Page fade-in, tilt animation on course cards, blinking login glow, error message fall-down effect

---



## 🏗️ Project Structure

```
attendanceapp/
├── login.php                    # Login page
├── attendance.php               # Main attendance dashboard (protected)
├── email.php                    # Basic email test script
│
├── css/
│   ├── login.css                # Login form styles & animations
│   ├── attendance.css           # Dashboard layout, cards, student rows
│   └── loader.css               # Spinner / lockscreen overlay
│
├── js/
│   ├── login.js                 # Login form validation & AJAX call
│   ├── attendance.js            # All dashboard logic & AJAX calls
│   └── jquery.js                # jQuery library
│
├── ajaxhandler/                 # Server-side AJAX endpoints (PHP)
│   ├── loginAjax.php            # Authenticates user, sets session
│   ├── logoutAjax.php           # Destroys session
│   └── attendanceAJAX.php       # Core handler for all attendance actions
│
└── reports/                     # Generated CSV report files (served to browser)
```

---

## ⚙️ How It Works

### Authentication Flow

```
Login Page → loginAjax.php (POST) → Session set → Redirect to attendance.php
                                  → Error message displayed on failure
```

### Attendance Dashboard Flow

```
Load Sessions (ddlclass dropdown)
    → Select Session → Load Course Cards
        → Click Course Card → Load Class Details + Student List
            → Check/Uncheck Checkbox → saveattendance (AJAX, instant)
```

### Defaulter & Email Flow

```
Click "NOTIFY ATTENDANCE SHORTAGE"
    → Enter cut-off % → Click GO
        → Defaulter list displayed with stats & notification count
            → Click "SEND EMAIL" → Emails sent to all defaulters
```

### Report Download Flow

```
Click "REPORT"
    → Click "DOWNLOAD SUMMARY"   → downloadSummaryReport (AJAX) → CSV served
    → Click "DOWNLOAD DETAILS"   → downloadDetailsReport (AJAX) → Date-wise CSV served
```

---

## 📡 AJAX API Reference

All requests go to `ajaxhandler/attendanceAJAX.php` via `POST`. The `action` field determines the operation.

| Action | Description | Key Parameters |
|---|---|---|
| `getSession` | Fetch academic sessions | — |
| `getFacultyCourses` | Get courses for a faculty in a session | `facid`, `sessionid` |
| `getStudentList` | Fetch students with attendance for a date | `facid`, `sessionid`, `classid`, `ondate` |
| `saveattendance` | Save one student's attendance | `studentid`, `courseid`, `facultyid`, `sessionid`, `ondate`, `ispresent` |
| `getdefaulterStudentList` | List students below cut-off % | `facid`, `sessionid`, `classid`, `cutoff` |
| `sendEmailToDefaulterStudents` | Send shortage emails | `facid`, `sessionid`, `classid`, `cutoff` |
| `downloadSummaryReport` | Generate summary CSV | `facid`, `sessionid`, `classid` |
| `downloadDetailsReport` | Generate date-wise CSV | `facid`, `sessionid`, `classid` |

---

## 📊 Sample Report Format

The generated CSV report looks like this:

```
Session:, 2023 AUTUMN SEMESTER
Course:,  CO321-Database management system lab
Faculty:, Pallabi
total,    8
start,    06-04-2024
end,      29-04-2024

slno, roll_no,   name,              06-04, 17-04, ..., attended, percent
1,    CSB21003,  Ryan Thompson,     P,     P,     ..., 8,        100
2,    CSB21005,  Daniel Brown,      P,     P,     ..., 7,        87.5
...
```

---

## 🚀 Getting Started

### Prerequisites

- PHP 8.x
- MySQL / MariaDB
- A web server — Apache (XAMPP / WAMP / LAMP) or Nginx
- PHP `mail()` configured (for email notifications)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/attendanceapp.git
   ```

2. **Move to your web server's root directory**
   ```bash
   # Example for XAMPP on Windows
   mv attendanceapp C:/xampp/htdocs/

   # Example for Linux
   mv attendanceapp /var/www/html/
   ```

3. **Import the database**
   - Create a MySQL database (e.g., `attendancedb`)
   - Import the provided `.sql` schema file:
     ```bash
     mysql -u root -p attendancedb < database/schema.sql
     ```

4. **Configure the database connection**
   - Open `ajaxhandler/db.php` (or equivalent config file) and update:
     ```php
     $host = "localhost";
     $dbname = "attendancedb";
     $username = "root";
     $password = "your_password";
     ```

5. **Configure email** *(optional — for defaulter notifications)*
   - Ensure PHP `mail()` is enabled on your server, or configure an SMTP library like PHPMailer

6. **Open in browser**
   ```
   http://localhost/attendanceapp/login.php
   ```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3 (Grid + Flexbox), JavaScript |
| UI Library | Bootstrap 5, jQuery 3 |
| Backend | PHP 8 (session-based, AJAX handlers) |
| Database | MySQL / MariaDB |
| Animations | Pure CSS keyframe animations |

---

## 🤝 Contributing

Contributions are welcome! Please open an issue first to discuss what you'd like to change, then submit a pull request.

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m 'Add my feature'`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
