# JobBoard Pro - Professional Job Portal System

A full-featured job board platform built with PHP, MySQL, and Bootstrap. This application enables companies to post jobs and job seekers to search, apply, and save job listings.

## 🚀 Features

### For Job Seekers
- 🔍 **Advanced Job Search** - Search jobs by title, category, location, and type
- 📄 **Job Applications** - Apply to multiple jobs with resume/CV upload
- ⭐ **Save Jobs** - Save interesting jobs for later viewing
- 👤 **Public Profile** - Create and manage your professional profile
- 📊 **Application Tracking** - View all your submitted applications

### For Companies
- 📝 **Post Jobs** - Create detailed job listings with comprehensive details
- 👥 **Manage Applicants** - View and manage all applicants for your listings
- 🏢 **Company Profile** - Showcase your company to potential candidates
- 📊 **Application Status** - Track and manage application statuses

### For Administrators
- 🎛️ **Admin Dashboard** - Centralized management interface
- 👨‍💼 **Admin Management** - Create and manage admin accounts
- 📂 **Category Management** - Create and organize job categories
- 📋 **Job Moderation** - Review and manage job postings
- 📈 **Platform Analytics** - Monitor platform activity

## 🛠️ Technologies Used

- **Backend**: PHP 7.4+ with PDO
- **Database**: MySQL/MariaDB
- **Frontend**: HTML5, CSS3, JavaScript
- **Framework**: Bootstrap 5
- **Icons**: IcoMoon, Line Icons
- **Build Tools**: SCSS

## 📋 Prerequisites

- PHP 7.4 or higher
- MySQL 5.7+ or MariaDB 10.4+
- Apache/Nginx web server
- Composer (optional for dependencies)

## 🔧 Installation Steps

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/jobboard-pro.git
cd jobboard-pro
```

### 2. Set Up the Database
1. Open phpMyAdmin or MySQL CLI
2. Create a new database named `jobboard`
3. Import the SQL file:
```bash
mysql -u root -p jobboard < SQL_FILE/jobboard.sql
```

### 3. Configure Database Connection
Edit `config/config.php` and update the database credentials:
```php
$host = "localhost";
$dbname = "jobboard";
$user = "root";
$pass = "your_password";
```

### 4. Configure Application URL
Update the `APPURL` constant in your configuration files to match your server URL.

### 5. Set Folder Permissions
Ensure the following folders are writable:
- `users/user-images/`
- `users/user-cvs/`

### 6. Start the Server
If using XAMPP:
1. Start Apache and MySQL services
2. Access the application at `http://localhost/jobboard-pro`

### 7. Login to Admin Panel
- URL: `http://localhost/jobboard-pro/admin-panel/admins/login-admins.php`
- Default Admin Email: `admin1@admin.com`
- Default Admin Password: `password123`

## 📁 Project Structure

```
jobboard-pro/
├── admin-panel/          # Admin dashboard and management
│   ├── admins/          # Admin authentication
│   ├── categories-admins/ # Category management
│   ├── jobs-admins/     # Job management
│   ├── layouts/        # Admin layout templates
│   └── styles/         # Admin-specific styles
├── auth/                # User authentication
│   ├── login.php
│   ├── logout.php
│   └── register.php
├── config/              # Configuration files
├── css/                 # Stylesheets
├── fonts/               # Icon fonts
├── gerneral/            # General pages (companies, workers)
├── images/              # Static images
├── includes/            # Shared includes (header, footer)
├── jobs/                # Job-related functionality
├── js/                  # JavaScript files
├── scss/                # SCSS source files
├── SQL_FILE/            # Database SQL files
├── users/               # User functionality
│   ├── user-cvs/       # Uploaded CVs
│   └── user-images/    # User profile images
├── index.php            # Home page
├── about.php            # About page
├── contact.php          # Contact page
└── search.php           # Job search page
```

## 🔐 Security Features

- Password hashing using PHP's `password_hash()`
- PDO prepared statements to prevent SQL injection
- Session-based authentication
- Role-based access control (Admin, Company, Worker)

## 🚀 Future Improvements

- [ ] RESTful API development
- [ ] Email notification system
- [ ] Advanced job search filters
- [ ] Resume parsing for applicants
- [ ] Payment gateway integration for featured listings
- [ ] Social media login integration
- [ ] Admin analytics dashboard with charts
- [ ] Mobile responsive improvements
- [ ] Multi-language support
- [ ] Job alerts and subscriptions

## 🤝 Contributing

Contributions are welcome! Please read our [CONTRIBUTING.md](CONTRIBUTING.md) file for details on how to contribute.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Ali Ayoub**
- GitHub: [@aliaybx](https://github.com/aliaybx)

## 🙏 Acknowledgments

- Bootstrap team for the amazing framework
- Open source icon providers (IcoMoon, Line Icons)
- All contributors who help improve this project
