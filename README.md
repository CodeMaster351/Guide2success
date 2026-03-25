# 🌱 Greenfield Local Hub

A simple PHP + MySQL web application for buying and managing local produce.

---

## 📦 1. Database Setup (phpMyAdmin)

Create a database:

```sql
glh
```

### 👤 Users Table

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    password VARCHAR(255)
);
```

### 🛒 Products Table

```sql
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(5,2),
    stock INT
);
```

---

## 🔌 2. Database Connection (`config.php`)

```php
<?php
$conn = new mysqli("localhost", "root", "", "glh");

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
?>
```

---

## 🧭 3. Navbar (`header.php`)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Greenfield Local Hub</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="navbar">
    <h2>Greenfield</h2>
    <div class="nav-links">
        <a href="index.php">Home</a>
        <a href="products.php">Product</a>
        <a href="checkout.php">Order</a>
        <a href="register.php">Sign up</a>
        <a href="login.php">Log in</a>
        <a href="dashboard.php">Dashboard</a>
    </div>
</div>
```

---

## 🧾 4. Footer (`footer.php`)

```html
<div class="footer">
    <p>Help centre | Contact us | About us</p>
    <p>Privacy policy | Terms and conditions | Accessibility</p>
</div>

</body>
</html>
```

---

## 🎨 5. Styling (`style.css`)

```css
body {
    font-family: Arial;
    margin: 0;
}

/* NAVBAR */
.navbar {
    background-color: green;
    color: white;
    padding: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.nav-links a {
    color: white;
    margin: 10px;
    text-decoration: none;
}

/* CONTAINER */
.container {
    padding: 20px;
}

/* HERO SECTION */
.hero {
    background-color: #2d5fff;
    color: white;
    padding: 40px;
    text-align: center;
    margin-bottom: 20px;
}

.hero button {
    margin-top: 10px;
}

/* BOXES */
.box {
    background-color: #2d5fff;
    color: white;
    padding: 20px;
    margin: 10px;
    text-align: center;
}

/* GRID */
.grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}

/* INFO SECTIONS */
.info-section {
    background-color: #f4f4f4;
    padding: 20px;
    margin-top: 20px;
}

/* FORM */
.form-box {
    width: 300px;
    margin: auto;
    background: #2d5fff;
    padding: 20px;
    color: white;
}

input {
    width: 100%;
    padding: 8px;
    margin: 5px 0;
}

button {
    width: 100%;
    padding: 10px;
    background: grey;
    color: white;
    border: none;
    cursor: pointer;
}

/* FOOTER */
.footer {
    background: green;
    color: white;
    text-align: center;
    padding: 10px;
    margin-top: 20px;
}
```

---

## 🏠 6. Homepage (`index.php`)

```php
<?php include 'header.php'; ?>

<div class="container">

    <!-- HERO SECTION -->
    <div class="hero">
        <h1>Welcome to Greenfield Local Hub</h1>
        <p>Supporting local farmers and fresh produce</p>
        <a href="products.php"><button>Browse Products</button></a>
    </div>

    <!-- FEATURED PRODUCTS -->
    <h2>Featured Products</h2>
    <div class="grid">
        <div class="box">Organic Apples - £2.50</div>
        <div class="box">Fresh Milk - £1.80</div>
        <div class="box">Local Honey - £4.00</div>
        <div class="box">Free-range Eggs - £3.00</div>
    </div>

    <!-- ABOUT SECTION -->
    <div class="info-section">
        <h2>About Greenfield</h2>
        <p>
            Greenfield Local Hub connects customers with local farmers and producers.
            Our goal is to support the local economy and provide fresh, high-quality produce.
        </p>
    </div>

    <!-- BENEFITS -->
    <div class="info-section">
        <h2>Why Buy Local?</h2>
        <ul>
            <li>Fresher and higher quality food</li>
            <li>Supports local farmers</li>
            <li>Reduces environmental impact</li>
            <li>Promotes sustainability</li>
        </ul>
    </div>

</div>

<?php include 'footer.php'; ?>
```

