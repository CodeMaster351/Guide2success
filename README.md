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



<img width="1400" height="1102" alt="image" src="https://github.com/user-attachments/assets/c52c405d-6f8b-4d66-ad0a-7825e6cb4080" />
<img width="1230" height="1454" alt="image" src="https://github.com/user-attachments/assets/86e1978e-0250-4aa2-8d6d-a63c0235914f" />
<img width="1230" height="1454" alt="image" src="https://github.com/user-attachments/assets/c818e72c-6e12-413b-b998-412a07122865" />

🚀 Feature Enhancements & Improvements
This section outlines additional features added to improve the prototype’s functionality, appearance, and overall quality. These enhancements focus on making the system appear more dynamic and user-friendly.
🔍 Product Search Function
The product search feature was implemented to allow users to filter products in real time.
✅ Implementation
Search Input (HTML):

```php
<input type="text" id="searchInput" placeholder="Search products..." onkeyup="searchProducts()">
JavaScript Function:
<script>
function searchProducts() {
  let input = document.getElementById("searchInput").value.toLowerCase();
  let items = document.getElementsByClassName("product");

  for (let i = 0; i < items.length; i++) {
    let text = items[i].innerText.toLowerCase();

    if (text.includes(input)) {
      items[i].style.display = "block";
    } else {
      items[i].style.display = "none";
    }
  }
}
</script>
```
Product Structure Requirement:
<div class="product">Apple</div>
🎯 Outcome
Products dynamically filter as the user types
Improves usability and interactivity
Enhances the overall impression of functionality
📊 Dashboard Improvements
The dashboard was enhanced to display dynamic-looking data using animated counters.
✅ Updated Dashboard Cards

```php
<div class="card">
  <h3>Items in stock</h3>
  <p id="stock">0</p>
</div>

<div class="card">
  <h3>Items out of stock</h3>
  <p id="outStock">0</p>
</div>
```

✅ Animation Script

```php
<script>
function animateValue(id, start, end, duration) {
  let range = end - start;
  let current = start;
  let increment = end > start ? 1 : -1;
  let stepTime = Math.abs(Math.floor(duration / range));
  let obj = document.getElementById(id);

  let timer = setInterval(function () {
    current += increment;
    obj.innerHTML = current;
    if (current == end) {
      clearInterval(timer);
    }
  }, stepTime);
}

animateValue("stock", 0, 120, 1000);
animateValue("outStock", 0, 15, 1000);
</script>
```
🎯 Outcome
Dashboard appears dynamic and data-driven
Improves visual professionalism
Simulates real-time updates
🎨 Visual Design Improvements (CSS)
The styling was upgraded to create a modern, clean interface without changing the structure.
✅ Key Features
Consistent colour scheme
Hover animations
Card-based layout
Responsive grid design
✅ CSS Implementation
```php
body {
  font-family: Arial, sans-serif;
  margin: 0;
  background-color: #f4f4f4;
}

/* NAVBAR */
.navbar {
  background-color: #2ecc71;
  padding: 15px;
  display: flex;
  justify-content: space-between;
}

.navbar a {
  color: white;
  margin: 0 10px;
  text-decoration: none;
  font-weight: bold;
}

.navbar a:hover {
  color: #000;
}

/* CARDS */
.card {
  background: white;
  padding: 20px;
  margin: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  text-align: center;
  transition: transform 0.2s;
}

.card:hover {
  transform: scale(1.05);
}

/* PRODUCT GRID */
.products {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 15px;
  padding: 20px;
}

.product {
  background: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s;
}

.product:hover {
  transform: scale(1.05);
}

/* BUTTONS */
button {
  background: #2ecc71;
  color: white;
  border: none;
  padding: 10px;
  margin-top: 10px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background: #27ae60;
}

/* FOOTER */
.footer {
  background: #2ecc71;
  color: white;
  text-align: center;
  padding: 15px;
  margin-top: 20px;
}
```
🎯 Outcome
Cleaner, more modern UI
Improved user engagement
Consistent design across pages
🛒 Add to Basket Feature
A simple interactive feature was added to simulate adding items to a basket.
✅ Implementation
```php
<button onclick="addToBasket()">Add to Basket</button>

<script>
function addToBasket() {
  alert("Item added to basket");
}
</script>
```
🎯 Outcome
Provides user feedback
Makes the system feel interactive
Enhances perceived functionality
📈 Additional Page Content
📦 Orders Page
A basic table was added to display order information.
```php
<table>
  <tr>
    <th>Order ID</th>
    <th>Status</th>
  </tr>
  <tr>
    <td>#001</td>
    <td>Pending</td>
  </tr>
</table>
```
🏠 Homepage Content
Additional sections were added to improve page depth and scrolling experience.
```php
<section>
  <h2>Featured Products</h2>
  <p>Check out our best sellers</p>
</section>

<section>
  <h2>About Us</h2>
  <p>We support local producers and sustainable products.</p>
</section>
```
🎯 Outcome
Pages feel more complete
Improves user engagement
Demonstrates content structure
🧠 Key Insight
The goal of this prototype is not to create a fully functional system, but to develop a convincing and realistic representation of a working application.
✅ Summary
Focus on usability and design
Simulate functionality where necessary
Demonstrate technical understanding
Present a professional and complete system



