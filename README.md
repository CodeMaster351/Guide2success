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
}

.nav-links a {
    color: white;
    margin: 10px;
    text-decoration: none;
}

/* MAIN BOXES */
.container {
    padding: 20px;
}

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
    <h1>Welcome to Greenfield Local Hub</h1>
    <p>Buy fresh local produce</p>
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