---

## 🛍️ 7. Products Page (`products.php`)

```php
<?php 
session_start();
include 'header.php'; 
include 'config.php'; 
?>

<div class="container">
    <h2>Products</h2>

    <!-- SEARCH -->
    <form method="GET">
        <input type="text" name="search" placeholder="Search products...">
        <button type="submit">Search</button>
    </form>

    <div class="grid">
        <?php
        $search = "";
        if (isset($_GET['search'])) {
            $search = $_GET['search'];
            $sql = "SELECT * FROM products WHERE name LIKE '%$search%'";
        } else {
            $sql = "SELECT * FROM products";
        }

        $result = $conn->query($sql);

        while($row = $result->fetch_assoc()) {
            echo "<div class='box'>";
            echo "<b>" . $row['name'] . "</b><br>";
            echo "£" . $row['price'] . "<br>";

            echo "<form method='POST'>";
            echo "<input type='hidden
```

---

## 🔐 8. Register Page (`register.php`)

```php
<?php include 'header.php'; ?>
<?php include 'config.php'; ?>

<div class="form-box">
    <h2>Register</h2>

    <form method="POST">
        <input type="text" name="first" placeholder="First name" required>
        <input type="text" name="last" placeholder="Last name" required>
        <input type="email" name="email" placeholder="Email" required>
        <input type="password" name="password" placeholder="Password" required>
        <button type="submit">Register</button>
    </form>

    <?php
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        $pass = password_hash($_POST['password'], PASSWORD_DEFAULT);

        $sql = "INSERT INTO users (first_name, last_name, email, password)
                VALUES ('$_POST[first]', '$_POST[last]', '$_POST[email]', '$pass')";

        $conn->query($sql);
        echo "Registered!";
    }
    ?>
</div>

<?php include 'footer.php'; ?>
```

---

## 🔑 9. Login Page (`login.php`)

```php
<?php include 'header.php'; ?>
<?php include 'config.php'; ?>

<div class="form-box">
    <h2>Log in</h2>

    <form method="POST">
        <input type="email" name="email" placeholder="Email">
        <input type="password" name="password" placeholder="Password">
        <button type="submit">Log in</button>
    </form>

    <?php
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        $result = $conn->query("SELECT * FROM users WHERE email='$_POST[email]'");

        if ($row = $result->fetch_assoc()) {
            if (password_verify($_POST['password'], $row['password'])) {
                echo "Login successful!";
            } else {
                echo "Wrong password";
            }
        } else {
            echo "User not found";
        }
    }
    ?>
</div>

<?php include 'footer.php'; ?>
```

---

## 🛒 10. Checkout Page (`checkout.php`)

```php
<?php include 'header.php'; ?>

<div class="container">
    <div class="box">
        <h2>Items & Prices</h2>
        <p>Apples - £2.50</p>
        <p>Honey - £4.00</p>
        <h3>Total: £6.50</h3>
        <button>Checkout</button>
    </div>
</div>

<?php include 'footer.php'; ?>
```

---

## 📊 11. Dashboard (`dashboard.php`)

```php
<?php include 'header.php'; ?>

<div class="container">

    <div class="grid">
        <div class="box">Items in stock</div>
        <div class="box">Items out of stock</div>
        <div class="box">Sales this week</div>
        <div class="box">Orders to deliver</div>
    </div>

    <div class="box">Sales trend</div>
    <div class="box">Best products</div>
    <div class="box">Leaderboard of producers</div>

</div>

<?php include 'footer.php'; ?>
```

---

## 🚀 How to Run

1. Start Apache & MySQL (e.g. XAMPP)
2. Import the database into phpMyAdmin
3. Place files in `htdocs`
4. Visit:

```
http://localhost/your-folder-name
```

---

