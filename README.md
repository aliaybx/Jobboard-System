# JobBoard - PHP Job Portal Application

<p align="center">
  <img src="https://img.shields.io/badge/PHP-8.0+-777BB4?style=for-the-badge&logo=php&logoColor=white" alt="PHP Version">
  <img src="https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL">
  <img src="https://img.shields.io/badge/Bootstrap-5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white" alt="Bootstrap">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License">
</p>

> ⚠️ **IMPORTANT SECURITY NOTICE**: This codebase contains critical security vulnerabilities. **DO NOT use in production** without addressing the issues outlined in this README. See the Security Audit section for details.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Security Audit](#security-audit) ⛔
- [Code Quality Issues](#code-quality-issues)
- [Performance Issues](#performance-issues)
- [Recommendations for Production](#recommendations-for-production)
- [Roadmap](#roadmap)
- [License](#license)

---

## 🎯 Project Overview

JobBoard is a full-featured job portal application built with PHP, MySQL, and Bootstrap. It allows companies to post jobs and workers to search/apply for positions.

**Current Version:** 1.0.0  
**Author:** Ali Ayoub  
**GitHub:** [aliaybx/Jobboard-System](https://github.com/aliaybx/Jobboard-System)

---

## ✨ Features

### For Companies
- [ ] Company registration and login
- [ ] Post new job listings
- [ ] Manage job postings (edit/delete)
- [ ] View applicants for each job
- [ ] Company public profile

### For Workers/Job Seekers
- [ ] User registration and login
- [ ] Search jobs by title, region, and type
- [ ] View job details
- [ ] Apply for jobs
- [ ] Save favorite jobs
- [ ] Upload CV/Resume
- [ ] Public profile management

### Admin Features
- [ ] Admin dashboard
- [ ] Manage categories
- [ ] Manage jobs (approve/reject)
- [ ] View all users

### General Features
- [ ] Job search with filters
- [ ] Trending keywords
- [ ] Responsive design (Bootstrap 5)
- [ ] Job sharing (Facebook, Twitter, LinkedIn)

---

## 🛠 Tech Stack

| Component | Technology |
|-----------|------------|
| **Backend** | PHP 7.4+ (原生 PHP, 无框架) |
| **Database** | MySQL / MariaDB |
| **ORM/Query Builder** | PDO (原生) |
| **Frontend** | HTML5, CSS3, JavaScript |
| **CSS Framework** | Bootstrap 5 |
| **Icons** | IcoMoon, Line Icons |
| **Server** | Apache (XAMPP) |

---

## 📂 Project Structure

```
jobboard-final/
├── admin-panel/           # Admin dashboard
│   ├── admins/           # Admin management
│   ├── categories-admins/ # Category management
│   ├── jobs-admins/      # Job management
│   └── layouts/          # Admin layout templates
├── auth/                 # Authentication (login/register/logout)
├── config/               # Database configuration
├── css/                  # Stylesheets
├── fonts/                # Icon fonts
├── gerneral/            # General pages (companies/workers)
├── images/              # Image assets
├── includes/            # Shared includes (header/footer)
├── jobs/                # Job-related pages
├── js/                  # JavaScript files
├── scss/                # SCSS source files
├── SQL_FILE/            # Database SQL dump
├── users/               # User profile management
├── 404.php              # 404 error page
├── about.php            # About page
├── contact.php          # Contact page
├── index.php            # Home page
└── search.php           # Search results page
```

---

## 🚀 Installation

### Prerequisites
- PHP 7.4 or higher
- MySQL 5.7+ or MariaDB
- XAMPP/WAMP/LAMP stack

### Setup Steps

1. **Clone the repository**
   ```bash
   
   ```

2. **Start Apache and MySQL** in XAMPP

3. **Create the database**
   ```bash
   # Open phpMyAdmin
   # Create a new database named 'jobboard'
   # Import SQL_FILE/jobboard.sql
   ```

4. **Configure database connection**
   Edit `config/config.php`:
   ```php
   $host = "localhost";
   $dbname = "jobboard";
   $user = "root";        // Change this
   $pass = "";            // Change this
   ```

5. **Update APPURL in header.php**
   Edit `includes/header.php`:
   ```php
   define("APPURL", "http://localhost/php/jobboard");
   ```

6. **Access the application**
   - Frontend: http://localhost/php/jobboard
   - Admin Panel: http://localhost/jobboard/admin-panel
   - Admin Login: admin1@admin.com / (password: admin123)

---

## ⚠️ SECURITY AUDIT - CRITICAL

> ⚠️ **WARNING**: This codebase has **CRITICAL** security vulnerabilities. **DO NOT deploy to production** without fixing these issues.

### Critical Vulnerabilities Found

#### 1. SQL Injection - CRITICAL 🔴

**Risk Level:** HIGH  
**Files Affected:** Multiple files

**Vulnerable Code Example (auth/login.php):**
```php
// VULNERABLE - Direct user input in SQL query
$email = $_POST['email'];
$login = $conn->query("SELECT * FROM users WHERE email = '$email'");
```

**Secure Version:**
```php
// SECURE - Using prepared statements
$email = $_POST['email'];
$login = $conn->prepare("SELECT * FROM users WHERE email = :email");
$login->execute([':email' => $email]);
$select = $login->fetch(PDO::FETCH_ASSOC);
```

**Files with SQL Injection:**
- `auth/login.php` (line 29)
- `auth/register.php` (line 32)
- `admin-panel/admins/login-admins.php` (line 20)
- `users/update-profile.php` (line 28)
- `search.php` (line 19)
- `jobs/job-single.php` (multiple locations)
- And more...

---

#### 2. Cross-Site Scripting (XSS) - HIGH 🟠

**Risk Level:** HIGH  
**Files Affected:** Multiple files

**Vulnerable Code:**
```php
// VULNERABLE - Outputting user input without sanitization
echo $job->job_title;
echo $_SESSION['username'];
```

**Secure Version:**
```php
// SECURE - Using htmlspecialchars
echo htmlspecialchars($job->job_title, ENT_QUOTES, 'UTF-8');
echo htmlspecialchars($_SESSION['username'], ENT_QUOTES, 'UTF-8');
```

---

#### 3. Unrestricted File Upload - CRITICAL 🔴

**Risk Level:** HIGH  
**Files Affected:** `users/update-profile.php`

**Vulnerable Code:**
```php
// VULNERABLE - No file type validation
$dir_img = 'user-images/' . basename($img);
move_uploaded_file($_FILES['img']['tmp_name'], $dir_img);
```

**Secure Version:**
```php
// SECURE - With validation
$allowed_types = ['image/jpeg', 'image/png', 'image/gif'];
$max_size = 2 * 1024 * 1024; // 2MB

if (!in_array($_FILES['img']['type'], $allowed_types)) {
    die("Invalid file type");
}
if ($_FILES['img']['size'] > $max_size) {
    die("File too large");
}
$img = basename($_FILES['img']['name']);
$dir_img = 'user-images/' . $img;
```

---

#### 4. Missing CSRF Protection - MEDIUM 🟡

**Risk Level:** MEDIUM  
**Description:** No CSRF tokens on forms

**Recommendation:** Implement CSRF tokens for all POST forms:
```php
// Generate token
$_SESSION['csrf_token'] = bin2hex(random_bytes(32));

// Validate token
if (!hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'])) {
    die("CSRF validation failed");
}
```

---

#### 5. Session Security Issues - MEDIUM 🟡

**Risk Level:** MEDIUM  
**Files Affected:** `includes/header.php`

**Issues Found:**
- No session cookie configuration
- No session timeout
- `session_start()` called in include files (should be at top)

**Secure Version:**
```php
// At the very top of EVERY page
ini_set('session.cookie_httponly', 1);
ini_set('session.cookie_secure', 1);
ini_set('session.use_strict_mode', 1);
session_start();

// Session timeout
if (isset($_SESSION['LAST_ACTIVITY']) && (time() - $_SESSION['LAST_ACTIVITY'] > 1800)) {
    session_unset();
    session_destroy();
}
$_SESSION['LAST_ACTIVITY'] = time();
```

---

#### 6. Insecure Password Storage - LOW 🟢

**Status:** ✅ ACCEPTABLE  
**Note:** Using `password_hash()` with PASSWORD_DEFAULT is correct. However, consider upgrading to Argon2id for better security.

---

#### 7. Path Traversal - MEDIUM 🟡

**Risk Level:** MEDIUM  
**Files Affected:** `users/update-profile.php`

**Vulnerable Code:**
```php
// VULNERABLE - User can manipulate path
unlink("user-images/".$row->img."");
```

**Secure Version:**
```php
// SECURE - Validate path
$img_path = 'user-images/' . basename($row->img);
if (file_exists($img_path) && is_file($img_path)) {
    unlink($img_path);
}
```

---

#### 8. Information Disclosure - MEDIUM 🟡

**Risk Level:** MEDIUM  
**Files Affected:** `config/config.php`

**Issue:** Error messages expose database details

**Secure Version:**
```php
// In production, log errors instead of displaying
error_reporting(0);
ini_set('display_errors', 0);

try {
    $conn = new PDO("mysql:host=$host;dbname=$dbname", $user, $pass);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch(PDOException $e) {
    error_log($e->getMessage());
    die("Database connection failed. Please try again later.");
}
```

---

## 📊 Code Quality Issues

### 1. Mixed SQL Query Styles

**Issue:** Inconsistent use of prepared statements vs. direct queries

```php
// BAD - Direct query with string interpolation
$select = $conn->query("SELECT * FROM users WHERE id='$id'");

// GOOD - Prepared statements throughout
$select = $conn->prepare("SELECT * FROM users WHERE id = :id");
$select->execute([':id' => $id]);
```

---

### 2. Poor Error Handling

**Issue:** Using JavaScript alerts for validation instead of server-side

```php
// BAD
if(empty($_POST['email'])) {
    echo "<script>alert('some inputs are empty')</script>";
}

// GOOD
if(empty($_POST['email'])) {
    $errors[] = "Email is required";
}
// Then display errors properly
```

---

### 3. Code Repetition

**Issue:** Similar code blocks repeated across files

**Recommendation:** Create helper functions:
```php
// functions.php
function sanitize($data) {
    return htmlspecialchars(trim($data), ENT_QUOTES, 'UTF-8');
}

function isLoggedIn() {
    return isset($_SESSION['username']);
}

function isCompany() {
    return isset($_SESSION['type']) && $_SESSION['type'] === 'Company';
}
```

---

### 4. Hardcoded Values

**Issue:** Database credentials and URLs hardcoded

**Recommendation:** Use environment variables or config file:
```php
// config.php
define('DB_HOST', getenv('DB_HOST') ?: 'localhost');
define('APPURL', getenv('APP_URL') ?: 'http://localhost');
```

---

### 5. Missing Input Validation

**Issue:** No server-side validation of user inputs

**Recommendation:** Add validation:
```php
function validateEmail($email) {
    return filter_var($email, FILTER_VALIDATE_EMAIL);
}

function sanitizeString($str) {
    return preg_replace("/[^a-zA-Z0-9]/", "", $str);
}
```

---

### 6. Typographical Errors

**Issue:** Spelling errors in code and database
- `gerneral/` should be `general/`
- `benifits` should be `benefits`

---

### 7. No MVC Architecture

**Issue:** All logic mixed with views

**Recommendation:** Implement MVC pattern:
```
├── app/
│   ├── Controllers/
│   ├── Models/
│   ├── Views/
│   └── core/
├── public/
└── config/
```

---

## ⚡ Performance Issues

### 1. N+1 Query Problem

**Issue:** Multiple database queries in loops

```php
// BAD - Query inside loop
foreach($jobs as $job) {
    $company = $conn->query("SELECT * FROM users WHERE id = " . $job->company_id);
    // ...
}

// GOOD - Single query with JOIN
$jobs = $conn->query("SELECT j.*, u.username as company_name 
                      FROM jobs j 
                      JOIN users u ON j.company_id = u.id");
```

---

### 2. Missing Database Indexes

**Recommendation:** Add indexes to frequently queried columns:
```sql
ALTER TABLE jobs ADD INDEX idx_status (status);
ALTER TABLE jobs ADD INDEX idx_category (job_category);
ALTER TABLE jobs ADD INDEX idx_company (company_id);
ALTER TABLE job_applications ADD INDEX idx_worker (worker_id);
ALTER TABLE job_applications ADD INDEX idx_job (job_id);
```

---

### 3. No Caching

**Recommendation:** Implement caching:
```php
// Cache trending searches for 1 hour
$cache_key = 'trending_searches';
if (!$searches = cache_get($cache_key)) {
    $searches = $conn->query("SELECT...")->fetchAll();
    cache_set($cache_key, $searches, 3600);
}
```

---

### 4. Large File Uploads Not Handled

**Issue:** No chunked upload or size limits handled properly

---

### 5. No Pagination

**Issue:** All results loaded at once

**Recommendation:** Add pagination:
```php
$page = $_GET['page'] ?? 1;
$per_page = 10;
$offset = ($page - 1) * $per_page;

$jobs = $conn->prepare("SELECT * FROM jobs LIMIT :limit OFFSET :offset");
$jobs->bindValue(':limit', $per_page, PDO::PARAM_INT);
$jobs->bindValue(':offset', $offset, PDO::PARAM_INT);
$jobs->execute();
```

---

## 🎯 Recommendations for Production

### Immediate Actions Required

1. **Fix ALL SQL Injection vulnerabilities**
2. **Implement XSS protection (htmlspecialchars)**
3. **Add CSRF tokens to all forms**
4. **Secure session configuration**
5. **Add input validation**
6. **Configure error logging (not display)**

### Before Production Launch

1. [ ] Switch to MVC framework (Laravel recommended)
2. [ ] Implement proper authentication (use Laravel Breeze/Jetstream)
3. [ ] Add rate limiting
4. [ ] Implement HTTPS
5. [ ] Add database indexes
6. [ ] Add pagination
7. [ ] Implement caching
8. [ ] Add comprehensive error handling
9. [ ] Set up logging system
10. [ ] Add unit tests

### Recommended Framework Migration

If continuing with PHP, migrate to **Laravel**:

```bash
composer create-project laravel/laravel jobboard-laravel
```

Laravel provides:
- Built-in CSRF protection
- XSS protection via Blade templating
- SQL injection prevention via Eloquent
- Secure session handling
- Rate limiting
- Authentication scaffolding
- Role-based access control

---

## 🗺️ Roadmap

### Phase 1: Security Fixes (Immediate)
- [ ] Fix SQL injection in all files
- [ ] Add XSS protection
- [ ] Implement CSRF tokens
- [ ] Secure sessions

### Phase 2: Code Improvements
- [ ] Create helper functions
- [ ] Add input validation class
- [ ] Fix typos
- [ ] Implement error handling

### Phase 3: Performance
- [ ] Add database indexes
- [ ] Implement pagination
- [ ] Add caching layer

### Phase 4: Architecture
- [ ] Migrate to MVC pattern
- [ ] Add composer support
- [ ] Implement REST API

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## ⚠️ Disclaimer

**This code is provided for educational purposes only.** 

The security vulnerabilities identified in this codebase are real and serious. Do not use this code in any production environment without:

1. Fixing all identified security issues
2. Implementing proper security best practices
3. Conducting a comprehensive security audit
4. Performing thorough testing

The author assumes no liability for any damages resulting from the use of this code.

---

## 👏 Acknowledgments

- Bootstrap team for the CSS framework
- Colorlib for the original template
- All contributors

---

<p align="center">
  Made with ❤️ by <a href="https://github.com/Moha1234567890">Ali Ayoub</a>
</p>

