# Contributing to JobBoard

Thank you for your interest in contributing to JobBoard! This document outlines the process for contributing to this project.

> ⚠️ **Security Notice**: This project has known security vulnerabilities. Please read the Security Considerations section before contributing.

## Table of Contents

- [Code of Conduct](#code-of-cconduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Process](#development-process)
- [Security Considerations](#security-considerations)
- [Pull Request Guidelines](#pull-request-guidelines)
- [Coding Standards](#coding-standards)
- [Documentation](#documentation)

---

## Code of Conduct

By participating in this project, you are expected to uphold our Code of Conduct:

- Be respectful and inclusive
- Welcome newcomers and help them learn
- Accept constructive criticism professionally
- Focus on what is best for the community

## Getting Started

### Prerequisites

Before contributing, ensure you have:

1. **Git** installed
2. **PHP 7.4+** installed
3. **MySQL/MariaDB** installed
4. **XAMPP/WAMP/LAMP** for local development
5. **Code editor** (VS Code recommended)

### Setting Up Development Environment

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/aliaybx/Jobboard-System.git
   cd Job-Portal-in-PHP-MySQL-PDO-and-Bootstrap
   ```

3. Set up the database:
   - Start Apache and MySQL in XAMPP
   - Create a database named `jobboard`
   - Import `SQL_FILE/jobboard.sql`

4. Configure the application:
   - Edit `config/config.php` with your database credentials
   - Update `includes/header.php` with your local URL

5. Test the setup:
   - Visit http://localhost/your-path
   - Verify login works with: admin1@admin.com / admin123

---

## How to Contribute

### Reporting Bugs

If you find a bug, please create an issue with:

1. **Clear title** describing the problem
2. **Steps to reproduce** the bug
3. **Expected behavior** vs actual behavior
4. **Screenshots** if applicable
5. **Environment details** (OS, PHP version, etc.)

### Suggesting Features

For feature requests:

1. Check existing issues first
2. Describe the feature clearly
3. Explain why this feature would be useful
4. Provide examples of usage

### Pull Requests

We welcome pull requests! Here's how to proceed:

1. Create a branch from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/bug-description
   ```

2. Make your changes

3. Test your changes locally

4. Commit with clear messages:
   ```bash
   git commit -m "Add: description of your changes"
   ```

5. Push to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

6. Create a Pull Request

---

## Development Process

### Priority Areas for Contribution

Given the current state of the codebase, we prioritize contributions in these areas:

### 🔴 High Priority - Security Fixes

1. **SQL Injection Fixes**
   - Replace direct queries with prepared statements
   - Files: All PHP files in `auth/`, `users/`, `jobs/`, `admin-panel/`

2. **XSS Protection**
   - Add `htmlspecialchars()` to all output
   - Sanitize user inputs

3. **CSRF Protection**
   - Implement CSRF tokens for all forms

4. **Session Security**
   - Add secure session configuration
   - Implement session timeout

### 🟡 Medium Priority - Code Quality

1. **Input Validation**
   - Add server-side validation
   - Create validation helper functions

2. **Error Handling**
   - Replace JS alerts with proper error handling
   - Add logging

3. **Code Organization**
   - Create helper functions (`functions.php`)
   - Reduce code duplication

### 🟢 Low Priority - Features

1. Add pagination
2. Implement caching
3. Add unit tests

---

## Security Considerations

> ⚠️ **CRITICAL**: This codebase has known security vulnerabilities. Before contributing:

1. **DO NOT** make changes that introduce new vulnerabilities
2. **DO** follow OWASP security guidelines
3. **DO** use prepared statements for all database queries
4. **DO** sanitize all user inputs and outputs
5. **DO** validate file uploads strictly

### Security Checklist for PRs

- [ ] All database queries use prepared statements
- [ ] All user outputs are escaped with `htmlspecialchars()`
- [ ] Forms include CSRF tokens
- [ ] File uploads are validated (type, size)
- [ ] No sensitive data in error messages
- [ ] Sessions are configured securely

---

## Pull Request Guidelines

### PR Title Format

Use conventional commits style:

- `Fix: description` - Bug fixes
- `Feat: description` - New features
- `Refactor: description` - Code refactoring
- `Docs: description` - Documentation
- `Security: description` - Security fixes

### PR Description Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Security fix
- [ ] Documentation update

## Testing
Describe testing performed

## Security Checklist
- [ ] No SQL injection vulnerabilities
- [ ] XSS protection implemented
- [ ] CSRF tokens added
- [ ] Input validation added
```

### What Happens After PR

1. Code review by maintainers
2. Security review (critical for this project)
3. Testing verification
4. Merge or feedback

---

## Coding Standards

### PHP Standards

- Use PSR-12 coding style
- Use meaningful variable names
- Comment complex logic
- Keep functions focused and small

### Example: Database Queries

```php
// ❌ BAD - SQL Injection vulnerable
$email = $_POST['email'];
$query = $conn->query("SELECT * FROM users WHERE email = '$email'");

// ✅ GOOD - Prepared statement
$email = $_POST['email'];
$query = $conn->prepare("SELECT * FROM users WHERE email = :email");
$query->execute([':email' => $email]);
```

### Example: Output

```php
// ❌ BAD - XSS vulnerable
echo $user->username;

// ✅ GOOD - Escaped output
echo htmlspecialchars($user->username, ENT_QUOTES, 'UTF-8');
```

### Example: File Uploads

```php
// ❌ BAD - No validation
$target = 'uploads/' . basename($_FILES['file']['name']);
move_uploaded_file($_FILES['file']['tmp_name'], $target);

// ✅ GOOD - With validation
$allowed = ['image/jpeg', 'image/png'];
$maxSize = 2 * 1024 * 1024;

if (!in_array($_FILES['file']['type'], $allowed)) {
    die("Invalid file type");
}
if ($_FILES['file']['size'] > $maxSize) {
    die("File too large");
}

$filename = basename($_FILES['file']['name']);
$target = 'uploads/' . $filename;
move_uploaded_file($_FILES['file']['tmp_name'], $target);
```

---

## Documentation

### Updating README

If your changes affect:

- Installation process → Update README
- New features → Update feature list
- Security changes → Update security section
- API changes → Update documentation

### Code Comments

- Comment complex business logic
- Document security-sensitive code
- Keep comments up to date with code

---

## Questions?

If you have questions:

- Open an issue for bugs/features
- Check existing issues before creating new ones
- Be patient - this is a community project

---

## Recognition

Contributors will be recognized in:
- README.md contributors section
- GitHub contributors graph

Thank you for contributing to JobBoard!