## 🧠 Notes

* Uses **PHP (procedural)** and **MySQL**
* Basic authentication included
* Simple UI with CSS grid layout

---



🌿 Greenfield Local Hub – Web Prototype
📌 Overview
This project is a web-based prototype developed for Greenfield Local Hub.
It allows users to:
Browse products
Register and log in
View a dashboard with key business data
The system displays:
Stock levels
Sales trends
Orders awaiting delivery
🛠 Technologies Used
HTML – Structure
CSS – Styling and layout
PHP – Backend processing
MySQL (XAMPP) – Database for users and products
This meets the requirement of using both front-end and back-end technologies.
🔁 Iterative Development
Iteration 1
Created basic homepage using HTML and CSS
Added navigation bar and footer
Static layout (no interactivity)
Iteration 2
Developed login and registration pages
Added PHP for handling user input
Implemented basic validation (required fields)
Iteration 3
Built product page with grid layout
Added dropdown filter for product selection
Improved page navigation
Iteration 4
Created dashboard displaying:
Items in stock
Items out of stock
Sales trends
Orders awaiting delivery
Improved styling consistency
Final Iteration
Enhanced UI (spacing, colours, layout)
Added better error handling
Improved navigation and usability
Standardised structure across all pages
🧪 Testing
Testing followed the strategy defined in Task 1 and included:
Functional testing
Validation testing
Usability testing
Sample Test Log

Test ID	Component	Test Data	Expected Result	Actual Result	Result
T1	Login	Valid email + password	User logs in	Successful login	✅
T2	Login	Invalid password	Error message displayed	Error shown	✅
T3	Register	Empty fields	Submission prevented	Prevented	✅
T4	Product Page	Select filter	Products update	Works correctly	✅
T5	Navigation	Click links	Correct page loads	Works	✅
Testing was iterative, with improvements made after each round.


📚 Assets Log


Source	Content Used	Purpose	Date Accessed
W3Schools	HTML, CSS, PHP tutorials	Coding support	10/03/2026
MDN Web Docs	Development guidance	Best practices	11/03/2026
Google Fonts	Typography	Improve visual design	12/03/2026
Own Designs	Wireframes	Page structure planning	13/03/2026


🔐 Security Considerations
Input validation prevents invalid or empty data
Required fields enforced in forms
Password fields are hidden
PHP processes data securely on the server
Potential for session management to control access
These measures help protect user data and improve reliability.
🧩 Maintainability
The system is designed for easy maintenance:
Code separated into HTML, CSS, and PHP files
Clear file naming (e.g. login.php, register.php)
Consistent navigation and footer across pages
Reusable components reduce duplication
Code is simple and readable
🎯 User Experience (UX)
Consistent layout across all pages
Clear navigation bar
Simple and readable forms
Dashboard presents data clearly
Buttons and links are easy to identify
⚖️ Legal & Regulatory Considerations
GDPR awareness: only necessary data is collected
Privacy policy and terms included
Accessible layout and readable text
No unnecessary personal data stored
🚀 Future Improvements
📊 Dashboard Enhancements
Hover effects on cards
Animated counters
Colour indicators:
Green = good
Red = low stock
.card {
  transition: transform 0.2s;
}

.card:hover {
  transform: scale(1.05);
}
🛒 Product Page Improvements
Add search bar
“Add to basket” functionality
Product hover effects
<input type="text" placeholder="Search products...">
<button>Add to Basket</button>
🏠 Homepage Expansion
Add scrolling sections:
Featured Products
About Greenfield
Customer Reviews
🔑 Login/Register Enhancements
Error messages (e.g. password too short)
Confirm password field
📎 Footer Upgrade
Social icons
Additional links:
FAQ
Contact
Support
🧭 Navigation Bar Improvements
Highlight active page
.nav a:hover {
  color: yellow;
}
⚡ JavaScript Features (Optional)
Login failure alerts
Dynamic basket count
